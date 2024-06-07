## Application Load Balancer (aws-ipranges-alb)

## Deployment via CloudFormation console
Download [`aws-ipranges-alb.yaml`](aws-ipranges-alb.yaml) file and login to AWS [CloudFormation console](https://console.aws.amazon.com/cloudformation/home#/stacks/create/template). Choose **Create Stack**, **Upload a template file**, **Choose File**, select `aws-ipranges-alb.yaml` and choose **Next**.

### CloudFormation Parameters
Specify a **Stack name** and adjust parameters values as desired. Parameters options include

ALB API
- `awsServices`: Names of AWS service to return by root URL separated by commas. Default is `CLOUDFRONT_ORIGIN_FACING`
- `allowIPv4prefix`: Source IPv4 prefix allowed to access ALB. Default is `0.0.0.0/0`
- `allowIPv6prefix`: Source IPv6 prefix allowed to access ALB. Default is `::/0`

Lambda
- `pythonRuntime`: Python [runtime version](https://docs.aws.amazon.com/lambda/latest/dg/lambda-python.html). Default is `python3.12`
- `cpuArchitecture`: [instruction set architecture](https://docs.aws.amazon.com/lambda/latest/dg/foundation-arch.html), either `x86_64` or `arm64`. Default is `arm64`
- `lambdaFunctionName`: unique Lambda function name

Load Balancer
- `albScheme`: ALB scheme, either `internet-facing` or `internal`. An internet-facing load balancer routes requests from clients to targets over the internet. An internal load balancer routes requests to targets using private IP addresses. Default is `internet-facing`
- `targetGroupName`: unique [target group](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-target-groups.html) name

Networking
- `ipAddressType`: [IP address type](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/application-load-balancers.html#ip-address-type), either `IPv4`, `IPv4-and-IPv6` or `IPv6`. Default is `IPv4`
- `vpc`: [VPC](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) to deploy ALB
- `subnets`: [subnets](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/application-load-balancers.html#subnets-load-balancer) for ALB. Select at least 2 AZ subnets

[Optional] HTTPS listener

This section is optional
- `certificateArn`: Certificate ARN for [HTTPS listener](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/create-https-listener.html). Leave blank not to create HTTPS listener
- `securityPolicy`: [Security policy](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/create-https-listener.html#describe-ssl-policies) for HTTPS listener. Default is `ELBSecurityPolicy-TLS13-1-2-2021-06`
- `redirectHTTPtoHTTPS`: option to redirect HTTP requests to HTTPS. Default is `No`
- `sendHSTSheader`: option to send [HSTS (HTTP Strict Transport Security)](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html) header over HTTPS. Default is `No`

Continue **Next** with [Configure stack options](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-add-tags.html), [Review](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-using-console-create-stack-review.html) settings, and click **Create Stack** to launch your stack. 

After stack has been successfully created, its status changes to **CREATE_COMPLETE**. 

### CloudFormation Outputs
The following are available in `Outputs` section 
- `albDnsName`: ALB domain name. Create a DNS CNAME or [Route 53 alias](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-choosing-alias-non-alias.html) to this value especially if you are using HTTPS listener
- `albConsole`: ALB console URL
- `lambdaFunctionLog`: CloudWatch log URL for Lambda function

## ALB Customisation
Refer to [Application Load Balancer documentation](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html) for customisation options. Some examples include
- [Routing traffic to an ELB load balancer](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-elb-load-balancer.html)
- [Application Load Balancers and AWS WAF](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/application-load-balancers.html#load-balancer-waf)
- [Mutual authentication with TLS in Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/mutual-authentication.html)
- [Access logs for your Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-access-logs.html)
- [Connection logs for your Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-connection-logs.html)

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

