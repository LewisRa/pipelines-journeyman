# Diving into Distributed Systems: Pipelines 2/8

####  You just started at a new role at pretty cool startup. They're making some big waves, and growing, but they're initial Architecture and Infrastructure isn't really prepared for the future. They initially set everything up by hand on an on prem server. The disadvantage of having one server is if you overlod one application, it will negatively affect the other apps. 

We have 3 services on the on-prem server
- fulcrum - used for project management  and it coded with Ruby on rails with SQL. Specifically,the application provides a user story based backlog management system for agile development. 
- rocketchat - leading open source team chat software solution (Docker Compose yaml)
- gatsby start-up - node application 

In this class, we spun up 4 servers and decentalized (distributed) the on prem server to 4 AWS ec2 servers (nginx reverse proxy, fulcrm, gatsby, rocketchat). 

1. fulcrum - download ruby on rails, download project and run out of port 3000
2. rocket chat -  install docker compose, - install docker from website because most up to data, docker build, run out of port 3000

3. gatsy- instal npm, install node, run gatsy
4. nginx reverse proxy - security  rules for HTTP, HTTPS, chat.pipelines.com (rocketchat), blog.pipelines.com (gatsby), PM.pipelines.com(fulcrum),  var/www/
**LATER: we will have distributed fulcrum/rocketchat on frontend and bankend(db) on 2 servers. 

- Host file can be a simple solution for a DNS server that converts IP addresses --> domain

**How to run programs in the background indefinitely when you are not logged into the server but the server is still running?**--> daemon/ service 
- rails server -d
- forever? 
---
# Misc. 
- Only one application can run on a port a time. 
- Even though mongodb usually runs on port 3000, since it is on docker, we can run it on any port we want. 
