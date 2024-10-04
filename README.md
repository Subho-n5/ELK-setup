# 1. Install Elasticsearch
> `sudo apt-get install curl`
> 
> `curl -fsSL <https://artifacts.elastic.co/GPG-KEY-elasticsearch> | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/elastic-archive-keyring.gpg`
> 
> `echo "deb <https://artifacts.elastic.co/packages/8.x/apt> stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-8.x.list`

 **For installing as localhost**
> `sudo apt install elasticsearch`

**For installing as some custom website (e.g., <kali-purple.kali.purple>)**
> `sudo bash -c "export HOSTNAME=<kali-purple.kali.purple>; apt-get install elasticsearch -y"`

*Note: If you mention a custom website, certificates will be created based on the website name*
*Note: Take note of "elastic" user password and other security details*

# 2. Convert to single-node setup *(or replace fqdn name in initial_master_nodes list with IP address)*
> *To Comment cluster.initial_master_nodes*
> 
>  `sudo sed -e '/cluster.initial_master_nodes/ s/^#*/#/' -i /etc/elasticsearch/elasticsearch.yml`
> 
> *To set the single-node functionality*
> 
> `echo "discovery.type: single-node" | sudo tee -a /etc/elasticsearch/elasticsearch.yml`

# 3. Install Kibana
> `sudo apt install kibana`
>
> *Add keys to /etc/kibana/kibana.yml*
>
> `sudo /usr/share/kibana/bin/kibana-encryption-keys generate -q`

**Optional for custom-website:[Nothing to run for localhost]**
> `echo "server.host: \"<kali-purple.kali.purple>\"" | sudo tee -a /etc/kibana/kibana.yml`
> 
> *Ensure <kli-purple.kali.purple> is only mapped to 192.168.253.5 in /etc/hosts in order to bind Kibana to custom-URL interface*
> 
> `sudo systemctl enable elasticsearch kibana --now`

# 4. Enroll Kibana
> `sudo /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana`
> 
> Open browser and navigate to `http://localhost:5601`  OR `http://custom-IP:5601`
> 
> Enter username=elastic and password as displayed after installation
> 
> Paste token from first step
> 
> `sudo /usr/share/kibana/bin/kibana-verification-code`
> 
> Enter verification code into Kibana when prompted

# OPTIONAL for server-setup
## Enable HTTPS for Kibana
> `/usr/share/elasticsearch/bin/elasticsearch-certutil ca`
> 
> `/usr/share/elasticsearch/bin/elasticsearch-certutil cert --ca elastic-stack-ca.p12 --dns kali-purple.kali.purple,elastic.kali.purple,kali-purple --out kibana-server.p12`
> 
> `sudo openssl pkcs12 -in /usr/share/elasticsearch/kibana-server.p12 -out /etc/kibana/kibana-server.crt -clcerts -nokeys`
> 
> `sudo openssl pkcs12 -in /usr/share/elasticsearch/kibana-server.p12 -out /etc/kibana/kibana-server.key -nocerts -nodes`
> 
> `sudo chown root:kibana /etc/kibana/kibana-server.key`
> 
> `sudo chown root:kibana /etc/kibana/kibana-server.crt`
> 
> `sudo chmod 660 /etc/kibana/kibana-server.key`
> 
> `sudo chmod 660 /etc/kibana/kibana-server.crt`
> 
> `echo "server.ssl.enabled: true" | sudo tee -a /etc/kibana/kibana.yml`
> 
> `echo "server.ssl.certificate: /etc/kibana/kibana-server.crt" | sudo tee -a /etc/kibana/kibana.yml`
> 
> `echo "server.ssl.key: /etc/kibana/kibana-server.key" | sudo tee -a /etc/kibana/kibana.yml`
> 
> `echo "server.publicBaseUrl: \"https://kali-purple.kali.purple:5601\"" | sudo tee -a /etc/kibana/kibana.yml`

