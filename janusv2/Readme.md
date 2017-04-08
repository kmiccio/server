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
### Install Janus WebRTC Gateway on DigitalOcean.com<br>
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
  // CHANGES: 
  -git checkout v2.0-stable
  -cmake -DCMAKE_INSTALL_PREFIX:PATH=/usr -DCMAKE_C_FLAGS="-fpic" ..
  to: -cmake -DCMAKE_INSTALL_PREFIX:PATH=/usr -DCMAKE_C_FLAGS="-fpic" -DLWS_SERVER_OPTION_DO_SSL_GLOBAL_INIT=ON ..
		   
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
  
  	= CREATE DROPLET IMAGE -> BASE
  
  ### TEST & START JANUS ###
  -cd /opt/janus/etc/janus/ 
  
  
  
  
  
  
  -sudo nano janus.transport.http.cfg
    -chage this items:
    // ###################################
      	    https = yes  // no => yes
	    secure_port = 8089  // Delete ;
	    admin_http = yes // no => yes                         
	    admin_port = 7088  // Delete ;
	    admin_https = yes  // no => yes
	    admin_secure_port = 7889 // Delete ;
    // ###################################
  -save file / ctrl + x / y / enter
  
 
  -sudo ufw disable
 
  ### START JANUS ###
  -/opt/janus/bin/janus
  
  -Open your Browser and Check:
  	-http://Droplet_IP:8088/janus/info
	-https://Droplet_IP:8089/janus/info
	-You should see a json response
	
  -sudo ufw enable
  -sudo ufw status
  	-Status: active -> OK
  -Open your Browser and Check again:
  	-http://Droplet_IP:8088/janus/info
	-https://Droplet_IP:8089/janus/info
	-You should see a json response
	
  -At this point everything is running and Janus was successfully installed
  -Create Snapshot "Base-Janus"
  
```
