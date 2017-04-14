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
    
    
    
    
    
    
var options = {
    key: fs.readFileSync('/home/certificates.key'),
    cert: fs.readFileSync('/home/certificates.crt'),
    requestCert: true
};
var server = require('https').createServer(options, app);
var io = require('socket.io').listen(server);
server.listen(8000);
console.log('Server started at port: 8000');

var socket = io.connect('example.com:8000', {secure: true});




```




