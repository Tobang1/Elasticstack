### Installing Filebeat
###### Filebeat is a lightweight shipper for forwarding and centralizing log data. Installed as an agent on your servers, Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.


#### 1. INSTALL FILEBEAT

```
sudo apt update
sudo apt upgrade

curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-oss-7.15.1-amd64.deb
sudo dpkg -i filebeat-oss-7.15.1-amd64.deb

```

#### 2.  Enable SYSTEM MODULE FOR FILEBEAT
###### There are several built in filebeat modules you can use. To enable the system module run.

```
sudo filebeat modules list
sudo filebeat modules enable system


Additional module configuration can be done using the per module config files located in the modules.d folder, most commonly this would be to read logs from a non-default location

deb /etc/filebeat/modules.d/
```
#### 4. LOCATE CONFIGURATION FILE
```
/etc/filebeat/filebeat.yml

sudo vim /etc/filebeat/filebeat.yml

by default file beat collect data from all .logs files in /var/logs directory which can be edited in the filebeat.yml file .
```
#### 5.  CONFIGURE OUTPUT

```
We'll be shipping to Logstash so that we have the option to run filters before the data is indexed.
Comment out the elasticsearch output block.

## Comment out elasticsearch output
#output.elasticsearch:
#  hosts: ["localhost:9200"]
Uncomment and change the logstash output to match below.

output.logstash:
    hosts: ["your-logstash-host:your-ssl-port"]
    loadbalance: true
    ssl.enabled: true
```
#### 6. VALIDATE CONFIGURATION

```
sudo filebeat -e -c /etc/filebeat/filebeat.yml
```
#### 7. (OPTIONAL) UPDATE LOGSTASH FILTERS
```
STEP 6 - (OPTIONAL) UPDATE LOGSTASH FILTERS
All Logit.io stacks come pre-configured with popular Logstash filters. We would recommend that you add system specific filters if you don't already have them, to ensure enhanced dashboards and modules work correctly.

Edit your Logstash filters by choosing Stack > Settings > Logstash Filters

if [fileset][module] == "system" {
  if [fileset][name] == "auth" {
    grok {
      match => { "message" => ["%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} sshd(?:\[%{POSINT:[system][auth][pid]}\])?: %{DATA:[system][auth][ssh][event]} %{DATA:[system][auth][ssh][method]} for (invalid user )?%{DATA:[system][auth][user]} from %{IPORHOST:[system][auth][ssh][ip]} port %{NUMBER:[system][auth][ssh][port]} ssh2(: %{GREEDYDATA:[system][auth][ssh][signature]})?",
                "%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} sshd(?:\[%{POSINT:[system][auth][pid]}\])?: %{DATA:[system][auth][ssh][event]} user %{DATA:[system][auth][user]} from %{IPORHOST:[system][auth][ssh][ip]}",
                "%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} sshd(?:\[%{POSINT:[system][auth][pid]}\])?: Did not receive identification string from %{IPORHOST:[system][auth][ssh][dropped_ip]}",
                "%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} sudo(?:\[%{POSINT:[system][auth][pid]}\])?: \s*%{DATA:[system][auth][user]} :( %{DATA:[system][auth][sudo][error]} ;)? TTY=%{DATA:[system][auth][sudo][tty]} ; PWD=%{DATA:[system][auth][sudo][pwd]} ; USER=%{DATA:[system][auth][sudo][user]} ; COMMAND=%{GREEDYDATA:[system][auth][sudo][command]}",
                "%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} groupadd(?:\[%{POSINT:[system][auth][pid]}\])?: new group: name=%{DATA:system.auth.groupadd.name}, GID=%{NUMBER:system.auth.groupadd.gid}",
                "%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} useradd(?:\[%{POSINT:[system][auth][pid]}\])?: new user: name=%{DATA:[system][auth][user][add][name]}, UID=%{NUMBER:[system][auth][user][add][uid]}, GID=%{NUMBER:[system][auth][user][add][gid]}, home=%{DATA:[system][auth][user][add][home]}, shell=%{DATA:[system][auth][user][add][shell]}$",
                "%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} %{DATA:[system][auth][program]}(?:\[%{POSINT:[system][auth][pid]}\])?: %{GREEDYMULTILINE:[system][auth][message]}"] }
      pattern_definitions => {
        "GREEDYMULTILINE"=> "(.|\n)*"
      }
      remove_field => "message"
    }
    date {
      match => [ "[system][auth][timestamp]", "MMM  d HH:mm:ss", "MMM dd HH:mm:ss" ]
    }
    geoip {
      source => "[system][auth][ssh][ip]"
      target => "[system][auth][ssh][geoip]"
    }
  }
  else if [fileset][name] == "syslog" {
    grok {
      match => { "message" => ["%{SYSLOGTIMESTAMP:[system][syslog][timestamp]} %{SYSLOGHOST:[system][syslog][hostname]} %{DATA:[system][syslog][program]}(?:\[%{POSINT:[system][syslog][pid]}\])?: %{GREEDYMULTILINE:[system][syslog][message]}"] }
      pattern_definitions => { "GREEDYMULTILINE" => "(.|\n)*" }
      remove_field => "message"
    }
    date {
      match => [ "[system][syslog][timestamp]", "MMM  d HH:mm:ss", "MMM dd HH:mm:ss" ]
    }
  }
}
```
#### 8. START FILEBEAT
```
sudo systemctl enable filebeat
sudo systemctl start filebeat
```
### Addidng filebeat index to kibana UI

```
On the kibana Ui, 
under search bar search for kibana index pattern (alose located under management )
2. click on create index pattern
3. Under Name specify filebeat*
4. select timestamp @timestamp
5. select create Index pattern. 
```

```
 NB. if elasticsearch uses password. you should specify the username and password  under section Elastic Outputin filebeat yml file /etc/filebeat/filebeat.yml

   # Authentication credentials - either API key or username/password.
  #api_key: "id:api_key"
  username: "elastic"
  password: "q6FxGIzNMOzCgWan3aCn"

  To generate password follow the security repository link here
  ```