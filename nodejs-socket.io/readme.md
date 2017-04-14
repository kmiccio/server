# Install NodeJS & Socket.io<br>on DigitalOcean.com

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
  -sudo apt-get install -y nodejs
  -sudo apt-get install -y build-essential
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
  -From another terminal window: telnet droplet_ip 5000, if everything goes alright, you should see a welcome message, now type something to see it in the console of your nodejs server.
  -sudo ufw allow 5000/tcp
  -sudo ufw enable
  -Test Again.
```
