 
history
history -c
sudo !5
 
sudo kill -9 <processid>
 
sudo apt install net-tools
netstat -tulpn (to see which ports are listening)

history > history_for_print.txt

---
sudo apt install nginx 
service nginx status(nginx is usually working when install)
service nginx start (to start nginx)
 - navigate to localhost in web browser to see if running
service nginx stop (to stop nginx)

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
To set up more than one server in AWS download nginx on first server and that would be load balancer. Start up 3 mirror image instances in AWS, name them webserver 1, webserver2, webserver and install nginx on all 3 instances (check if there is a easier way to do that other then doing them one by one? ? ?) 

- make sure instances are in the same security group
---
- Get IP addresses of load balancer and do some host file modification
- sudo vim /etc/hosts
-Paste in <IP-address> loadbalancer/
- type loadbalancer into browser and you can go to the nginx welcome page. (1:10)
- repeat for webservers

In load balancer terminal, copy sites-available/default file just in case you break something. (sudo cp default default.bak)

---
Download Terraform

cd Downloads

unzip terraform_0.12.25_linux_amd64.zip

./terraform

which ls

sudo cp terraform/bin/

terraform

vim ec2.tf
```
provider "aws" {
  profile    = "default"
  region     = "us-east-1"
}

resource "aws_instance" "example" {
  ami           = "ami-2757f631"
  instance_type = "t2.micro"
}

```

terraform init (make sure you are in the same file as .tf file)
terraform fmt
terraform validate
terraform plan
terraform apply


