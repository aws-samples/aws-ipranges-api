## AWS-IPRanges-API

[Serverless](https://aws.amazon.com/serverless/) website that outputs AWS IP prefixes as web feeds for use by firewalls and other applications. Allows IP address lookup of corresponding Region, service and network border group. 



## Description
AWS [publishes](https://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html) its IP ranges in json format through [ip-ranges.json](https://ip-ranges.amazonaws.com/ip-ranges.json). The IP prefixes are commonly used by network firewalls for inbound and/or outbound network access control. 

A common use case is [Amazon CloudFront](https://aws.amazon.com/cloudfront/) with on-premise web server as origin; only [CloudFront origin facing](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/LocationsOfEdgeServers.html) (and if in use [Route 53 Health Checks](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/route-53-ip-addresses.html)) IP prefixes are allowed inbound access at perimeter network firewall. Another use case is for [egress control](https://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html#aws-ip-egress-control) where firewall administrators allow-list or deny-list outbound network acess to all or specific AWS service or Region such as [Route 53 name servers](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/route-53-ip-addresses.html) or [Amazon S3](https://aws.amazon.com/premiumsupport/knowledge-center/s3-find-ip-address-ranges/)

Typically, the firewall administrator will extract out required IP prefixes from ip-ranges.json for use in firewall rules. The IP prefixes need to be updated whenever there are changes and users can subscribe to [AWS IP address ranges notifications](https://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html#subscribe-notifications)

This project makes IP prefixes available as web feeds for automatic updates by firewalls. Users have the option to filter IP prefixes by service, Region and network border group, and in combined IPv4 and IPv6, IPv4 only or IPv6 only format. Entire solution is serverless and can be deployed with a single CloudFormation template. 

Solution is mentioned in blog post [How to enhance CloudFront origin security of on-premise web servers using third-party firewalls](https://aws.amazon.com/blogs/networking-and-content-delivery/how-to-enhance-cloudfront-origin-security-of-on-premise-web-servers-using-third-party-firewalls/)

## Architecture Diagram
<img width="1340" alt="image" src="aws-ipranges-api.png">



## Deployment Options
This solution website can be implemented using different AWS Services.
Click on link below for deployment details:
- [HTTP API](aws-ipranges-api.md) Gateway
- [Application Load Balancer](aws-ipranges-alb.md)
- [CloudFront](aws-ipranges-cloudfront.md)


## Firewall Setup
Firewalls that support external IP prefixes web feeds include (but not limited to)
- CheckPoint: [Updatable Objects (built-in feature)](https://support.checkpoint.com/results/sk/sk131852) or [External Network Feeds](https://sc1.checkpoint.com/documents/R81.20/WebAdminGuides/EN/CP_R81.20_SecurityManagement_AdminGuide/Content/Topics-SECMG/Network_Feed.htm)
- Cisco Secure Firewall: [Custom Security Intelligence Feeds (Network)](https://www.cisco.com/c/en/us/td/docs/security/secure-firewall/management-center/device-config/740/management-center-device-config-74/objects-object-mgmt.html#ID-2243-00000278)
- FortiGate: [Threat Feeds (IP Address)](https://docs.fortinet.com/document/fortigate/7.4.0/administration-guide/891236)
- Juniper: [Custom Feed (Dynamic Address)](https://www.juniper.net/documentation/us/en/software/nm-apps23.1/policy-enforcer-user-guide/topics/task/custom-feeds-creating.html)
- OPNsense: [Aliases (URL Tables (IPs))](https://docs.opnsense.org/manual/aliases.html)
- Palo Alto Networks: [External Dynamic List (IP Address)](https://docs.paloaltonetworks.com/pan-os/11-1/pan-os-admin/policy/use-an-external-dynamic-list-in-policy/configure-the-firewall-to-access-an-external-dynamic-list)
- pfSense: [Aliases (URL Tables (IPs))](https://docs.netgate.com/pfsense/en/latest/firewall/aliases.html)

Refer to vendor website documentation for configuration steps.

## Output options
Use different URLs to return IP prefixes and other values. Example
  - `/` : return CloudFront origin facing prefixes, customizable via `awsServices` in CloudFormation template and Lambda function `SERVICES` environment variable
  - `/SERVICE` : listing of available services
  - `/REGION` : listing of available Regions
  - `/NETWORK` : listing of network border groups which are a unique set of Availability Zones or [Local Zones](https://aws.amazon.com/about-aws/global-infrastructure/localzones/) from where AWS advertises IP addresses
  - `/SERVICE/<SERVICE>` : prefixes for specific SERVICE, e.g. `/SERVICE/CLOUDFRONT_ORIGIN_FACING`
  - `/SERVICE/<SERVICE>/<REGION>` : prefixes for specific SERVICE and REGION, e.g. `/SERVICE/EC2/us-east-1`
  - `/REGION/<REGION>` : prefixes for specific REGION, e.g. `/REGION/ap-southeast-1`
  - `/NETWORK/<NETWORK>` : prefixes for specific network border group, e.g. `/NETWORK/us-east-1-nyc-1`
  - `/SEARCH/<IP ADDRESS>`: IPv4 or IPv6 address to query, e.g. `/SEARCH/13.34.96.200`. This will return any matching entries, e.g.
    ```
    ip_prefix,region,service,network_border_group
    13.34.96.224/27,ap-southeast-1,AMAZON,ap-southeast-1
    ```
  - `/createDate` :  ip-ranges.json publication date and time, in UTC YY-MM-DD-hh-mm-ss format
  - `/syncToken` :  ip-ranges.json publication time, in Unix epoch time format

For IP prefixes, you can append `/ipv4.txt` or `/ipv6.txt` to filter by IPv4 or IPv6 respectively, e.g. `/SERVICE/ROUTE53_HEALTHCHECKS/ipv4.txt`

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

