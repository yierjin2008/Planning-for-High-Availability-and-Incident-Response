# Infrastructure

## AWS Zones

Zone1: primary site in us-east-2
Zone2: secondary site in us-west-1

## Servers and Clusters

### Table 1.1 Summary

| Asset                  | Purpose                           | Size         | Qty         | DR                        |
| ---------------------- | --------------------------------- | ------------ | ----------- | ------------------------- |
| EC2 instances          | running websites                  | t3.micro     | 3 instances | deployed to DR site       |
| SSH key                | administrating EC2                | pem key      | 1           | stored in seperate region |
| Github repo            | store terraform code              |              | 1           |                           |
| Route 53               | DNS management                    |              | 1           | deploy to DR              |
| load balancer (ALB)    | banlance load / trafic to website |              | 1           | deploy to DR              |
| Kubernetes clustser    | monitoring stack                  |              | 2 nodes     | deploy to DR              |
| Grafana and Prometheus | monitoring platform               |              | 1           | deploy to DR              |
| Database               | backend database for website      | aws RDS      | 2 nodes     | deploy to DR              |
| Database backup        | restore databse                   | stored in s3 | daily       | deploy to DR              |

### Descriptions

- 3 EC2 instances running the website, in the same region (us-east-2) but are in different AZ
- SSH keys for administering the EC2 instances, should be stored in a different region in case of the zone failure
- GitHub repo for storing the Terraform code, version controled, run automatically or mannually
- AWS route53 to manage DNS and manage DNS during failover
- Application Load balancer for the website, point to primary site and will failover to secondary DR site when primary is not available
- Kubernetes cluster for monitoring stack, including 2 nodes and are deployed in different AZ
- Monitoring platform (Grafana and Prometheus) for the web application deployed into the Kubernetes cluster
- Backend database running on 3 Amazon RDS nodes for the website, nodes deployed to different AZ. Database is replicated to a second zone2 (us-west-1) for DR
- Database backups stored in S3 for recovery, perform daily backup, backup retention window is 5 days

## DR Plan

### Pre-Steps:

Ensure both sites are set up the same and working properly.

## Steps:

1. During a failover scenario, you would fail over the single DNS entry from route53 to point to the DR site.
2. Failover your database replication instances to another region by:
   Manually force the secondary region to become primary at the database level, or
   Automatically failover the database by health checks
