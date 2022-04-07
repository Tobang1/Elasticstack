### Installing Elastic Search On AWS or WSL Local Server using VScode

###### Elasticsearch is a platform used for real-time full-text searches in applications where a large amount of data needs to be analyzed. In combination with other tools, such as Kibana, Logstash, X-Pack, etc., Elasticsearch can aggregate and monitor Big Data at a massive scale.

##### Install Necessary Dependencies

###### Since Elasticsearch runs on top of Java, you need to install the Java Development Kit (JDK). 
###### You can check if Java is installed and the version on your Ubuntu machine with:
```
java -version
```
###### Update and upgrade the package index

```
sudo apt update
sudo apt upgrade -y
```
###### Install default JDK 
```
sudo apt install openjdk-8-jdk
```
###### check java version
```
java -version
```
###### To allow access to your repositories via HTTPS, you need to install an APT transport package:

```
sudo apt install apt-transport-https
```

### Install and Download Elasticsearch on Ubuntu WSL or AWS instance

###### After you confirm Java and apt-transport-https installed successfully, proceed with steps to install Elasticsearch. Add Elasticsearch Repository, First, update the GPG key for the Elasticsearch repository.Use the wget command to pull the public key:

```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```
###### Add the repository to the system
```
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
```
### Update and Install Elasticsearch
```
sudo apt update
sudo apt install elasticsearch
sudo systemctl daemon-reload
sudo systemctl enable elasticsearch.service
sudo systemctl start elasticsearch.service
service elasticsearch status
```

### Configure Elasticsearch
```
To make changes to the default Elasticsearch configuration, edit the elasticsearch.yml file. The file is located in the /etc/elasticsearch directory.

The configuration for logging is located in the /var/log/elasticsearch/logging.yml file. You can leave the defaults for logging for now and come back to it later if needed.

Note: Any time you make a change to the Elasticsearch configuration, use the sudo systemctl restart elasticsearch.service command to restart the service.
```

### Allow Remote Access

```
sudo vim /etc/elasticsearch/elasticsearch.yml

Uncomment the line (remove the pound (#) sign), set the IP address to 0.0.0.0, and add these lines:

transport.host: localhost
transport.tcp.port: 9300
http.port: 9200
network.host: 0.0.0.0

Exit and save the changes. If using vim press ESC and type :wq
```

### Use UFW to Secure Elasticsearch (Optional)
###### If you allow remote access to Elasticsearch, then we strongly advise using the UFW tool, as a minimum security measure. If you are using AWS make sure you open these ports under security inbound rules.

```
sudo ufw allow 22
sudo ufw allow 9200
sudo ufw enable
sudo ufw status
```

### Test Elastic 
```
curl localhost:9200

if you are using Vscode ssh agent you should port forward into the port section beside the terminal the curl on your browser. 
http://localhost:9200
```
