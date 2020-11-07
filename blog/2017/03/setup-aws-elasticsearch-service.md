Setup AWS CloudSearch using AWS CLI
===================================
**Author:** Alan Mills  
**Date:** [22 March 2017 15:50]
**Tags:** [AWS], [CLI], [ElasticSearch]
**Status**: Draft

[Getting started with Amazon Elasticsearch Service Domains](http://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-gsg.html)

### Setup ElasticSearch Domain
* Domain name: movies
* Elasticsearch version: 5.1
* Everything else is using defaults

``` bash
$ aws es create-elasticsearch-domain --domain-name movies --elasticsearch-version 5.1

{
    "DomainStatus": {
        "ElasticsearchClusterConfig": {
            "DedicatedMasterEnabled": false, 
            "InstanceCount": 1, 
            "ZoneAwarenessEnabled": false, 
            "InstanceType": "m3.medium.elasticsearch"
        }, 
        "DomainId": "538135665317/movies", 
        "Created": true, 
        "Deleted": false, 
        "EBSOptions": {
            "EBSEnabled": false
        }, 
        "Processing": true, 
        "DomainName": "movies", 
        "SnapshotOptions": {
            "AutomatedSnapshotStartHour": 0
        }, 
        "ElasticsearchVersion": "5.1", 
        "AccessPolicies": "", 
        "AdvancedOptions": {
            "rest.action.multi.allow_explicit_index": "true"
        }, 
        "ARN": "arn:aws:es:eu-west-1:538135665317:domain/movies"
    }
}
```


* InstanceType: m3.medium.elasticsearch (this is the default size)
``` bash
aws es create-elasticsearch-domain --domain-name movies --elasticsearch-version 5.1 --elasticsearch-cluster-config InstanceType=m3.medium.elasticsearch,InstanceCount=2 --ebs-options EBSEnabled=true,VolumeType=standard,VolumeSize=100
```


### IP address access policy
``` bash
aws es update-elasticsearch-domain-config --endpoint https://es.eu-west-1.amazonaws.com --domain-name movies --access-policies '{"Version": "2012-10-17", "Statement": [{"Action": "es:ESHttp*","Principal":"*","Effect": "Allow", "Condition": {"IpAddress":{"aws:SourceIp":["91.185.160.194/32"]}}}]}"'

aws es update-elasticsearch-domain-config --endpoint https://es.eu-west-1.amazonaws.com --domain-name movies --access-policies '{"Version": "2012-10-17", "Statement": [{"Action": "es:ESHttp*","Principal":"*","Effect": "Allow", "Condition": {"IpAddress":{"aws:SourceIp":["192.0.2.0/32"]}}}]}"'

'{"Version": "2012-10-17", "Statement": [{
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "AWS": "*"
      },
      "Action": "es:*",
      "Condition": {
        "IpAddress": {
          "aws:SourceIp": [
            "91.185.160.194"
          ]
        }
      },
      "Resource": "arn:aws:es:eu-west-1:538135665317:domain/movies/*"
    }
  ]
}'
```


### Upload single documet to Amazon ES domain
``` bash
curl -XPUT search-movies-sj6hjznzvrwoca2tkatexheolq.eu-west-1.es.amazonaws.com/movies/movie/tt0116996 -d '{"directors" : ["Tim Burton"],"genres" : ["Comedy","Sci-Fi"],"plot" : "The Earth is invaded by Martians with irresistible weapons and a cruel sense of humor.","title" : "Mars Attacks!","actors" : ["Jack Nicholson","Pierce Brosnan","Sarah Jessica Parker"],"year" : 1996}'
```

### Bulk upload movie data to Amazon ES domain
* Get the movie data from: [Node.js and DynamoDB, Step 2: Load Sample data](http://docs.aws.amazon.com/amazondynamodb/latest/gettingstartedguide/GettingStarted.NodeJs.02.html#GettingStarted.NodeJs.02.01).  Or download the movie data file directly [moviedata.zip](http://docs.aws.amazon.com/amazondynamodb/latest/gettingstartedguide/samples/moviedata.zip)



### Search documents in Amazon ES Domain
```  bash
curl -XGET 'search-movies-sj6hjznzvrwoca2tkatexheolq.eu-west-1.es.amazonaws.com/movies/_search?q=mars'
```

curl -XGET 'search-movies-sj6hjznzvrwoca2tkatexheolq.eu-west-1.es.amazonaws.com/movies/_search?q=nightmare'


### Deleting an Amazon ES Domain
``` bash
aws es delete-elasticsearch-domain --endpoint https://es.eu-west-1.amazonaws.com --domain-name movies
```




``` bash
aws es create-elasticsearch-domain --domain-name bulk-movies --elasticsearch-version 5.1
aws es update-elasticsearch-domain-config --endpoint https://es.eu-west-1.amazonaws.com --domain-name bulk-movies --access-policies '{"Version": "2012-10-17","Statement": [{ "Effect": "Allow", "Principal": { "AWS": "*" }, "Action": "es:*", "Resource": "arn:aws:es:eu-west-1:538135665317:domain/movies/*" }]}'
curl -XPOST 'http://search-movies-sj6hjznzvrwoca2tkatexheolq.eu-west-1.es.amazonaws.com/_bulk' --data-binary @moviedata.json
aws es delete-elasticsearch-domain --endpoint https://es.eu-west-1.amazonaws.com --domain-name bulk-movies
```