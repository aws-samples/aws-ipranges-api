## Amazon CloudFront (aws-ipranges-cloudfront)


## Deployment via CloudFormation console
Download [`aws-ipranges-cloudfront.yaml`](aws-ipranges-cloudfront.yaml) file and login to AWS [CloudFormation console](https://console.aws.amazon.com/cloudformation/home#/stacks/create/template). Choose **Create Stack**, **Upload a template file**, **Choose File**, select `aws-ipranges-api-alb.yaml` and choose **Next**.

### CloudFormation Parameters
Specify a **Stack name** and adjust parameters values as desired. Parameters options include

CloudFront API
- `awsServices`: Name of AWS services to return by root URL separated by commas. Default is **CLOUDFRONT_ORIGIN_FACING**

Lambda
- `pythonRuntime`: Python [runtime version](https://docs.aws.amazon.com/lambda/latest/dg/lambda-python.html). Default is **python3.12**
- `cpuArchitecture`: Instruction set architecture (x86_64 or arm64). Default is **arm64**
- `lambdaFunctionName`: Lambda function name. Default is **aws-ipranges-api-alb**

CloudFront
- `enableIPv6`: Enable and disable IPv6. Default is `yes`
- `oacName`: name of [Origin Access Control (OAC)](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-lambda.html) to use with Lambda function URL origin
- `viewerProtocolPolicy`: configure viewer access by HTTP and/or HTTPS. Default is `redirect-to-https`
- `cachePolicy`: [managed cahce policy](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/using-managed-cache-policies.html). Default is `CachingDisabled`
- `responseHeaderPolicy`: [managed response headers policies](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/using-managed-response-headers-policies.html). Default is `SecurityHeadersPolicy`

Continue **Next** with [Configure stack options](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-add-tags.html), [Review](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-using-console-create-stack-review.html) settings, and click **Create Stack** to launch your stack. 

After stack has been successfully created, its status changes to **CREATE_COMPLETE**. 

### CloudFormation Outputs
The following are available in `Outputs` section 
- `cloudFrontURL`: CloudFront distribution domain name
- `cloudFrontConsole`: URL to CloudFront distribution console
- `lambdaFunctionLog`: CLoudWatch log URL for Lambda function


## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

