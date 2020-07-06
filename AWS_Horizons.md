1. Intro
2. Global Infrastructure
3. Intro to IAM + AWS Shared Responsibility Model
4. EC2
5. EC2 + Nginx
6. Load Balancers
7. AWS S3 +Static Website Hosting
8. Content Delivery Networks + Cloudfront
- CDN are geographically distributed group of servers which work together to provide fast delivery of Internet content. These servers are called **edge locations**
- Cloudsearch functions by caching(saving/remembering) the first request made to an origin. All subsequent requests will be served from the edge locations.
- Cloudfront Orgin - A Location where content is stored
#### - A Cloudfront Distribution - A distribution tells CloudFront where you want content to be delivered from, and the details about how to track and manage content delivery.
- Cache Behaviors describe what and how content is cached in cloudfront.
- TTL (Time to Live) is a setting for each cache behavior that specifies how long cloudfront is supposed to cache (or remember) the that cache behavior before it expires and a new one needs to be done
- Cache Invalidation - Sometime you need to “clear” the cache before the TTL has expired.
9. Cloud Deployment Strategies
  - Recreate
  - Highlander/Big Bang
  - Canary
  - A/B Testing
  - Rolling
  - Blue Green
10. On Prem, Hybrid Cloud and RDS
11. Elastic Beanstalk
12. Infrastructure as Code with Cloudformation
13. DNS + Route53
14. Cloudwatch, Systems Manager and Billing:  Application and Infrastructure Monitoring
  - CloudWatch collects monitoring and operational data in the form of logs, metrics, and events
  - Observability of your AWS resources and applications on AWS and on-premises
  - Parse Logs
  - Observe metrics like CPU usage and memory usage
  -  Amazon CloudWatch Events delivers a near real-time stream of system events that describe changes in Amazon Web Services (AWS) resources. You can configure the following AWS services as targets for CloudWatch Events: Amazon EC2 instances, AWS Lambda functions, Streams in Amazon Kinesis Data Streams, Log groups in Amazon CloudWatch Logs, Amazon ECS tasks, Systems Manager Automation,  cron jobs

The default event bus of another AWS account
15. Security
16. Engineering Panel
