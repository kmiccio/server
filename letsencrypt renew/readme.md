# Letsencrypt auto renew

### Auto renew Certificates
```js

  -sudo crontab -e
  -Select 2
	- 0 * * * * ./opt/letsencrypt/certbot-auto renew --dry-run --quiet --no-self-upgrade
  -save: ctrl + x / y / Enter
  
  Congrats, you dont need to worry about certificates anymore, they will auto renew.
  happy coding!
  
```
### Auto renew manual
```js

  -cd /opt
  -cd /letsencrypt
  ./letsencrypt-auto certonly --standalone --email name@domain -d name.domain.com

 
  Congrats, you dont need to worry about certificates anymore, they will auto renew.
  happy coding!
  
```
### Get Expirate day of SSL certicate
```js
  -Get the days
  -apt-get install ssl-cert-check
  -ssl-cert-check -c /etc/letsencrypt/live/your.domain.xxx/cert.pem
```
