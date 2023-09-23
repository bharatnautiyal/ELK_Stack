# Setup ELK Stack

## Installing Java on Ubuntu

Before getting your hands on Elasticsearch, you’ll first have to install Java on your server. Java installation is required for Elasticsearch to run.
You’ll install OpenJDK, the open-source Java Development Kit (JDK). This JDK is the recommended Java development environment for Elasticsearch.

sudo apt update -y

sudo apt install default-jdk -y

java -version


## Install Elasticsearch on Ubuntu

sudo apt-get install apt-transport-https -y

curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -

echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list

sudo apt update -y

sudo apt install elasticsearch -y


## Configuring Elasticsearch

sudo nano /etc/elasticsearch/elasticsearch.yml

Uncomment the network.host line by removing the leading # symbol and replacing its value with localhost. Doing so increases security by restricting outside access to your Elasticsearch instance.
Save the changes and exit your editor.

<img src="https://adamtheautomator.com/wp-content/uploads/2022/04/image-653.png" alt="image" width="700">


sudo systemctl daemon-reload

sudo systemctl restart elasticsearch

sudo systemctl start elasticsearch

systemctl status elasticsearch


Lastly, run the following netstat command to verify your Elasticsearch server is listening on the localhost interface on port 9200.

netstat -plntu | grep "9200"

<img src="https://adamtheautomator.com/wp-content/uploads/2022/04/image-655.png" alt="image" width="900">




## Optional

Securing Elasticsearch Using UFW Firewall

sudo ufw allow from 192.168.1.200 to any port 9200

Now, run the ufw status command below to check the status of your UFW firewall.

sudo ufw status verbose

.

.

.

.

# Download and install the Debian package of Kibana v8.10.1 manually

wget https://artifacts.elastic.co/downloads/kibana/kibana-8.10.2-amd64.deb

shasum -a 512 kibana-8.10.2-amd64.deb 

sudo dpkg -i kibana-8.10.2-amd64.deb

## Optional

### If you get any error during the instalation like this one: 
" dpkg: error: dpkg frontend lock was locked by another process with pid 1426

Note: removing the lock file is always wrong, and can end up damaging the

locked area and the entire system. See <https://wiki.debian.org/Teams/Dpkg/FAQ>. "

#### 1. RUN This to Check for Running dpkg or apt Processes: 

ps aux | grep -E '(dpkg|apt)'

#### 2. If you are certain that there are no package management tasks running, RUN THIS:

sudo rm /var/lib/dpkg/lock

#### 3. Then RUN DPKG Again: 
sudo dpkg -i kibana-8.10.2-amd64.deb


## Latest Kibana Debian package Can be downloaded from the website:

https://www.elastic.co/guide/en/kibana/current/deb.html#install-deb

.

.

.

.

# Installing Logstash from Package Repositories

#### 1. Download and install the Public Signing Key:

wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elastic-keyring.gpg

#### 2. You may need to install the apt-transport-https package on Debian before proceeding:

sudo apt-get install apt-transport-https

#### 3. Save the repository definition to /etc/apt/sources.list.d/elastic-8.x.list:

echo "deb [signed-by=/usr/share/keyrings/elastic-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-8.x.list

#### 4. Run sudo apt-get update and the repository is ready for use. You can install it with:

sudo apt-get update && sudo apt-get install logstash


## Latest Logstash Debian package Can be installed from the website:

https://www.elastic.co/guide/en/logstash/current/installing-logstash.html#installing-logstash



