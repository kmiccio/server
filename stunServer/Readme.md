# Install STUNTMAN Open source STUN server<br>on DigitalOcean.com

### Digital Ocean Droplet<br>
```js
  -Enter DigitalOcean
  -Ubuntu 16.10 x64
  -5/mo
  -NY 3
  -hostname: Stun1
```
### Install STUNTMAN on Digital Ocean Droplet<br>
```js
  -Get your pass, from email.
  -Terminal: shh root@Droplet_IP
  -Change Password
  
  -sudo apt-get update
  -sudo apt-get install stuntman-server
  -stunserver ( wait 10 MIN )
  -terminal: reconnect to server -> shh root@Droplet_IP
  -test: stunclient Droplet_ip
    -Binding test: success
    
```
### STUNTMAN DNS setup on Digital Ocean Droplet<br>
```js
  -Networking:
  -Domains
  -Create A record to Droplet_IP
  -xxx.domain.com -> A -> Droplet_IP
  
```
