# Pcap-Analyzer

## Update Notes
+ Remove the item from Python2.X Transplanted to Python3.X
+ Multiple fixed Bug

## Key features:
+ 1.Displays the basic information of the packet
+ 2.Analyze the packet protocol
+ 3.Analyze packet traffic
+ 4.Draw a map of the latitude and longitude of the access IP
+ 5.Extract the session connection for a specific protocol in the packet（WEB，FTP，Telnet）
+ 6.Extract sensitive data (passwords) from a session
+ 7.SIMPLE ANALYSIS OF SECURITY RISKS IN PACKETS (WEB ATTACKS, BRUTE FORCE ATTACKS)
+ 8.Extract the transfer file or all binary files in a datagram for a specific protocol

## Effect display
### Home:
![Alt Text](https://github.com/HatBoy/Pcap-Analyzer/blob/master/images/index.png)

### Basic data display:
![Alt Text](https://github.com/HatBoy/Pcap-Analyzer/blob/master/images/basedata.png)

### Protocol Analysis:
![Alt Text](https://github.com/HatBoy/Pcap-Analyzer/blob/master/images/protoanalyxer.png)

### Traffic analysis:
![Alt ​​Text](https://github.com/HatBoy/Pcap-Analyzer/blob/master/images/flowanalyzer.png)

### Access IP latitude and longitude map:
![Alt ​​Text](https://github.com/HatBoy/Pcap-Analyzer/blob/master/images/ipmap.png)

### Session extraction:
![Alt ​​Text](https://github.com/HatBoy/Pcap-Analyzer/blob/master/images/getdata.png)

### Attack information warning:
![Alt ​​Text](https://github.com/HatBoy/Pcap-Analyzer/blob/master/images/attackinfo.png)

### File extraction:
![Alt ​​Text](https://github.com/HatBoy/Pcap-Analyzer/blob/master/images/getfiles.png)

## Installation and deployment process:

+ Operating environment: Python 3.5.X
+ Operating system: Linux (Ubuntu 15.10 as an example)

### 1. Python related environment configuration (Ubuntu installs Python 2.7 by default, no need to install Python separately)
Python package manager installation: sudo apt-get install python-setuptools python-pip

### 2. Related third-party dependency library installation:
+ sudo apt-get install tcpdump graphviz imagemagick python-gnuplot python-crypto python-pyx
+ sudo pip3 install Flask
+ sudo pip3 install Flask-WTF
+ sudo pip3 install geoip2
+ sudo pip3 install pyx
+ sudo pip3 install requests
+ For the installation of scapy, please refer to the official documentation. The version of scapy is 2.4.0. There are major changes after 2.4.0, which may cause incompatibility

### 3. Modify the configuration file
Note that the directory location in the config.py configuration file is modified
+ UPLOAD_FOLDER = '/home/dj/PCAP/' The location where the uploaded PCAP file is saved
+ FILE_FOLDER = '/home/dj/Files/' The location where the file is saved when it is extracted. There must be All, FTP, Mail, and Web subdirectories below to store files extracted from different protocols
+ PDF_FOLDER = '/home/dj/Files/PDF/' The location where PCAP is saved when it is saved as PDF

### 4. Server installation
+ Gunicorn server: pip3 install gunicorn
+ Nginx server: sudo apt-get install nginx
+ Nginx configuration: Modify the /etc/nginx/nginx.conf file and add the following code in http{}:
```
server {
listen 81;
server_name localhost;
access_log /var/log/nginx/access.log;
error_log /var/log/nginx/error.log;

location / {
#root html;
#index index.html index.htm;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header Host $http_host;
proxy_pass http://127.0.0.1:8000;
}

error_page 500 502 503 504 /50x.html;
location = /50x.html {
root html;
}
```

### 5. Start the system:
+ Enter the directory where the system is located: ../pcap-analyzer
+ Start the system through the Gunicorn server, run the command: gunicorn -c deploy_config.py run:app
+ At this time, the system can only be accessed locally, address: http://127.0.0.1:8000
+ Start the Nginx server: sudo service nginx start
+ At this time, other hosts can also access the system, address: http://serverIP:81


## Analysis Optimization
### The accuracy of the data packet analysis results can be improved by modifying the configuration file, and the correction
+ Replacing the IP address latitude and longitude database file of ./app/utils/GeoIP/GeoLite2-City.mmdb can improve the accuracy of the IP latitude and longitude map
+ Modifying the representation number and corresponding protocol name of each TCP/IP protocol stack in the ./app/utils/protocol/ directory can correct the protocol analysis results
+ Modifying the ./app/utils/waring/HTTP_ATTACK file can improve the accuracy of HTTP protocol attacks in data packets
