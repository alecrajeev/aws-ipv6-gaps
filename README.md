# AWS Services That Do Not Support IPv6

Now that AWS [is charging per IPv4 address](https://aws.amazon.com/blogs/aws/new-aws-public-ipv4-address-charge-public-ip-insights/) as of February 2024, suddenly lack of IPv6 support has economic consequences for AWS customers. Thus, the reason for this page is to highlight IPv6 gaps across AWS's service offerings.

| Service | Corey's Commentary |
| ---- | ---- |
| VPC Reachability Analyzer | Networks break; if you want to figure out why, you'd best be using an IPv4 stack. Even AWS doesn't know how to charge you to fix your IPv6 problems... |
| Load Balancers | Yes, these support IPv6 in dual-stack configuration--but the price of each of these just went up by $3.50 a month for every AZ they have a listener hanging out within. Network Load Balancers don't listen on UDP if they're set to dual stack, Classic ELBs only support IPv6 if they're in the now-deprecated EC2 Classic, and Application Load Balancers seem... mostly okay? Gateway Load Balancers have no use case that I have discovered yet. Yes, I know, today almost nobody needs a pure IPv6 load balancer, but the future is coming.|
| Gateway Endpoints | If you want the only two services that offer gateway endpoints in private Subnets (S3 and DynamoDB) to continue to not incur significant charges when passing through a Managed NAT Gateway, these things need to support IPv4 to get you there; in an IPv6 only environment you theoretically don't need these as NAT isn't a concern, but in dual-stack environments I feel like a lot of stuff is going to bias for the 4.5¢ per GB option rather than the free one. |
| EC2 Instance Connect | EC2 Instance Connect doesn't support IPv6, full stop. That means that for this to work you've gotta use IPv4, and also get yourself into the privately-addressed range via VPN, Direct Connect, Arcane Prayers, etc. |
| Auto Scaling Groups | Auto Scaling Groups can only register/de-register instances to target groups via 'instance-id' and not 'ip-address', and 'instance-id' only supports IPv4 targets. |
| Systems Manager | The SSM Agent requires IPv4 access to Systems Manager endpoints in order to function |
| Kinesis/SQS | Examples of possible high volume AWS services, not supporting IPv6.  Requiring you to make the trade of between paying $3.50 a month for every ec2 instance using it, or spawning a VPC endpoint at 10.5$ a month for every AZ and 0.01$ per GB.  And do not forget to update all your code/config to use these new endpoints. |
| App Runner | No option to select IPv6.  Defaults to IPv4. |

