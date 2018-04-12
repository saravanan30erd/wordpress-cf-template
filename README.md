wordpress-cf-template
=====================

Overview
---------
 - Deploy a fault-tolerant web-server cluster, RDS cluster and jump host server.
 - The webserver and database cluster should supports High Availability and Fault Tolerance(Zero Down Time).
 - The jump host is publicly available to login and to access internal infrastructure.

 Architecture
 -------------
  - Network: VPC with multiple public subnets and multiple private subnets(with NAT Gateway).
  - Jump Host: A Jump host server runs on  public subnets.
  - Webserver: The webserver cluster runs in Autoscaling Group on private subnets and ASG            attached with Application Load Balancer which running in public network.
  - Database: The RDS Aurora cluster have a primary RDS instance with single Read Replica.

Notes
------
- We can choose either single nat gateway across all the AZ or multiple
  nat gateway(one per AZ). It is recommended to use NAT Gateway per
  AZ in production, because If you have resources in multiple AZ and
  they share one NAT gateway, in the event that NAT gateway’s AZ is
  down, then resources in the other Availability Zones lose internet access.
  To configure multiple nat gateway(per AZ),
    `set parameter NATGatewayPerAZ = true`
- We can choose the number of availability zones to use for this setup,
  you must match the Number of Availability Zones and Availability Zones parameter

    `Number of Availability Zones = 3`
    `Availability Zones = eu-west-1a,eu-west-1b,eu-west-1c`
