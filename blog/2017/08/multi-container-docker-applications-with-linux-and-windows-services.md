# Mulit container  applications with Linux and Windows services
**Author:** [Alan Mills]
**Date:** [16 August 2017 10:21]
**Tags:** [Windows Server 2016], [Docker], [Linux]
**Status**: Draft


docker-compose.yml
``` yml
services:
  database:
    image: sixeyed/atsea-db:mssql
    ports:
      - mode: host
        target: 1433
        published: 1433
    networks:
      - atsea
    deploy:
      endpoint_mode: dnsrr
      placement:
        constraints:
          - 'node.platform.os == windows'

  appserver:
    image: sixeyed/atsea-app:mssql
    ports:
      - mode: host
        target: 8080
        published: 80
    networks:
      - atsea
    deploy:
      endpoint_mode: dnsrr
      placement:
        constraints:
          - 'node.platform.os == linux'
```

**Is this just an Enterprise version thing of can this be one with the Community version?**
When a worker is added to the cluster it is automatically labeled as Windows or Linux, the constraint **node.platform.os** is using this capability.
