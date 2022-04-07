## Installing Kibana on Ubuntu AWS or WSL

###### Update the Instance

```
sudo apt update
sudo apt upgrade -y
```
###### Install the necessary packages, Import the Elasticsearch PGP Key and install kibana
```
sudo apt install -y vim wget

wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -

echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list

sudo apt-get update && sudo apt-get install kibana
```

###### start enable and check the status of Kibana
```
sudo systemctl start kibana

sudo systemctl status kibana

sudo systemctl enable kibana
```
###### Enable the service in firewall

```
By default Kibana uses port 5601

sudo ufw allow 5601

confirm the port using 
ss -tulpn | grep 5601
```
##### OUTPUT
```
tcp   LISTEN 0      511             127.0.0.1:5601         0.0.0.0:*
```
```
curl localhost:5601 in terminal or http://localhost:5601 on browser
```
##### NB: If you have configured a password for your elasticsearch, configure with this: Also Elasticsearch needs to be running before kibana can work
```
elasticsearch.username: "kibana_system"
elasticsearch.password: "pass"
```
```
To apply any changes make sure you restart kibana or elasticsearch
 sudo systemctl restart kibana
```

#### Setting up security will be under security repository for Kibana Ui integration like Rabbitmq etc.




