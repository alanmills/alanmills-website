Setup AWS CloudSearch using AWS CLI
===================================
**Author:** Alan Mills  
**Date:** [21 March 2017 15:50]
**Tags:** [AWS], [CLI], [CloudSearch]
**Status**: Draft

[Creating a Domain Using the AWS CLI](http://docs.aws.amazon.com/cloudsearch/latest/developerguide/creating-domains.html#create-domain-clt)

``` bash
aws cloudsearch create-domain --domain-name movies
{
  "DomainStatus": {
      "DomainId": "965407640801/movies", 
      "Created": true, 
      "Deleted": false, 
      "SearchInstanceCount": 0, 
      "DomainName": "movies", 
      "SearchService": {}, 
      "RequiresIndexDocuments": false, 
      "Processing": false, 
      "DocService": {}, 
      "ARN": "arn:aws:cloudsearch:us-east-1:965407640801:domain/movies", 
      "SearchPartitionCount": 0
  }
}
```