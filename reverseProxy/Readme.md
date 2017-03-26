# Install Reverse Proxy Apache with SSL with Letsencrypt<br>in DigitalOcean.com

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
  -Create a test server
   


  
  
  
```js
HELLO
```
