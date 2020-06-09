# docker-componse.md
what's docker componse ?
core: yaml 

version Sel
recommend v3(multi machine)->v2(single machine )
not recommend  v1

* three core
    1. services 
    2. network
    3. volume 

## Services 
1. it's a container,create image from dockerhub,or build image from dockerfile

2. similarly docker run ,we may specify the network and volume(ref)

for exmaple : yaml  content
services : from dockerhub
    db:
            image: postgres:9.4
            volumes:
                - "db-data:/var/lib/postgresql/data"
            networks:
                - back-tier
    # [docker run -d --network back-tier -v db-data:/var/lib/postgresql/data postgresql:9.4]

services:
worker:
    build: ./worker(dockerifle position)
    links:
        - db
        - redis
    networks:
        - back-tier

volumes- services:
services:
    db:
        image: postgresql:9.4
        volumes:
            - "db-data:/var/lib/postgresql/data"
        networks:
            - back-tier

sixth course : docker-componse.yml

## componse install


