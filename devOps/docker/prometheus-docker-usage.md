#prometheus-docker-usage.md

sudo docker run -d -p 9090:9090 --name prometheus -v /home/ghost/data/prometheus:/data  prom/prometheus --config.file=/data/prometheus.yml