# Codeanywhere Setup Instructions

- Sign up for a free account at [https://codeanywhere.com/signup](https://codeanywhere.com/signup)
- Go to your email and activate your account
- Click the link to go to the editor
- When prompted to create a container with the connection wizard
	- Name the container anything
	- Select the MEAN Stack container template for Ubuntu 14.04
	- Create the container and wait for it to deploy
- Once the terminal & info tabs are up and the file tree has loaded on the left hand side, you'll know the container is ready to use
- Leave the info tab open for later use and click on the terminal tab
- Inside of the terminal, paste the following script:

```
cd ; rm -rf workspace ; mkdir workspace ; cd workspace ; 
echo 'sudo service mongod stop
sudo apt-get purge -y mongodb-org*
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2930ADAE8CAF5059EE73BB4B58712A2291FA4AD5
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.6 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.6.list
sudo apt-get update
sudo apt-get install -y mongodb-org
sudo chown -R $USER /data/db
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
nvm install 8
' > setup.sh
chmod 755 setup.sh ;
./setup.sh
```

- Once the script has loaded it will stall on the last line where it says `./setup.sh`, press enter/return to run the script
- Wait for the script to finish running and the terminal to return to `cabox@box-codeanywhere:~/workspace$`
- Type `. ~/.bashrc` into the terminal and press enter/return
- Check your node and mongodb versions with: `node -v` and `mongo --version`
	- You should see node version 8.9.3 or higher and mongodb version 3.6.1 or higher, if that checks out then you're good to go
- Right click the name of your container from the left hand file menu and select 'Restart'
- Let the container restart and you should now have an empty container with the latest versions of node and mongodb installed

# Setting Up Mongod to Use the --nojournal Flag by Default

- Open the SSH terminal (right click container name and select SSH Terminal) and navigate to the ~/workspace directory (this is the default directory so you may already be there)
- Enter the following commands into the terminal: 
	- `echo "mongod --nojournal" > mongod`
	- `chmod 755 mongod`
- Now you can run `./mongod` from the ~/workspace directory and mongod will start with --nojournal
	- This means that journaling with be disabled which saves disk space in your workspace

# Testing Node and Express JS

*Note: Feel free to skip this section and return later once Colt introduces Express JS*

- Once the container is restarted type the following into the terminal:

```
touch index.js
npm init -y
```

- Now right click the container/connection name from the left hand menu and click 'Refresh'
	- If the index.js file doesn't appear (along with a package.json file) then right click and 'Restart' the container
- Double click index.js from the left hand menu to open it in the editor then paste in the following:

```
var express = require('express');
var app = express();

app.get('/', function(req, res) {
  res.send('Is this thing on?');
});

app.listen(3000, function() {
  console.log('Server listening on port 3000');
});
```

- Save the file then open the terminal tab and type in `npm i express`
- Now start the server with `node index.js`
- Right click the container name and select 'Info'
- From the info file click the link beneath where it says "To access your application over HTTPS, make sure your application is running on port 3000 and use the following link:"
- This should open a new browser tab with a page that says 'Is this thing on?' in the top left corner
- If the page loaded then node is working as expected
- Go back into your codeanywhere editor and press `ctrl + c` to shut down the node process
