sudo apt-get upgrade 
sudo apt-get update
#These are needed for building and running Node
sudo apt-get install build-essential lamp-server^

sudo useradd -m -s /bin/bash fmaster
sudo passwd fmaster
#fmasterbedlabaws

#Node
wget http://nodejs.org/dist/v0.10.22/node-v0.10.22.tar.gz
tar -xvzf node-v0.10.22.tar.gz
cd node-v0.10.22

./configure
make
sudo make install

#test Node installed correctly
node -v
npm version

#Mongodb
#Issue the following command to import the MongoDB public GPG Key:
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
#Create the /etc/apt/sources.list.d/mongodb.list list file using the following command:
echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | sudo tee /etc/apt/sources.list.d/mongodb.list

#Reload local package database
sudo apt-get update
# install mongoDB
# create /data/db
# change permissions
sudo chown -R username.groupname /data/db

- Sources
http://askubuntu.com/questions/328681/installing-the-latest-node-js-mongodb
http://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/
http://stackoverflow.com/questions/5300861/mongodb-only-works-when-run-as-root-on-ubuntu-data-directory-issue
