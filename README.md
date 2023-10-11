# Setup ELK Stack for Ubuntu 22.04

## Installing Java

Before getting your hands on Elasticsearch, you’ll first have to install Java and Nginx on your server. Java installation is required for Elasticsearch to run.

sudo apt update -y

sudo apt install default-jdk -y

java -version

## Install Nginx

sudo apt-get -y install nginx

.

.

.

.

.

.

.

# Install Elasticsearch on Ubuntu

sudo apt-get install apt-transport-https -y

curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -

echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list

sudo apt update -y

sudo apt install elasticsearch -y


## Configuring Elasticsearch

sudo nano /etc/elasticsearch/elasticsearch.yml

Uncomment the network.host and http.port line replacing its value with:

```nginx
network.host: "localhost"

http.port: 9200
```

<img src="https://adamtheautomator.com/wp-content/uploads/2022/04/image-653.png" alt="image" width="700">


sudo systemctl daemon-reload

sudo systemctl restart elasticsearch

sudo systemctl start elasticsearch

systemctl status elasticsearch


Lastly, run the following netstat command to verify your Elasticsearch server is listening on the localhost interface on port 9200.

netstat -plntu | grep "9200"

<img src="https://adamtheautomator.com/wp-content/uploads/2022/04/image-655.png" alt="image" width="900">



## Latest Elasticsearch Debian package Can be installed from the website:

https://www.elastic.co/guide/en/elasticsearch/reference/current/deb.html

### For installing Manually

https://www.elastic.co/guide/en/elasticsearch/reference/current/deb.html#install-deb

.

.

.

.

.

.

.

# Download and install the Debian package of Kibana.

sudo apt update

sudo apt install kibana


### Uncomment this in kibana.yml 

sudo nano /etc/kibana/kibana.yml

```nginx
http.port: 5601

network.host: "localhost"
```



## Optional if getting error while instalation like this one: 

" dpkg: error: dpkg frontend lock was locked by another process with pid 1426 "


#### 1. RUN This to Check for Running dpkg or apt Processes: 

ps aux | grep -E '(dpkg|apt)'

#### 2. If you are certain that there are no package management tasks running, RUN THIS:

sudo rm /var/lib/dpkg/lock

#### 3. Then RUN DPKG Again: 
sudo dpkg -i kibana-8.10.2-amd64.deb



## Latest Kibana Debian package Can be installed from the website:

https://www.elastic.co/guide/en/kibana/current/deb.html#deb

.

.

.

.

.

# Creating Auth for Kibana

sudo apt-get install -y apache2-utils

#### You can create the .htpasswd file using the htpasswd utility. Run the following command to create the file and add a user (replace your-username with the desired username):

sudo htpasswd -c /etc/nginx/.htpasswd your-username

### Ensure .htpasswd file has Proper Ownership and Permissions so that Nginx can read it.

sudo chown www-data:www-data /etc/nginx/.htpasswd

sudo chmod 644 /etc/nginx/.htpasswd

sudo systemctl reload nginx


.

.

.

.

.

# Configure the Nginx for Kibana. 

sudo nano /etc/nginx/sites-available/kibana

### Then Past This Configuration 

Change server_name with your public ip or Host Name


```nginx
server {
    listen 80;
    server_name your_public_ip;       # Change it with your Public IP or Host Name

    location / {
        auth_basic "Restricted Access";          # Displayed to users as the login prompt
        auth_basic_user_file /etc/nginx/.htpasswd;  # Location of the password file

        proxy_pass http://localhost:5601;  # Kibana's default port
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }

    access_log /var/log/nginx/kibana-access.log;
    error_log /var/log/nginx/kibana-error.log;
}
```
 ### Reload Nginx for Apply All Changes 
sudo systemctl reload nginx

.

.

.

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
.

.

.

.

## Install Filebeat

curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-8.10.2-amd64.deb

sudo dpkg -i filebeat-8.10.2-amd64.deb


