# Setting Prometheus - Grafana - Node Exporter 

## Pull Docker Image
  ```
   docker pull prom/prometheus
   docker pull prom/node-exporter
   docker pull grafana/grafana
   
   ```
    
## RUN Docker
  ### 1. Prometheus (See documentation [here](https://github.com/prometheus/prometheus))


  ```
  docker run -d -p 9090:9090 -v /path/to/your/prometheus.yml:/etc/prometheus/prometheus.yml --restart=always --name=prometheus prom/prometheus 

  ```
  * ***Default from DockerFile ***
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

  ### 2. Node-Exporter (See documentation [here](https://github.com/prometheus/node_exporter))

 * ***Using Docker*** 
    ```
    docker run -d -p 9100:9100 --restart=always --name=node-exporter prom/node-exporter
    ```
 * ***Building and running Local***

    __Prerequisites__:

    * [Go compiler](https://golang.org/dl/)
    * RHEL/CentOS: `glibc-static` package.

    __Building:__

        go get github.com/prometheus/node_exporter
        cd ${GOPATH-$HOME/go}/src/github.com/prometheus/node_exporter
        make
        ./node_exporter <flags>

    __To see all available configuration flags:__
    ```
    ./node_exporter -h
    ```
  
  
  ### 3. Grafana
  
  ```
  docker run -d -p 3000:3000 --name=grafana --restart=always -e "GF_SERVER_ROOT_URL=http://grafana.server.name" -e "GF_SECURITY_ADMIN_PASSWORD=admin" grafana/grafana
  ```
  *Grafana user,pass*
  - User: admin
  - Pass: admin


## Setting-Grafana (See documentation [here](https://grafana.com/grafana/dashboards/1860))

![Setting-Grafana](https://github.com/kittipat1413/Prometheus_Grafana/blob/master/img/Grafana1.png)

