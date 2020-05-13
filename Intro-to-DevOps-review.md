 
 history
 sudo !5
 sudo kill -9 <processid>
  
 sudo apt install nginx 
 service nginx status(nginx is usually working when install)
 service nginx start (to start nginx)
  - navigate to localhost in web browser to see if running
 service nginx stop (to stop nginx)
 
 sudo apt install net-tools
 netstat -tulpn (to see which ports are listening)
 
 cd /etc
 ls
 ll
 cd nginx
 ll
 cd sites-available
 ll
 vim default (sudo apt install vim)
 - server_name example.com;
 - /var/www/example.com; 
   index index.html;
 
 cd sites-enabled (You can store file sites-avaiable/ and symlink that to sites-enabled/ to enable it.)
 
 ll
 cd /var/www/html
 - (/var contains things that are prone to change, such as websites, temporary files (/var/tmp) and databases.)
 ll
 cat (vim) index nginx-debian.html
 ---
 To set up more than one server in AWS download nginx on first server and that would be load balancer. Start up 3 mirror image instances in AWS, name them webserver 1, webserver2, webserver and install nginx on all 3 instance (check if there is a easier way to do that other then doing them one by one? ? ?) 
 
- make sure instances are in the same security group
---
- Get IP addresses of load balancer and do some host file modification
- sudo vim /etc/hosts
-Paste in <IP-address> loadbalancer/
 - type loadbalancer into browser and you can go to the nginx welcome page. (1:10)
 - repeat for webservers
  
  In load balancer terminal, copy sites-available/default file just in case you break something. (sudo cp default default.bak)
  
 
 
