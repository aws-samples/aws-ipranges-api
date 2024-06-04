## Amazon API Gateway (aws-ipranges-api)

## Deployment via CloudFormation console
Download [`aws-ipranges-api.yaml`](aws-ipranges-api.yaml) file and login to AWS [CloudFormation console](https://console.aws.amazon.com/cloudformation/home#/stacks/create/template). Choose **Create Stack**, **Upload a template file**, **Choose File**, select `template.yaml` and choose **Next**.

### CloudFormation Parameters
Specify a **Stack name** and adjust parameters values as desired. Parameters options include

HTTP API
- `allowNetworks`: Source IP prefixes that are authorized to use API separated by commas. Default is **0.0.0.0/0**
- `awsServices`: Names of AWS service to return by root URL separated by commas. Default is **CLOUDFRONT_ORIGIN_FACING**

Lambda
- `pythonRuntime`: Python [runtime version](https://docs.aws.amazon.com/lambda/latest/dg/lambda-python.html). Default is **python3.12**
- `cpuArchitecture`: Instruction set architecture (x86_64 or arm64). Default is **arm64**
- `lambdaFunctionName`: Lambda function name. Default is **aws-ipranges-API**
- `lambdaAuthorizerFunctionName`: Lambda authorizer function name. Default is **aws-ipranges-API-authorizer**

[Optional] Custom domain name

This section is optional
- `customDomainName`: [custom domain name](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-custom-domain-names.html) for your API
- `certificateArn`: ARN of [ACM certificate](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-custom-domain-names.html#http-api-custom-domain-names-certificates)
- `disableDefaultEndPoint`: option to [disable default endpoint](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-disable-default-endpoint.html). This ensures that clients can only access API by using specified custom domain name

Continue **Next** with [Configure stack options](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-add-tags.html), [Review](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-using-console-create-stack-review.html) settings, and click **Create Stack** to launch your stack. 

After stack has been successfully created, its status changes to **CREATE_COMPLETE**. 

### CloudFormation Outputs
The following are available in `Outputs` section 
- `apiGatewayInvokeURL` (if  `disableDefaultEndPoint` is false ): URL of format `https://<api-id>.execute-api.<region>.amazonaws.com`) for use by firewall. Refer to [Output options](#output-options) below for details.
- `apiFQDN` (if `customDomainName` is specified): Create a DNS CNAME or [Route 53 alias](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-choosing-alias-non-alias.html) record of your `customDomainName` to this value

- `apiGatewayLog`: CloudWatch log URL for API Gateway
- `lambdaFunctionLog`: CLoudWatch log URL for Lambda function
- `lambdaAuthorizerFunctionLog`: CloudWatch log URL for Lambda authorizer function


## API Gateway Customisation
Refer to [Amazon API Gateway documentation](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html) for HTTP API customisation options. Some examples include
- [Setting up custom domain names](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-custom-domain-names.html)
- [Routing traffic to an Amazon API Gateway API by using your domain name](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-api-gateway.html)
- [Configuring mutual TLS authentication](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-mutual-tls.html)
- [Throttling requests to your HTTP API](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-throttling.html)
- [Working with AWS Lambda authorizers for HTTP APIs](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-lambda-authorizer.html): Lambda authorizer is used to limit access by source IP prefixes

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

