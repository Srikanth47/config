## When ever there is new resource found on the aws region The below automation will help to gives us an email notification alerts

**Required AWS resources to automate this**

1) AWS config
2) CloudWatch Event rule
3) SNS topic

![work2](https://user-images.githubusercontent.com/2727726/133082625-51fc2b1c-3eb7-462a-8a78-5e0dafd5656d.png)


1) AWS Config will keeps on monitoring the resources, when ever there is new resources or change to the resource is it will keep tracking those resources

2) CloudWatch event will have a rule defined, which will take action and triggers the SNS.

Here is the CloudWatch Event pattern rule we configured, which will be deployed using terraform code

` {
  "source": [
    "aws.config"
  ],
  "detail-type": [
    "Config Configuration Item Change"
  ],
  "detail": {
    "messageType": [
      "ConfigurationItemChangeNotification"
    ],
    "configurationItem": {
      "resourceType": [
        "AWS::EC2::Instance",
         "AWS::EC2::VPC",
         "AWS::S3::Bucket"
      ],
      "configurationItemStatus": [
        "ResourceDiscovered"
      ]
    }
  }
} `

3) SNS topic and subscription list already been deployed using terraform code 

` resource "aws_sns_topic_subscription" "email-target" {
  topic_arn = aws_sns_topic.aws_logins.arn
  protocol  = "email"
  count = length(var.sns_subscription_email_address_list)
  endpoint  =  var.sns_subscription_email_address_list[count.index]
  }
`
## Deployment steps:

`$CD to WorkSpace`

*to add email subscriptions to get the notifications*

`$vi varible.tf`  

change line according to req default = ["example@gmail.com", "example2@gmail.com"]
  
`$terraform init`

*to verify/check what are all the new resources are added*

`$terraform plan` 

`$terraform apply -auto-approve`


