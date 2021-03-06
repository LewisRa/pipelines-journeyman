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

#  Software Development Lifecycle + Source Control Strategies: Pipelines 3/8
## Example: this docker file involves git and AWS: for example: build-webpack only runs when master branch are feature branch is pushed

![](https://github.com/LewisRa/pipelines-journeyman/blob/master/markdownImages/gitflow.PNG)

**Hot fixes- use hot fixes when a bug is found in production (master branch) but you're not ready to release other
features into production so you have a branch specifically for fixing bugs**
All feature branches are merge into the development branch


From development to release branches: let's say all three
features (A, B, and C) have been merged into
develop and they now exist on the
develop branch but we want to deploy we
want to create a release from there. Create a new branch called version 1.0.0
and this is gonna be our first version
so we create it and then merge that into
our release branch. We go from release branches to staging/UAT before we merge into master which is production to make sure
that everything is working and good to
go

The idea is anything that
gets merged into master is ultimately
what is deployed. You can set up
some automation, so that anything
that gets brought into master will
instantly deploy to the mobile app .e.
the moment we merge in to the master branch, anyone that has
our mobile app downloaded will get a
pop-up that says "hey,
version 1.0.2 is ready, would you
like to download it?" 

Protecting feature branches from development brach is our code reviews and pull requests so
before anything can be merged in we make
sure that multiple people have eyes on
it

and then going from a version branch
into release branch we make sure " is
this working? is this what we want to
actually test as a release?

 before we merge into master we
make sure that the production
environment ready to accept this latest
merge like have we set up all the
environment variables is everything good
to go
and then we can merge it in and it all
it all works 

#### Squash all my commits into one for GitHub pull request 

#### Staging/UAT has the same environment (same docker container, database, etc) as production/ master branch) unlike development branch 


![](https://github.com/LewisRa/pipelines-journeyman/blob/master/markdownImages/gitbranchPermission.PNG)

```
# AMAZON_ECR_ENDPOINT_LOWER=url-for-lower-ECR
# AMAZON_ECR_REPO=repo-name-within-ECR
# LOWER_AWS_KEY=
# LOWER_AWS_SECRET=
#
#
### Deployment Workflows:
#
# Push to Branch     | Deploy to Environment
# ------------------ | ---------------------
# integration        | mint
# staging            | staging
# master             | HOLD -> prod
#

version: 2.1

orbs:
  snyk: snyk/snyk@0.0.8

workflows:
  test-build-deploy:
    jobs:
    - cache-dependencies-lower:
        filters:
          branches:
            only:
            - /.*/
          tags:
            only:
            - mint
            - amy
            - pat
            - qa
            - /int-\w+/ 
    - cache-dependencies-upper:
        filters:
          branches:
            ignore:
            - /.*/
          tags:
            only:
            - staging
            - production
        context: snyk   
    - test-jest:
        requires: 
          - cache-dependencies-lower
          - cache-dependencies-upper
        filters:
          branches:
            only:
            - /.*/
          tags:
            only:
            - /.*/
    - test-poirot:
        requires: 
          - cache-dependencies-lower
          - cache-dependencies-upper
        filters:
          branches:
            only:
            - /.*/
          tags:
            only:
            - /.*/
    - test-mocha:
        requires: 
          - cache-dependencies-lower
          - cache-dependencies-upper
        filters:
          branches:
            only:
            - /.*/
          tags:
            only:
            - /.*/
    - test-format:
        requires:
          - cache-dependencies-lower
          - cache-dependencies-upper
        filters:
          branches:
            only:
            - /.*/
          tags:
            only:
            - /.*/    
    - build-webpack: #### only runs when master branch are feature branch is pushed 
        requires: [test-jest, test-mocha, test-format, test-poirot]
        filters:
          branches:
            only:
            - master
            - /[Ff]eature-\w+-\d+[-\w]*/
            - /snyk-fix-.*/
          tags:
            only:
            - production
            - staging
            - mint
            - amy
            - pat
            - qa
            - /int-\w+/
            - /RELEASE-\d+/
   ...
   ##########################################################################################################################
    - build-package:
        
    - upload-package:
    
    - deploy-package:
       
    - hold:
     
    - upload-package-with-approval:
       
    - deploy-package-with-approval:
     ##########################################################################################################################
....
  deploy-package-with-approval:
    docker:
      - image: ussba/cc-docker-git-aws:latest
    steps:
      - checkout
      - run:
          name: deploy
          shell: /bin/bash -eo pipefail
          command: |
            if [ -z "${CIRCLE_BRANCH}" ]
            then
              TARGET_ENVIRONMENT=${CIRCLE_TAG}
            else
              TARGET_ENVIRONMENT=${CIRCLE_BRANCH}
            fi
            export AWS_DEFAULT_REGION=${AWS_DEFAULT_REGION:-us-east-1}
            export AWS_DEFAULT_OUTPUT=${AWS_DEFAULT_OUTPUT:-json}
            export VERSION_ID=$(aws s3api list-object-versions --bucket ${BUCKET_NAME} --prefix "${TARGET_ENVIRONMENT}/katana-lambda.zip" | jq '.Versions[0].VersionId')
            export LAMBDA_NAME="${TARGET_ENVIRONMENT}-KatanaLambda"
            aws lambda update-function-code --function-name "${LAMBDA_NAME}" --s3-bucket "${BUCKET_NAME}" --s3-key "${TARGET_ENVIRONMENT}/katana-lambda.zip" --s3-object-version "${VERSION_ID}"
```
# Misc. 
- Only one application can run on a port a time. 
- Even though mongodb usually runs on port 3000, since it is on docker, we can run it on any port we want. 
