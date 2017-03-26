# Install Apache Reverse Proxy with Letsencrypt SSL<br>on DigitalOcean.com

### Digital Ocean Droplet<br>
```js
  -Enter DigitalOcean
  -Create Droplet
  -One-click apps
  -LAMP 14.04
  -5/mo
  -NY 3
  -hostname: Proxy1
```
### Digital Ocean Droplet<br>
```js
  -Networking:
  -Domains
  -Create A record to Droplet_IP
  -xxx.domain.com -> A -> Droplet_IP
  
  
  -Get your pass, from email.
  -Terminal: shh root@Droplet_IP
  -Change Password
  -sudo add-apt-repository ppa:certbot/certbot
  -sudo apt-get update
  -sudo apt-get install python-certbot-apache 
    -Y
  -certbot --apache
  -On new Terminal Test ping before: ping xxx.domain.com should see the Droplet IP.
  -enter your domain name: xxx.domain.com
  -Your email
  -A
  -N
  -1
  -If everything goes all right: search for Congratulations! 
  -Certificates are here: /etc/letsencrypt/live/xxx.domain.com/
  
  -Check your https://xxx.domain.com
  -Everything should go fine until this point.
```
### Reverse Proxy server<br>
```js
Create a second test server, we will create a node.js server
  -Create Droplet
  -LAMP 14.04.05 x64
  -5/mo
  -NY 3
  -hostname: Node1
  
  -Get your pass, from email.
  -Terminal: shh root@Droplet_IP
  -Change Password
  
  -curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
  -sudo apt-get install -y build-essential
  -sudo apt-get install -y nodejs
  -sudo mkdir node
  -cd node
  -sudo nano server.js
  Copy this:
  
  
  
  
   


  
  
  
```js
HELLO
```
