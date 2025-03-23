# jupiter-elasticsearch-docker

Dockerinzing Jupyter Notebook and ElasticSearch. 


## Docker compose 

Create the *docker-compose.yml* file and add the following content

````bash
version: "3.9"
services:
  jupyter:
    image: jupyter/minimal-notebook:ubuntu-18.04
    container_name: my_jupyter_from_compose
    networks:
      - my_network_from_compose
    volumes:
      - .:/home/kkherrazi/work
    ports:
      - target: 8888
        published: 4444
        protocol: tcp
        mode: host
    environment:
      JUPYTER_TOKEN: "bonjour"
  elasticsearch:
    image: elasticsearch:7.2.0
    container_name: my_es_from_compose
    networks:
      - my_network_from_compose
    ports:
      - "9200:9200"
      - "9300:9300"
    environment:
      discovery.type: single-node
networks:
  my_network_from_compose:
````

Start services: 

````bash
docker-compose up

````
## Jupyter Notebook
Once the services have been launched, go to the browser with your virtual machine's IP address and port 4444, and you should see the jupyter interface.

In a new notebook, run the following **Python** code:

````bash
import requests
import pprint
import json

content = json.loads(requests.get('http://my_es_from_compose:9200').content)

pprint.pprint(content)
````

 
