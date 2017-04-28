# Install NodeJS & Socket.io on DigitalOcean.com

### Digital Ocean Droplet<br>
```js
  -Enter DigitalOcean
  -Create Droplet
  -Ubuntu 14.04.5 64bit
  -5/mo
  -NY 3
  -hostname: NodeJS
```
# Install NodeJS

```js
  -sudo apt-get update
  -curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
      // https://nodejs.org/es/download/package-manager/#debian-and-ubuntu-based-linux-distributions fro latest NodeJS version 
  -sudo apt-get install -y build-essential
  -sudo apt-get install -y nodejs
  -cd /
  -sudo mkdir node
  -sudo nano server.js
  -test your node and add this to server.js
  
      ####################
      net = require('net');
      var clients = [];

        net.createServer(function (socket) {
          socket.name = socket.remoteAddress + ":" + socket.remotePort 
          clients.push(socket);

          socket.write("Welcome " + socket.name + "\n");
          broadcast(socket.name + " joined the chat\n", socket);

          socket.on('data', function (data) {
            broadcast(socket.name + "> " + data, socket);
          });

          socket.on('end', function () {
            clients.splice(clients.indexOf(socket), 1);
            broadcast(socket.name + " left the chat.\n");
          });

          function broadcast(message, sender) {
            clients.forEach(function (client) {
              if (client === sender) return;
              client.write(message);
            });
            process.stdout.write(message)
          }

         }).listen(5000);
       console.log("Chat server running at port 5000\n");
       ####################
   
  -Save the file ctrl + x / Y / Enter
  -sudo ufw disable
  -node server.js
  -From another terminal window: telnet droplet_ip 5000, if everything goes alright, you should see a welcome message, now type something to see it in the console of your nodejs server.
  -Control + c ( exit from NodeJS )
  -sudo ufw allow 5000/tcp
  -sudo ufw enable
  -node server.js
  -Test Again.
```
# Install Socket.io

```js
  -cd /node
  -sudo npm install -g socket.io
  -sudo nano socket.js
  -test your socket.io server, add this to socket.js
      ####################
      var app = require('http').createServer(handler)
      var io = require('socket.io')(app);
      var fs = require('fs');

      app.listen(5000);

      function handler (req, res) {
        fs.readFile(__dirname + '/index.html',
        function (err, data) {
          if (err) {
            res.writeHead(500);
            return res.end('Error loading index.html');
          }

          res.writeHead(200);
          res.end(data);
        });
      }

      io.on('connection', function (socket) {
        socket.emit('news', { hello: 'Hello world' });
        socket.on('my other event', function (data) {
          console.log(data);
        });
      });
      
      ####################
  -Save the file ctrl + x / Y / Enter
  Now add an index.html
  -sudo nano index.html and add this
      ####################
      <html>
      <head>
      <script src="/socket.io/socket.io.js"></script>
      <script>
        var socket = io('http://your_droplet_ip:5000');
        socket.on('news', function (data) {
          console.log(data);
          document.body.innerHTML=data.hello;
          socket.emit('my other event', { my: 'data' });
        });
      </script>
      </head>
        <body></body>
      </html>
      ####################
    -Save the file ctrl + x / Y / Enter
    -node socket.js
    -test your NodeJS+Socket.io server: Enter to: http://your_droplet_ip:5000
    -You shuold see Hello world, from a message coming from the sokcet.io server injected in the  body tag.
      
    Congratulation! you have node and socket.io running perfectly.

```
# Running NodeJS & Socket.io with SSL

```js
  -Networking:
    -Domains
    -Create A record to Droplet_IP
    -xxx.domain.com -> A -> Droplet_IP
    -test your NodeJS+Socket.io server: Enter to: http://your.domain.xxx:5000
    -You shuold see Hello world, from a message coming from the sokcet.io server injected in the  body tag, but instead of enter directly by the Ip you reach your server by your domain. But no SSL yet...
    
  -cd /opt
  -apt-get install git
  -git clone https://github.com/letsencrypt/letsencrypt	
  -cd letsencrypt
  -./letsencrypt-auto --help
  -On new Terminal Test ping before: ping xxx.domain.com should see the Droplet IP.
  -./letsencrypt-auto certonly --standalone --email your@email -d yourdomain_or_subdomain
  	-search for congratulations:
	-Search for the directory: /etc/letsencrypt/live/your.domain.xxx/
  -cd /etc/letsencrypt/live/your.domain.xxx/
  -ls -> you should see: cert.pem  chain.pem  fullchain.pem  privkey.pem
  -copy in a file this, you will need it later: 
  	-/etc/letsencrypt/live/your.domain.xxx/cert.pem
	  -/etc/letsencrypt/live/your.domain.xxx/privkey.pem
    
  -Now we gone a make some tweak to our code, to get SSL working with socket.io, Do the following
  -cd /node
  -sudo nano socket.js and copy this
      ####################
	var fs = require('fs');

	var options = {
	    key: fs.readFileSync('/etc/letsencrypt/live/your.domain.com/privkey.pem'),
	    // cert: fs.readFileSync('/etc/letsencrypt/live/your.domain.com/cert.pem'),
	    // Fullchain is need it if you use web sockets, Full chain solve the Chrome Mobile Problem
	    cert: fs.readFileSync('/etc/letsencrypt/live/your.domain.com/fullchain.pem'),
	    requestCert: true
	};

	var app = require('https').createServer(options, handler)
	var io = require('socket.io')(app);

	app.listen(5000);

	function handler (req, res) {
	  fs.readFile(__dirname + '/index.html',
	  function (err, data) {
	    if (err) {
	      res.writeHead(500);
	      return res.end('Error loading index.html');
	    }

	    res.writeHead(200);
	    res.end(data);
	  });
	}

	io.on('connection', function (socket) {
	  socket.emit('news', { hello: 'hello world' });
	  socket.on('my other event', function (data) {
	    console.log(data);
	  });
	});
      ####################
  -Save the file ctrl + x / Y / Enter
  -sudo nano index.html and change your code to this.
      ####################
      	<html>
	<head>
	<script src="/socket.io/socket.io.js"></script>
	<script>
	  var socket = io('https://your.domain.xxx:5000', {secure: true});
	  socket.on('news', function (data) {
		console.log(data);
		document.body.innerHTML=data.hello;
		socket.emit('my other event', { my: 'data' });
	  });
	</script>
	</head>
	  <body></body>
	</html>
      ####################
  -Save the file ctrl + x / Y / Enter
  
  -Finally test your NodeJS + Socket.io server with SSL enter to: https://your.domain.xxx:5000
  -You shuold see Hello world, from a message coming from the sokcet.io server injected in the body tag but with SSL encryption.
      
  Congratulation! you have node and socket.io running perfectly and Secure.
  Happy coding!
  
```
# Socket.io service only ( NO HTML )

