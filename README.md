# docker ELK -  ElasticSearch, Logstash, and Kibana
*************************************************************************************
sudo systemctl  stop metricbeat \
sudo dpkg --remove metricbeat \
sudo rm -rf /etc/metricbeat \
sudo rm -rf /var/lib/metricbeat\
sudo dpkg --purge  metricbeat 

sudo systemctl stop packetbeat \
sudo dpkg --remove packetbeat  \
sudo rm -rf /etc/packetbeat \
sudo rm -rf /var/lib/packetbeat \
sudo dpkg --purge  packetbeat 

sudo systemctl  stop filebeat \
sudo dpkg --remove filebeat \
sudo rm -rf /etc/filebeat \
sudo rm -rf /var/lib/filebeat \
sudo dpkg --purge  filebeat 

sudo systemctl  stop heartbeat \
sudo dpkg --remove heartbeat \
sudo rm -rf /etc/heartbeat \
sudo rm -rf /var/lib/heartbeat \
sudo dpkg --purge  heartbeat

sudo systemctl  stop auditbeat \
sudo dpkg --remove auditbeat \
sudo rm -rf /etc/auditbeat \
sudo rm -rf /var/lib/auditbeat \
sudo dpkg --purge auditbeat
*************************************************************************************
curl http://localhost:9200 \
curl http://localhost:5601 \

curl -XPOST -D- 'http://localhost:5601/api/saved_objects/index-pattern' \
 -H 'Content-Type: application/json' \
 -H 'kbn-version: 6.7.1' \
 -d '{"attributes":{"title":"logstash-*","timeFieldName":"@timestamp"}}'

wget https://download.elastic.co/demos/kibana/gettingstarted/logs.jsonl.gz \
gunzip logs.jsonl.gz \
cat logs.jsonl | nc localhost 5000
*************************************************************************************
curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-6.7.1-amd64.deb \
sudo dpkg -i filebeat-6.7.1-amd64.deb \
sudo nano /etc/filebeat/filebeat.yml \
setup.kibana:
  host: "localhost:5601"  
sudo filebeat modules enable system \
sudo nano /etc/filebeat/modules.d/system.yml \
- module: system
  syslog:
    enabled: true
    var.paths: ["/var/log/syslog*"]
  auth:
    enabled: true
    var.paths: ["/var/log/auth.log*"]
sudo filebeat setup \
sudo service filebeat restart
*************************************************************************************
curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-6.7.1-amd64.deb
sudo dpkg -i metricbeat-6.7.1-amd64.deb

sudo nano /etc/metricbeat/metricbeat.yml

setup.kibana:
  host: "localhost:5601"

sudo metricbeat modules enable system

sudo metricbeat setup

sudo service metricbeat start

*************************************************************************************
sudo apt-get install libpcap0.8 -y

curl -L -O https://artifacts.elastic.co/downloads/beats/packetbeat/packetbeat-6.7.1-amd64.deb

sudo dpkg -i packetbeat-6.7.1-amd64.deb

sudo nano /etc/packetbeat/packetbeat.yml

setup.kibana:
  host: "localhost:5601" 
  
sudo packetbeat setup 

sudo service packetbeat start
*************************************************************************************
curl -L -O https://artifacts.elastic.co/downloads/beats/heartbeat/heartbeat-6.7.1-amd64.deb

sudo dpkg -i heartbeat-6.7.1-amd64.deb

sudo nano /etc/heartbeat/heartbeat.yml

setup.kibana:
  host: "localhost:5601" 
  
sudo heartbeat setup 

sudo service heartbeat-elastic start

sudo heartbeat export template > heartbeat.template.json

curl -XPUT -H 'Content-Type: application/json' http://localhost:9200/_template/heartbeat-6.7.1 -d@heartbeat.template.json
*************************************************************************************
curl -L -O https://artifacts.elastic.co/downloads/beats/auditbeat/auditbeat-6.7.1-amd64.deb

sudo dpkg -i auditbeat-6.7.1-amd64.deb

sudo nano /etc/auditbeat/auditbeat.yml

setup.kibana:
  host: "localhost:5601" 
  
sudo auditbeat export template > auditbeat.template.json

curl -XPUT -H 'Content-Type: application/json' http://localhost:9200/_template/auditbeat-6.7.1 -d@auditbeat.template.json

sudo auditbeat setup --dashboards

sudo service auditbeat start
*************************************************************************************
