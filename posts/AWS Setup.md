#AWS Enviorment set up for Maker 2014

As part of our Maker 2014 project we decided to utilise an AWS server [Martin]() had signed up to. While we will probably end up using a local server on the day we thought it was a good opertunity to give AWS a test run. As well as giving us a way to share an enviorment while working as a distrubuted team, the process of setting up the AWS will server to Document the setup we require for our project. The Aim would be to later create a script that does all the installs and setups for us.

**Note: at the time of writing, this is a work in progress and I hope to keep it updated**

So what do we need to install & setup?
 - [Before we start](#beforestart)
 - [Node.js](#nodesetup)
 - [Mongodb](#Mongodbsetup)

<a name="beforestart"></a>
###Before we start
Lets make sure everything is uptdate on our system using `apt-get`:

```bash
   sudo apt-get upgrade 
   sudo apt-get update
```


<a name="nodesetup"></a>
#Node.js Setup
What is Node.js? It is a Server-side implementation of the Javascript platform which allows repaid development of applications.This is particularly true if you are using complete Javascript stack such as [MEAN]()(MongoDB, Express.js,Angular.js & Node.js) as development can be more consistent and be designed within the same system. This make rapid prototyping and proof of concept that much quicker.

![nodejs.org](https://cloud.githubusercontent.com/assets/3673943/3397003/7ab4c020-fd17-11e3-8c71-5f972dafdeba.jpg)

From my research online I have found that it is suggested building Node.js from source as packages in the Advanced Packaging Tool (AptGet) do not work always or are outdated at times on Ubuntu. So the first thing we need to do is install the some dependiences required for building Node.js
 - [build-essential](http://packages.ubuntu.com/lucid/build-essential) which is used to build Debian packages
 - [lamp-server](https://help.ubuntu.com/community/ApacheMySQLPHP) which installs Apache, MySQL & PHP for linux

```bash
   sudo apt-get install build-essential lamp-server^
```
Next we need to get the node packages to build, I went with v0.10.22 as I knew it was a stable release but if you wish to go for a newer version check out the [Node.js dist](http://nodejs.org/dist/) site for the verion number of your choice

```bash
   #Get Node package
   wget http://nodejs.org/dist/v0.10.22/node-v0.10.22.tar.gz
   #unarchive it
   tar -xvzf node-v0.10.22.tar.gz
```   
Next we need to mov the the node directory, run the configuration script, build it and then install it
```bash
  #Move into directory
  cd node-v0.10.22
  #Run the configuration script
  ./configure
  #Build it
  make
  #Install Node
  sudo make install
```

The Configuration script just sets up what ever enviroment variables required and performs checks required to build Node for our platform

The `make` utility is to determine automatically which pieces of Node.js needs to be recompiled, and issues the commands to recompile them. 

Nod should be all set Now so lets test by checking the version
```bash
   node -v
   #Should output something like the following depending on the version you installed: v0.10.22
   
   npm version
   # should out put something like
   # { http_parser: '1.0',
   #   node: '0.10.22',
   #   v8: '3.14.5.9',
   #   ares: '1.9.0-DEV',
   #   uv: '0.10.19',
   #   zlib: '1.2.3',
   #   modules: '11',
   #   openssl: '1.0.1e',
   #   npm: '1.3.14' }
```
   
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
