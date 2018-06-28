Trouble shooting the entire ROCK STACK

1. Ensure NIC is seeing traffic

`sudo tcpdump -i enp2s0`

2. Stop all services

  `sudo systemctl stop suricata`

   `sudo systemctl stop stenographer`

   `sudo systemctl stop fsf`

   `sudo systemctl stop filebeat`

   `sudo systemctl stop zookeeper`

   `sudo systemctl stop kafka`

   `sudo systemctl stop kibana`

   `sudo systemctl stop elasticsearch`

   `sudo systemctl stop logstash`

   `sudo broctl stop`

3. time for some cleanup - recommended for new install - not if you want to retain data:

check suricata:

`sudo rm -rf /data/suricata/logs/*`

`ls /data/suricata/logs/``
:should see nothing:

`sudo systemctl start suricata`

`ls /data/suricata/logs/`
:should see eve.json file.

## validate Stenographer:

`sudo rm -rf /data/stenographer/*`

`sudo systemctl start stenographer`

`sudo -s`

`cd /data/stenographer/packets/`

## validate BRO

`sudo broctl start`

`cd /data/bro/logs/current/`

`ls`
:looking for conn.log:


## validate Kafka : contains bro, fsf, and suricata: Can validate all three...

`sudo rm -rf /data/kafka/logs/*`

`sudo rm -rf /var/lib/zookeeper/version-2/`

`sudo systemctl start zookeeper`

`sudo systemctl start kafka`

`cd /data/kafka/logs`

`ls`

:look for bro-raw-0: this means bro is sending data to kafka:

`sudo systemctl start filebeat`

`ls`

:look for suricata-raw-0: this means that filebeat is moving eve.json to kafka.

Note - if you're not seeing anything check the /etc/filebeat/filebeat.yml for the input section and the kafka section.


## Validate FSF:

`cd /data/fsf/logs`

`sudo rm rockout.log`

`sudo systemctl start fsf`

`/opt/fsf/fsf-client/fsf_client.py --full daemon.log`

`ls`
looking for a rockout.bro

`cd /data/kafka/logs`
looking for fsf-raw-0

## Validate elasticsearch:

`sudo systemctl start elasticsearch`

test the api
`curl 172.16.10.100:9200`

## Validate Logstash
`sudo systemctl start logstash`

`curl 172.16.10.100:9200/_cat/indices` run this a couple of times and look for a data increase

## validate kibana

`sudo systemctl start kibana`

---SIMPLY GO TO WEBSITE:

4. Overall troubleshooting techniques

cd var/log/
ls
cat kafka/server/log | grep ERROR

etc.

you can cat all of the logs looking for ERROR 's'

cd /var/log/elasticsearch/



journalctl -xe - is good troubleshooting techniques

`sudo systemctl stop filebeat`

`sudo systemctl start filebeat`

`journalctl -xe`
