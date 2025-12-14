# ELK

1 — Add Elastic APT repository (one time)

Run as a user with sudo:

    # install prerequisites
    sudo apt update
    sudo apt install -y wget apt-transport-https ca-certificates

    # Import Elastic's GPG key and add repo
    wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
    sudo sh -c 'echo "deb https://artifacts.elastic.co/packages/8.x/apt stable main" > /etc/apt/sources.list.d/elastic-8.x.list'

    sudo apt update


(Replace 8.x with the major version you want — keep stack components the same version.) 
Elastic
+1

## 2. Install Elasticsearch
    sudo apt install -y elasticsearch


    Edit /etc/elasticsearch/elasticsearch.yml (minimum for a single node):

    network.host: 0.0.0.0        # bind to all interfaces (or set a single IP / FQDN)
    http.port: 9200
    discovery.type: single-node  # single-node development/test only
# (leave xpack.security enabled by default for 8.x)


## Enable & start:

    # kernel & limits required for production (do this before starting)
    
    sudo sysctl -w vm.max_map_count=262144
    
    # make it persistent:

    echo "vm.max_map_count=262144" | sudo tee -a /etc/sysctl.conf
    
    # Add file descriptor limits (example)
    
    sudo bash -c 'cat >> /etc/security/limits.conf <<EOF
    
    elasticsearch - nofile 65536
    
    EOF'

# start
    sudo systemctl daemon-reload
    sudo systemctl enable --now elasticsearch
    sudo systemctl status elasticsearch --no-pager


Security note: Elasticsearch 8+ automatically performs initial security steps on first start: it will generate TLS certs, set a bootstrap/elastic password and produce an enrollment token for Kibana. Save the bootstrap output printed on first start (or run the tools later). 
Elastic
+1

## 3. Get the generated password / enrollment token

When Elasticsearch first starts it outputs the bootstrap password + tokens to the service stdout (or journal). You can find them like:

    sudo journalctl -u elasticsearch -b | grep -i "Enrollment token\|Password"


    If you need to create new tokens later:

    sudo /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana


And to reset built-in users’ passwords later use elasticsearch-reset-password. 
elastic.org.cn
+1

## 4. Install Kibana
    sudo apt install -y kibana


    Edit /etc/kibana/kibana.yml:

    server.host: "0.0.0.0"               # or a single IP/FQDN
    elasticsearch.hosts: ["https://localhost:9200"]  # use https if ES has TLS
# If Kibana needs an enrollment token, you can enroll using the token (Kibana setup UI will accept it).


Start & enable:

    sudo systemctl enable --now kibana
    sudo systemctl status kibana --no-pager


Open http://<kibana-ip>:5601 in your browser; use the elastic user and password (or follow Kibana’s enrollment flow if prompted). 
Elastic
+1

## 5. Install Logstash (optional, for heavy ingest / filtering)
sudo apt install -y logstash


Create a sample pipeline /etc/logstash/conf.d/10-beats.conf:

    input {
    beats {
        port => 5044
    }
    }

    filter {
    # example: parse syslog, add grok here
    }

    output {
    elasticsearch {
        hosts => ["https://localhost:9200"]
        user => "logstash_system"       # set after creating passwords
        password => "<LOGSTASH_SYSTEM_PASSWORD>"
        ssl => true
        cacert => "/etc/logstash/certs/ca.crt"  # if using TLS from ES
    }
    }


Enable & start:

    sudo systemctl enable --now logstash
    sudo systemctl status logstash --no-pager


See official Logstash install & pipeline docs for more advanced pipeline examples. 
Elastic

## 6. — Install Filebeat (on the host(s) you want to ship logs from)

On the shipper machine (can be same machine for testing):

    sudo apt install -y filebeat
    # enable system module which collects /var/log/syslog, auth, etc.
    sudo filebeat modules enable system
    # if sending to Logstash:
    sudo sed -i 's/#output.logstash:/output.logstash:\n  hosts: ["your-logstash-host:5044"]/g' /etc/filebeat/filebeat.yml
    # or to Elasticsearch:
    # sudo sed -i 's/#output.elasticsearch:/output.elasticsearch:\n  hosts: ["http://your-es:9200"]/g' /etc/filebeat/filebeat.yml

    sudo filebeat setup       # loads dashboards and index templates (requires Kibana reachable)
    sudo systemctl enable --now filebeat


Filebeat docs and quickstart are excellent for module use and dashboards. 
Elastic

## 7 — Basic verification

Elasticsearch cluster health:

# if security enabled, use elastic user and password
    curl -u elastic:'<password>' -k https://localhost:9200/_cluster/health?pretty
