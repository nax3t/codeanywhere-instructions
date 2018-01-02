# CodeAnywhere Instructions

- Sign up for a free account at [https://codeanywhere.com/signup](https://codeanywhere.com/signup)
- Create a new project and name it aything
- Open the project
- When prompted to create a container
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


