## AWS-IPRanges-API

[Serverless](https://aws.amazon.com/serverless/) web portal based on [Amazon API Gateway](https://aws.amazon.com/api-gateway/) and [AWS Lambda](https://aws.amazon.com/lambda/) that output IP Prefixes from AWS ip-ranges.json for use by firewalls and other applications

## Description
AWS [publishes](https://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html) its IP ranges in json format through [ip-ranges.json](https://ip-ranges.amazonaws.com/ip-ranges.json). The IP prefixes are commonly used by firewalls for network access control. A common use case is [CloudFront](https://aws.amazon.com/cloudfront/) with origin at customer's on-premise web server. A recommended best practice is for customers to [protect origin](https://docs.aws.amazon.com/whitepapers/latest/secure-content-delivery-amazon-cloudfront/protecting-your-origin-by-allowing-access-to-cloudfront-only.html) by only allowing CloudFront (and if needed Route 53 Health Checks) IP address ranges inbound access to web server through origin firewall policies.

Typically, the firewall administrator will extract out desired IP prefixes from ip-ranges.json for use in their firewall rules. The IP prefixes need to be updated whenever there are changes. 

This project simplifies the extraction process by making IP prefixes accessible as a web feed for dynamic updates by firewalls. Users have the option to filter the IP prefixes by service, region and network border group (local zones) and in combined IPv4 and IPv6, IPv4 only or IPv6 format. Entire solution is serverless, and can be easily deployed via a single CloudFormation file. 

## Architecture Diagram
<img width="1340" alt="image" src="https://user-images.githubusercontent.com/88474310/155283397-b34594ea-213d-4b8f-b391-6081087f1743.png">


## CloudFormation Setup
Login to AWS [CloudFormation console](https://console.aws.amazon.com/cloudformation) and use [Create Stack](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-create-stack.html) to upload the .yaml file. The template has default values that you can change
- awsServices: Name of AWS services to return by root URL separated by commas. Default value is **CLOUDFRONT_ORIGIN_FACING,ROUTE53_HEALTHCHECKS**
- lambdaFunctionName: Lambda function name. Default value is **IPRanges-2-apiGW**
- pythonRuntime: Python 3 runtime version. Default is **python3.9**
- cpuArchitecture: Instruction set architecture (x86_64 or ARM64). Default is **x86_64**

Go to Outputs tab to get **apiGatewayInvokeURL** value (in the form https://\<VALUE\>.execute-api.\<REGION\>.amazonaws.com) for use by firewall

  
## Firewall Setup
Firewalls that supports IP prefixes updates via web URL include (but not limited to)
- Palo Alto: [External Dynamic List](https://docs.paloaltonetworks.com/pan-os/9-1/pan-os-admin/policy/use-an-external-dynamic-list-in-policy/external-dynamic-list) 
- Fortigate: [External Threat List](https://docs.fortinet.com/document/fortigate/6.2.0/new-features/625349/external-block-list-threat-feed-policy).
- pfSense: [URLTables(IPs) Aliases](https://docs.netgate.com/pfsense/en/latest/firewall/aliases.html#url-aliases)
- OPNsense: [URL Tables (IPs)Aliases](https://docs.opnsense.org/manual/aliases.html)

Refer to vendor website documentation on configuration steps.
  
For Check Point users, AWS IP prefixes are available through [Updatable Objects](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk131852)

## Other options
Use different URLs to return IP prefixes. Example
  - / : return CloudFront origin facing and R53 Health Checks  prefixes: customizable via **SERVICES** Lambda environment variables
  - /SERVICE : listing of available services, e.g. S3
  - /REGION : listing of available regions, e.g. ap-southeast-1
  - /NETWORK : listing of network border groups, e.g. us-east-1-atl-1
  - /SERVICE/\<SERVICE\> : prefixes for specific SERVICE, e.g. /SERVICE/S3
  - /SERVICE/\<SERVICE\>/\<REGION\>: prefixes for specific SERVICE and REGION, e.g. /SERVICE/EC2/us-east-1
  - /REGION/\<REGION\> : prefixes for REGION, e.g. /REGION/eu-west-1
  - /NETWORK/\<NETWORK\> : prefixes for specific network border group, e.g. /NETWORK/us-east-1-nyc-1

Append /IPv4.txt or /IPv6.txt to limit IP prefixes to IPv4 or IPv6 respectively
 
## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

