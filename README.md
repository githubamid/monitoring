docker pull bitnami/node-exporter:latest
docker pull bitnami/prometheus:latest 
docker network create monitor

docker run -d --name node-exporter1 -p 9101:9100 --network monitor bitnami/node-exporter:latest
docker run -d --name node-exporter2 -p 9102:9100 --network monitor bitnami/node-exporter:latest
docker run -d --name node-exporter3 -p 9103:9100 --network monitor bitnami/node-exporter:latest

docker run -d --name prometheus -p 9090:9090 --network monitor -v $(pwd)/prometheus.yml:/opt/bitnami/prometheus/conf/prometheus.yml bitnami/prometheus:latest

docker stop node-exporter1
docker build -t pythonserver .
docker run -d --name pythonserver -p 8081:8080 --network monitor pythonserver
docker restart prometheus

curl localhost:8081
curl localhost:8081/home
curl localhost:8081/contact


Use the Prometheus UI to query for the following metrics.

node_cpu_seconds_total{instance="node-exporter2:9100"}
node_ipvs_connections_total

flask_http_request_duration_seconds_bucket
flask_http_request_total
process_virtual_memory_bytes