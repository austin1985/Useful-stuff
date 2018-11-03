# MongoDB useful commands

## Install (Ubuntu 18.04)
**Open a command line and add these commands**

`sudo apt update`

`sudo apt install -y mongodb`

## Checking the Service and Database

**First, check the service's status**

`sudo systemctl status mongodb`

**Try connecting to the database server and execute a diagnostic command**

`mongo --eval 'db.runCommand({ connectionStatus: 1 })'`

##Managing the MongoDB Service

**Verify the status of the service**

`sudo systemctl status mongodb`

**You can stop the server anytime by typing**

`sudo systemctl stop mongodb`

**Start the server when it is stopped**

`sudo systemctl start mongodb`

**Restart the server**

`sudo systemctl restart mongodb`

**By default, MongoDB is configured to start automatically with the server. If you wish to disable the automatic startup**

`sudo systemctl disable mongodb`

Enable MongoDb on startup again.

`sudo systemctl enable mongodb`


