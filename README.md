When ever there is new resource find out on the aws region The below automation will help to gives us an email notiofication alerts

Required AWS resources to automate this

1) AWS config
2) CloudWatch Event rule
3) SNS topic



1) AWS Config will keeps on monitoring the resources, when ever there is new resources or change to the resource is it will keep tracking those resources

2) CloudWatch event will have a rule define, which will take action to send out the SNS.

Here is the CloudWatch Event pattern rule we configured, which will be deoloyed using terraform code

{
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
}


3) SNS topic and subscription list already been deployed using terraform code 

resource "aws_sns_topic_subscription" "email-target" {
  topic_arn = aws_sns_topic.aws_logins.arn
  protocol  = "email"
  count = length(var.sns_subscription_email_address_list)
  endpoint  =  var.sns_subscription_email_address_list[count.index]
  }
