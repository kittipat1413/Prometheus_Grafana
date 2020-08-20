# Prometheus_Grafana on AWS

## Pull Docker Image
  ```
   docker pull prom/prometheus
   docker pull prom/node-exporter
   docker pull grafana/grafana
   
   ```
    
## Prometheus
  ### 1. RUN Prometheus-Docker

  ```
  docker run -d -p 9090:9090 -v /home/ec2-user/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml --restart=always --name=prometheus prom/prometheus 

  ```
  * ***Default from DockerFile***
    * ENTRYPOINT is default command to execute at runtime (./bin/prometheus)
    * CMD or COMMAND will be appended as arguments to the ENTRYPOINT
    * Full command that will be run in container => ./bin/prometheus --config.file=/etc/prometheus/prometheus.yml --storage.tsdb.path=/prometheus ........................

    ```
    ENTRYPOINT [ "/bin/prometheus" ]
    CMD        [ "--config.file=/etc/prometheus/prometheus.yml", \
                 "--storage.tsdb.path=/prometheus", \
                 "--web.console.libraries=/usr/share/prometheus/console_libraries", \
                 "--web.console.templates=/usr/share/prometheus/consoles" ]

    ```
    ### See documentation [here](https://github.com/prometheus/prometheus)


  * ***Check Args of prometheus (flags)***
    ```
    docker inspect {{Container NAME,ID}}
    ```
  * ***Custom flags using docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]***
    
    *Override prometheus flag by adding --{{flags set}}*
    ### EX.
    ```
    docker run -d -p 9090:9090 -v /home/ec2-user/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml --restart=always --name=prometheus prom/prometheus --storage.tsdb.retention=5d    
    ```

## Node-Exporter

  ### 2.
  ```
  docker run -d -p 9100:9100 --restart=always --name=node-exporter prom/node-exporter
  ```
  
## Grafana
  ### 3.
  ```
  docker run -d -p 3000:3000 --name=grafana --restart=always -e "GF_SERVER_ROOT_URL=http://grafana.server.name" -e "GF_SECURITY_ADMIN_PASSWORD=admin" grafana/grafana
  ```
*Grafana user,pass*
- User: admin
- Pass: admin

## Setting-Grafana
![Setting-Grafana](https://github.com/kittipat1413/Prometheus_Grafana/blob/master/img/Grafana1.png)
