# Letsencrypt auto renew

### Auto renew Certificates
```js

  -sudo crontab -e
  -Select 2
	- 0 * * * * ./opt/letsencrypt/certbot-auto renew --dry-run --quiet --no-self-upgrade
  -save: ctrl + x / y / Enter
  
  Congrats, you don't need to worry about certificates anymore, they will auto renew.
  happy coding!
  
```
