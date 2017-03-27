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
  -sudo apt-get install g++
  -sudo apt-get install golang
  -sudo aptitude install doxygen graphviz
  -sudo apt-get install libavformat-dev
  -sudo apt-get remove libsrtp0 libsrtp0-dev
  -sudo apt-get autoremove libsrtp0 libsrtp0-dev
  
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
  -mkdir build && cd build/
  -cmake -DCMAKE_INSTALL_PREFIX:PATH=/usr -DCMAKE_C_FLAGS="-fpic" ..
  -make && make install
  
  -cd /opt
  -git clone https://github.com/eclipse/paho.mqtt.c.git
  -cd paho.mqtt.c
  -make && make install
  
  -cd /opt
  -git clone https://github.com/alanxz/rabbitmq-c
  -cd rabbitmq-c
  -git submodule init
  -git submodule update
  -autoreconf -i
  -apt-get install -y erlang-nox erlang-dev erlang-src
  -./configure --prefix=/usr && make && sudo make install
    - No problem if you see an error, we will disable rabbitmq later
    
  -cd /opt
  -git clone https://github.com/meetecho/janus-gateway.git
  -cd janus-gateway
  -sh autogen.sh
  -./configure –prefix=/opt/janus
  -make
  -make install
  -make configs
  -./configure –disable-rabbitmq
  -./configure –enable-docs 
  
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
  
  -sudo ufw allow ssh
  -sudo ufw allow http
  -sudo ufw allow https
  
  -sudo ufw allow 8088/tcp
  -sudo ufw allow 8088/udp

  -sudo ufw allow 8089/tcp
  -sudo ufw allow 8089/udp

  -sudo ufw allow 8188/tcp
  -sudo ufw allow 8188/udp
  
  -sudo ufw allow 8989/tcp
  -sudo ufw allow 8989/udp

  -sudo ufw allow 7188/tcp
  -sudo ufw allow 7188/udp

  -sudo ufw allow 7989/tcp
  -sudo ufw allow 7989/udp

  -sudo ufw allow 7088/tcp
  -sudo ufw allow 7088/udp

  -sudo ufw allow 7889/tcp
  -sudo ufw allow 7889/udp

  -sudo ufw disable
  -sudo ufw status
  	-Status: inactive -> OK
	
  -/opt/janus/bin/janus
  
  -Open your Browser and Check:
  	-http://Droplet_IP:8088/janus/info
	-https://Droplet_IP:8089/janus/info
	-You should see a json response
	
  - At this point everything is running and Janus was successfully installed
  
```
### Instal Apache, SSL & Reverse Proxy to Janus ports<br>
```js
HELLO
```

