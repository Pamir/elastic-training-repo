# Clusters-to-go
Below is a collection of docker-compose files to spin up simple Elastic clusters, useful for practicing with ElasticSearch.
For more details on how to setup Elasticsearch with docker, please refer to the [Elastic documentation](https://www.elastic.co/guide/en/elasticsearch/reference/6.6/docker.html).

## 1 Elasticsearch node, 1 Kibana instance
(v6.6.1)

```
version: '3'
services:
  esnode:
    image: docker.elastic.co/elasticsearch/elasticsearch:6.6.1
    container_name: esnode
    environment:
      - node.name=esnode
      - cluster.name=elastic-cluster
      - discovery.type=single-node
      - bootstrap.memory_lock=true
      - ES_JAVA_OPTS=-Xms512m -Xmx512m
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - esnode-data:/usr/share/elasticsearch/data
    ports:
      - 9200:9200
    networks:
      - elastic-net
  kibana:
    image: docker.elastic.co/kibana/kibana:6.6.1
    container_name: kibana
    environment:
      - elasticsearch.url=http://elasticsearch:9200
    ulimits:
      memlock:
        soft: -1
        hard: -1
    ports:
      - 5601:5601
    networks:
      - elastic-net
    links:
      - esnode:elasticsearch

volumes:
  esnode-data:
    driver: local

networks:
  elastic-net:
```