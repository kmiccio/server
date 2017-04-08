# Install Janus WebRTC Gateway<br>on DigitalOcean.com

### Digital Ocean Droplet<br>
```js
  -Enter DigitalOcean
  -Create Droplet
  -Ubuntu 14.04.5 x64
  -20/mo
  -NY 3
  -hostname: Janus1
  
```
### Install Janus WebRTC Gateway on DigitalOcean.com - PART1<br>
```js
  -Networking:
  -Domains
  -Create A record to Droplet_IP
  -xxx.domain.com -> A -> Droplet_IP
  
  -Get your pass, from email.
  -Terminal: shh root@Droplet_IP
  -Change Password
  
  -sudo apt-get update
  -sudo apt-get install aptitude
  -sudo aptitude install libmicrohttpd-dev libjansson-dev libnice-dev libssl-dev libsrtp-dev libsofia-sip-ua-dev libglib2.0-dev libopus-dev libogg-dev libcurl4-openssl-dev pkg-config gengetopt libtool automake make git cmake
  -aptitude install -y build-essential
  -sudo apt-get install golang
  -sudo aptitude install doxygen graphviz
  -sudo apt-get install libavformat-dev
  -apt-get install -y erlang-nox erlang-dev erlang-src
  -sudo apt-get remove libsrtp0 libsrtp0-dev
    
  -cd /opt
  -wget https://www.openssl.org/source/openssl-1.0.2k.tar.gz
  -tar -xzvf openssl-1.0.2k.tar.gz
  -cd openssl-1.0.2k
  -sudo ./config
  -sudo make install
  -sudo ln -sf /usr/local/ssl/bin/openssl `which openssl`
  -openssl version -v
  
  -cd /opt/
  -wget https://github.com/cisco/libsrtp/archive/v2.0.0.tar.gz
  -tar xfv v2.0.0.tar.gz
  -cd libsrtp-2.0.0
  -./configure --prefix=/usr --enable-openssl
  -make shared_library && sudo make install
  
  -cd /opt/
  -git clone https://github.com/sctplab/usrsctp
  -cd usrsctp
  -./bootstrap
  -./configure –prefix=/usr/lib64 && make && make install
  
  -cd /opt/
  -git clone https://github.com/warmcat/libwebsockets.git
  -cd libwebsockets
  -git checkout v2.0-stable
  -mkdir build && cd build/
  -cmake -DCMAKE_INSTALL_PREFIX:PATH=/usr -DCMAKE_C_FLAGS="-fpic" -DLWS_SERVER_OPTION_DO_SSL_GLOBAL_INIT=ON ..
  -make && make install
		   
  -cd /opt
  -git clone https://github.com/meetecho/janus-gateway.git
  -cd janus-gateway
  -sh autogen.sh
  -./configure -–prefix=/opt/janus --disable-data-channels --disable-rabbitmq --disable-mqtt --disable-docs
  -make && make install
  -make configs
  
  -sudo ufw disable
 
   ### TEST & START JANUS ###
  -/opt/janus/bin/janus
  -http://Droplet_IP:8088/janus/info
  -http://Your_Domain:8088/janus/info
  -You should see a json response
  
  	=> CREATE DROPLET IMAGE -> BASE
```	
### Install Janus WebRTC Gateway on DigitalOcean.com - PART2 (SSL)<br>
```js
  -cd /opt	
  -git clone https://github.com/letsencrypt/letsencrypt	
  -cd letsencrypt
  -./letsencrypt-auto --help
  -On new Terminal Test ping before: ping xxx.domain.com should see the Droplet IP.
  -./letsencrypt-auto certonly --standalone --email your@email -d yourdomain_or_subdomain
  	-search for congratulations:
	-Search for the directory: /etc/letsencrypt/live/your_domain.com/
  -cd /etc/letsencrypt/live/your_domain.com/
  -ls -> you should see: cert.pem  chain.pem  fullchain.pem  privkey.pem
  -copy in a file this, you will need it later: 
  	-/etc/letsencrypt/live/your_domain.com/cert.pem
	-/etc/letsencrypt/live/your_domain.com/privkey.pem
  -cd /opt/janus/etc/janus/ 
  -sudo nano janus.transport.http.cfg
    -change this items:
    // ###################################
    	    http = yes
	    port = 80 	
      	    https = yes  // no => yes
	    secure_port = 443  // Delete ;
	    
	    cert_pem = /etc/letsencrypt/live/your_domain_xxx/cert.pem
            cert_key = /etc/letsencrypt/live/your_domain_xxx/privkey.pem
    // ###################################
  -save file / ctrl + x / y / enter
  
  ### START JANUS ###
  -/opt/janus/bin/janus
  
  -Open your Browser and Check:
  	-http://Droplet_IP/janus/info
	-https://Droplet_IP/janus/info
	-You should see a json response in both case, with secure SSL certificate.
	
 -Congratulation! At this point everything is running and Janus was successfully installed
  
  => CREATE DROPLET IMAGE -> BASE2

```
  ### Install Janus WebRTC Gateway on DigitalOcean.com - PART3 (CHROME FIX)<br>
```js
	
  -CHROME FAIL FIX	
  -cd /
  -git clone https://boringssl.googlesource.com/boringssl
  -cd boringssl
  -sed -i s/" -Werror"//g CMakeLists.txt
  -mkdir -p build
  -cd build
  -cmake -DCMAKE_CXX_FLAGS="-lrt" ..
  -make
  # Install
  -cd ..
  -sudo mkdir -p /opt/boringssl
  -sudo cp -R include /opt/boringssl/
  -sudo mkdir -p /opt/boringssl/lib
  -sudo cp build/ssl/libssl.a /opt/boringssl/lib/
  -sudo cp build/crypto/libcrypto.a /opt/boringssl/lib/

  -cd /opt
  -cd janus-gateway
  -sh autogen.sh
  -export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/boringssl/lib
  -./configure --enable-boringssl --disable-data-channels --disable-rabbitmq --disable-mqtt --disable-docs --prefix=/opt/janus LDFLAGS="-L/usr/local/lib -Wl,-rpath=/usr/local/lib" CFLAGS="-I/usr/local/include"
  -make clean && make install
  -make configs

  -cd /opt/janus/etc/janus/ 
  -sudo nano janus.transport.http.cfg

 -change this items:
    // ###################################
    	    http = yes
	    port = 80 	
      	    https = yes  // no => yes
	    secure_port = 443  // Delete ;

	    cert_pem = /etc/letsencrypt/live/your_domain_xxx/cert.pem
            cert_key = /etc/letsencrypt/live/your_domain_xxx/privkey.pem
    // ###################################
  -save file / ctrl + x / y / enter
  
  ### START JANUS ###
  -/opt/janus/bin/janus

  
```