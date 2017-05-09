Password MySQL:
	- nano /root/.digitalocean_password // Obtiene password de Mysql

Actualizar:
	- sudo apt-get update

Instalar phpmyadmin:
	- sudo apt-get install phpmyadmin
	- Password administrador esta en: /root/.digitalocean_password  
	- Password user phpmyadmin -> random pass next 

	al finalizar la instalación de phpmyadmin
	- sudo php5enmod mcrypt
	- sudo service apache2 restart
		*en caso de no seleccionar Apache2 ( no requerido )
		- sudo ln -s /usr/share/phpmyadmin/ /var/www/html/phpmyadmin
	- phpmyadmin queda en: http://domain_name_or_IP/phpmyadmin
