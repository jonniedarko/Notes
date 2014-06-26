#AWS Enviorment set up for Maker 2014

As part of our Maker 2014 project we decided to utilise an AWS server [Martin]() had signed up to. While we will probably end up using a local server on the day we thought it was a good opertunity to give AWS a test run. As well as giving us a way to share an enviorment while working as a distrubuted team, the process of setting up the AWS will server to Document the setup we require for our project. The Aim would be to later create a script that does all the installs and setups for us.

**Note: at the time of writing, this is a work in progress and I hope to keep it updated**

So what do we need to install & setup?
 - [Before we start](#beforestart)
 - [Node.js](#nodesetup)
 - [Mongodb](#Mongodbsetup)

<a name="beforestart"></a>

sudo apt-get upgrade 
sudo apt-get update


<a name="nodesetup"></a>
#Node.js Setup
###These are needed for building and running Node
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
<a name="Mongodbsetup"></a>
##Mongodb
###Issue the following command to import the MongoDB public GPG Key:
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
###Create the /etc/apt/sources.list.d/mongodb.list list file using the following command:
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