```js
-cd /node
-sudo nano socket.js and copy this
      ####################
	var fs = require('fs');

	var options = {
	    key: fs.readFileSync('/etc/letsencrypt/live/your.domain.com/privkey.pem'),
	    cert: fs.readFileSync('/etc/letsencrypt/live/your.domain.com/cert.pem'),
	    // Fullchain is need it if you use web sockets, Full chain solve the Chrome Mobile Problem, use fullchain intead of cert.com
	    // if not work see next chapter
	    // cert: fs.readFileSync('/etc/letsencrypt/live/your.domain.com/fullchain.pem'),
	    requestCert: true
	};

	var app = require('https').createServer(options, handler)
	var io = require('socket.io')(app);

	app.listen(5000);

	function handler (req, res) {
	    res.writeHead(200);
	    res.end();
	}

	io.on('connection', function (socket) {
	  socket.emit('news', { hello: 'hello world' });
	  socket.on('my other event', function (data) {
	    console.log(data);
	  });
	});
      ####################
  -Save the file ctrl + x / Y / Enter

```
# # Socket.io SSL Fix for Android's Chrome

```js
  -If you get a pop up asking for certificate in Chrome Mobile
  -change your SSL option inside your socket.js
  -cd /node
  -sudo nano socket.js and change this
      ####################
      	var options = {
    		key: fs.readFileSync('/etc/letsencrypt/live/your.domain.xxx/privkey.pem'),
    		cert: fs.readFileSync('/etc/letsencrypt/live/your.domain.xxx/cert.pem'),
    		ca: fs.readFileSync('/etc/letsencrypt/live/your.domain.xxx/chain.pem')
	};
      ####################
  -Your ready to go :)
```

# Daemonize your Socket.io - (RUN IT AS A SERVICE)

```js

  -apt-get install supervisor
  -check: service supervisor restart
  	-> You should see: Restarting supervisor: supervisord.
  -sudo nano /etc/supervisor/conf.d/name_of_your_service.conf
  -Copy and paste the following code:
  	-> NOTE: NO ADD ANY SPACE AT THE BIGINNING OF THE FILE.
  #####################################
	[program:name_of_your_service]
	command=node /node/name_of_your_service.js
	user=root
	autostart=true
	autorestart=true
	environment=NODE_ENV=production
	stderr_logfile=/var/log/name_of_your_service.err.log
	stdout_logfile=/var/log/name_of_your_service.out.log
  #####################################
  -Save the file ctrl + x / Y / Enter

  -supervisorctl reread
  -supervisorctl update
  
  -sudo shutdown -r now
  
  -supervisorctl
  	-status => you should see name_of_your_service -> RUNNING
  
  // OPTIONAL 
        -tail /var/log/name_of_your_service.out.log
   	-supervisorctl
   		-status
   		-tail name_of_your_service 
  		-tail name_of_your_service stderr
		-stop name_of_your_service
		-start name_of_your_service
		-restart name_of_your_service
		-quit
		
  -sudo ufw allow 9001/tcp
  -sudo nano /etc/supervisor/supervisord.conf
  -add this lines
      // ###################################
	[inet_http_server]
	port = 9001
	username = admin
	password = your_password
    // ###################################
   -Save the file ctrl + x / Y / Enter
   -service supervisor restart
   -Open your Browser and Check:
  	-http://your_domain_xxx:9001
	-Write your user/password and then You should see the supervisor status web server! COOL!
  
  -Congratulation! At this point everything is running and Socket.io service was successfully installed, secure and Daemonized
  Happy coding!

```



