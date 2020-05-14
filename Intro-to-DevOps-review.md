 
history
history -c
sudo !5
 
sudo kill -9 <processid>
 
sudo apt install net-tools
netstat -tulpn (to see which ports are listening)

history > history_for_print.txt

In computing, ".bak" is a filename extension commonly used to signify a backup copy of a file.Database Applications like FoxPro and SQL Server use .bak files to back up their databases and other applications, like XML shell, create .bak files in their autosave process.[1] They do not get automatically deleted, so they need to be manually deleted after the process using it is stopped.

---
# Create and Start Web Server Using Nginx
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
# Set up Load Balancer
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
# Use Terraform to spin up multiple AWS instances at once
Terrafrom is an open-source infrastructure as code software tool created by HashiCorp. It enables users to define and provision a datacenter infrastructure using a high-level configuration language

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
  profile = "default"
  region  = var.region
}


resource "aws_instance" "ansible-control-server" {
  ami           = var.mastermnd_ami
  instance_type = var.mastermnd_instance
  tags = {
    Name = "ansible-control"
  }
}

resource "aws_instance" "ansible-web-servers" {
  count         = length(var.web_servers)
  ami           = var.mastermnd_ami
  instance_type = var.mastermnd_instance
  tags = {
    Name = element(var.web_servers, count.index)
  }
}

```
vim var.tf

```
variable "region" {
  default = "us-east-1"
}

variable "mastermnd_ami" {
  default = "ami-07ebfd5b3428b6f4d"
}

variable "mastermnd_instance" {
  default = "t2.micro"
}

variable "web_servers" {
  type = list(string)
  default = [
    "ansible-web-1",
    "ansible-web-2",
    "ansible-web-3",
  ]
}
```

terraform init (make sure you are in the same file as .tf file)

terraform fmt

terraform validate

terraform plan

terraform apply
---
# Use Ansible to configure multiple AWS instances at once
Ansible is an open-source software provisioning, **configuration management**, and application-deployment tool.

```
---
# This playbook deploys the whole application stack in this site.

# Apply common configuration to all hosts
- hosts: all

  roles:
  - common

# Configure and deploy database servers.
- hosts: dbservers

  roles:
  - db

# Configure and deploy the web servers. Note that we include two roles
# here, the 'base-apache' role which simply sets up Apache, and 'web'
# which includes our example web application.

- hosts: webservers

  roles:
  - base-apache
  - web

# Configure and deploy the load balancer(s).
- hosts: lbservers

  roles:
  - haproxy

# Configure and deploy the Nagios monitoring node(s).
- hosts: monitoring

  roles:
  - base-apache
  - nagios
```
Ansible.yaml
```
- hosts: webservers
  gather_facts: yes
  become: true
  become_user: root
  tasks:
  - name: Install Nginx
    apt: update_cache=yes pkg=nginx state=present
    notify:
    - restart nginx
  - name: Enable Nginx during boot
    service: name=nginx state=started enabled=yes
  handlers:
    - name: restart nginx
      service: name=nginx state=restarted

hosts:dbservers
become_user: root
tasks:
- name: pkg=mysql-server state=present
```
