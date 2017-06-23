# kafdrop-docker-image

Using the baseimage [maven:alpine](https://hub.docker.com/r/library/maven/) from [Docker Hub](https://hub.docker.com/), [kafdrop](https://github.com/HomeAdvisor/Kafdrop) is compiled and included in the image.

Some environment variable can be adjusted at runtime:

    ZK_HOSTS=localhost:2181
    LISTEN=9000

The default command of the image is
    
    java -jar /usr/local/bin/kafdrop.jar \
        --zookeeper.connect=${ZK_HOSTS} --server.port=${LISTEN}

If there is a working kafka instance running on localhost (aka: the zookeeper ports are accessible), start a container with

    docker run -e ZK_HOSTS=localhost:2181 -e LISTEN=9010 \
        thomsch98/kafdrop:alpine

For docker-compose, assume a setup like this

    kafdrop:
        image: thomsch98/kafdrop:alpine
        ports:
        - "9010:9010" 
        environment:
        - "ZK_HOSTS=kafka:2181"
        - "LISTEN=9010"
        logging:
        driver: journald

    kafka:
        image: massenz/kafka:0.10.1.1
        hostname: kafka
        ports:
        - "2181:2181"
        - "9092:9092"
        environment:
        - "ADVERTISED_HOST=kafka"
        - "ADVERTISED_PORT=9092"
        logging:
        driver: journald