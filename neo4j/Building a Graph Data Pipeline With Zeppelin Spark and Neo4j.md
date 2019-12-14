Dịch từ bài gốc: https://towardsdatascience.com/building-a-graph-data-pipeline-with-zeppelin-spark-and-neo4j-8b6b83f4fb70

## Yếu tố kĩ thuật:

- Zeppelin
- Spark
- Neo4j
- Docker

## Dataset

- Chicago Crime dataset https://dev.socrata.com/foundry/data.cityofchicago.org/6zsd-86xi

## Bước 1. Cấu hình Container

```bash
FROM apache/zeppelin:0.8.0

# Workaround to "fix" https://issues.apache.org/jira/browse/ZEPPELIN-3586

RUN echo "$LOG_TAG Download Spark binary" && \
    wget -O /tmp/spark-2.3.1-bin-hadoop2.7.tgz https://archive.apache.org/dist/spark/spark-2.3.1/spark-2.3.1-bin-hadoop2.7.tgz && \
    tar -zxvf /tmp/spark-2.3.1-bin-hadoop2.7.tgz && \
    rm -rf /tmp/spark-2.3.1-bin-hadoop2.7.tgz && \
    mv spark-2.3.1-bin-hadoop2.7 /spark-2.3.1-bin-hadoop2.7
	
ENV SPARK_HOME=/spark-2.3.1-bin-hadoop2.7
	
CMD ["bin/zeppelin.sh"]
```

## Bước 2. Cấu hình Docker compose

```yaml
version: '3'

services:
    neo4j:
        image: neo4j
        networks:
            - host
        ports:
            - 7474:7474
            - 7687:7687
        environment:
            - NEO4J_AUTH=neo4j/zeppelin
    zeppelin:
        build:
            context: .
        image: santand84/zeppelin:0.8.0
        networks:
            - host
        ports:
            - 8080:8080
        volumes:
            - ./zeppelin/notebook:/zeppelin/notebook
            - ./zeppelin/conf:/zeppelin/conf
networks:
    host:
```

Source code: https://github.com/toanalien/zeppelin-neo4j

## Bước 3: Chạy tất cả dịch vụ bằng Docker compose

```bash
docker-compose -f neo4j-zeppelin.yml up -d
```

Truy cập dịch vụ:
- Zeppelin: http://localhost:8080
- Neo4j: http://localhost:7474

## Bước 4: Cấu hình interpreter

- Làm giống trong hướng dẫn, restart Zeppelin container



