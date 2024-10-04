# Install Elasticsearch

> sudo apt-get install curl
> 
> curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/elastic-archive-keyring.gpg
> 
> echo "deb https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-8.x.list
> 
> sudo apt install elasticsearch

**For installing as some custom website (e.g., kali-purple.kali.purple)**
> sudo bash -c "export HOSTNAME=kali-purple.kali.purple; apt-get install elasticsearch -y"

