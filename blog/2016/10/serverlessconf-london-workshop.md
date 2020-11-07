Serverlessconf, London 2016 workshop:
===========================================================================
**Author:** Alan Mills  
**Date:** [24 October 17:49](/blog/history/2016-10.md)  
**Tags:** [Data Lake](/blog/categories/data-lake.md)
**Status**: Draft

* Flo, CTO of Serverlessconf
* Philipp, Framework Core Team at
* Rob, Nordstrom Serverless Technology Team.  Nordstrom have been building a solution using Serverless
* Andy
* Erik
* Greg Smith


Serverless Framework
--------------------
Serverless (1.0.3)
https://github.com/ServerlessInc/serverlessconf-workshop.git

### Introduction
Servers are still there but you do not have to think (too much) about them.

Key things:
* Goal is to help us build Serverless Micro-Services.
* Event Driven Infrastructure

![Image Upload and tracking service]()

### Serverless.yml
Main configuraiton is **Serverless.yml**
![Key Components]()

### Serverless framework
Uses CloudFormation under the hood.
Everything is a Plugin service
You can extend Serverless through your own plugins

Pre-requisites
--------------
* You need an AWS account
* Setup an account in IAM
* Download the Credentials
* Add permissions to the account, for demo purposes this can be **AdministratorAccess** clearly bad practice for the real world.


Workshop coding
---------------
# Prepare workshop folder
mkdir workshop
cd workshop

# Prepare Docker containers
docker-compose build serverless-workshop
docker-compose run serverless-workshop bash

# Create the Serverless service
serverless create -t aws-nodejs -n serverless-workshop


# Setup the AWS Credentials
https://serverless.com/framework/docs/providers/aws/guide/credentials/
Get the keys from the downloaded credentials file.
export AWS_ACCESS_KEY_ID=<key>
export AWS_SECRET_ACCESS_KEY=<secret>

# Deploy with verbose mode
serverless deploy -v

# Invoke
serverless invoke -f hello


Change the hello


Nordstrom
---------
Artillery.io
https://github.com/nordstrom/serverless-artillery-workshop

![How does it work?]()
There is a YML file that kicks
Each lambda is generating 25 requests per second.
InfluxDB+Grafana is giving more insights
There is a limit on lambda and using Serverless-artillery can pin this and then the other lambdas in your AWS account they will go into a 'hold state'.  Therefore, you probably don't want to do this with your production AWS account.

https://github.com/Nordstrom/serverless-artillery-workshop/tree/master/Lesson1:%20%22Hello%2C%20artillery.%22
npm install -g serverless-artillery

export AWS_ACCESS_KEY_ID=<access-key-id>
export AWS_SECRET_ACCESS_KEY=<secret-access-key>
### Battle test Artillery


there is a default of 100 simultaneous lambda invokations per account, per region.  However, you can request to get this increased.


### InfluxDB
The Nordstrom guys have setup a server for this.  When we use this at another time we will need to setup our own InfluxDB instance.

http://ec2-54-161-98-139.compute-1.amazonaws.com:8083
Select database: artillery_metrics
Query: select * from latency where testName = 'alan-test' limit 10
http://ec2-54-161-98-139.compute-1.amazonaws.com:3000/login
