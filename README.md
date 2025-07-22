# Terraform-aws-sns

# Terraform AWS Cloud sns Module

## Table of Contents
- [Introduction](#introduction)
- [Usage](#usage)
- [Examples](#examples)
- [License](#license)
- [Author](#Author)
- [Inputs](#inputs)
- [Outputs](#outputs)

## Introduction
This Terraform module creates an AWS sns along with additional configuration options.
## Usage
To use this module, you should have Terraform installed and configured for AWS. This module provides the necessary Terraform configuration for creating AWS resources, and you can customize the inputs as needed. Below is an example of how to use this module:

# Examples:

## Binary:

```hcl

module "sns" {
  source       = "cypik/sns/aws"
  version      = "1.0.2"
  name         = local.name
  environment  = local.environment
  enable_topic = true

  subscribers  = {
    newrelic   = {
      protocol                        = "https"
      endpoint                        = "https://example.com"
      endpoint_auto_confirms          = false
      raw_message_delivery            = true
      filter_policy                   = ""
      delivery_policy                 = ""
      confirmation_timeout_in_minutes = "60"
    },
    sms = {
      protocol                        = "sms"
      endpoint                        = "+91********64"
      endpoint_auto_confirms          = false
      raw_message_delivery            = false
      filter_policy                   = ""
      delivery_policy                 = ""
      confirmation_timeout_in_minutes = "60"
    },
    email = {
      protocol                        = "email"
      endpoint                        = "example@gmail.com"
      endpoint_auto_confirms          = false
      raw_message_delivery            = false
      filter_policy                   = ""
      delivery_policy                 = ""
      confirmation_timeout_in_minutes = "60"
    }
  }

  data_protection_policy = jsonencode(
    {
      Description = "Deny Inbound Address"
      Name        = "DenyInboundEmailAdressPolicy"
      Statement = [
        {
          "DataDirection" = "Inbound"
          "DataIdentifier" = [
            "arn:aws:dataprotection::aws:data-identifier/EmailAddress",
          ]
          "Operation" = {
            "Deny" = {}
          }
          "Principal" = [
            "*",
          ]
          "Sid" = "DenyInboundEmailAddress"
        },
      ]
      Version = "2021-06-01"
    }
  )
}
```
This example demonstrates how to create various AWS resources using the provided modules. Adjust the input values to suit your specific requirements.

## Examples
For detailed examples on how to use this module, please refer to the [Examples](https://github.com/cypik/terraform-aws-sns/tree/master/examples) directory within this repository.

## License
This Terraform module is provided under the **MIT** License. Please see the [LICENSE](https://github.com/cypik/terraform-aws-sns/blob/master/LICENSE) file for more details.

## Author
Your Name
Replace **MIT** and **Cypik** with the appropriate license and your information. Feel free to expand this README with additional details or usage instructions as needed for your specific use case.

<!-- BEGIN_TF_DOCS -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 1.12.1 |
| <a name="requirement_aws"></a> [aws](#requirement\_aws) | >= 5.99.1 |
| <a name="requirement_tls"></a> [tls](#requirement\_tls) | >= 4.1.0 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_aws"></a> [aws](#provider\_aws) | >= 5.99.1 |

## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_labels"></a> [labels](#module\_labels) | Cypik/labels/aws | 1.0.2 |

## Resources

| Name | Type |
|------|------|
| [aws_sns_platform_application.default](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/sns_platform_application) | resource |
| [aws_sns_sms_preferences.default](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/sns_sms_preferences) | resource |
| [aws_sns_topic.default](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/sns_topic) | resource |
| [aws_sns_topic_data_protection_policy.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/sns_topic_data_protection_policy) | resource |
| [aws_sns_topic_policy.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/sns_topic_policy) | resource |
| [aws_sns_topic_subscription.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/sns_topic_subscription) | resource |
| [aws_caller_identity.current](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/caller_identity) | data source |
| [aws_iam_policy_document.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam_policy_document) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_application_failure_feedback_role_arn"></a> [application\_failure\_feedback\_role\_arn](#input\_application\_failure\_feedback\_role\_arn) | IAM role for failure feedback. | `string` | `""` | no |
| <a name="input_application_success_feedback_role_arn"></a> [application\_success\_feedback\_role\_arn](#input\_application\_success\_feedback\_role\_arn) | The IAM role permitted to receive success feedback for this topic. | `string` | `""` | no |
| <a name="input_application_success_feedback_sample_rate"></a> [application\_success\_feedback\_sample\_rate](#input\_application\_success\_feedback\_sample\_rate) | Percentage of success to sample. | `number` | `100` | no |
| <a name="input_certificate"></a> [certificate](#input\_certificate) | application Platform principal. See Principal for type of principal required for platform. The value of this attribute when stored into the Terraform state is only a hash of the real value, so therefore it is not practical to use this as an attribute for other resources. | `string` | `""` | no |
| <a name="input_content_based_deduplication"></a> [content\_based\_deduplication](#input\_content\_based\_deduplication) | Boolean indicating whether or not to enable content-based deduplication for FIFO topics. | `bool` | `false` | no |
| <a name="input_create_topic_policy"></a> [create\_topic\_policy](#input\_create\_topic\_policy) | Determines whether an SNS topic policy is created | `bool` | `true` | no |
| <a name="input_data_protection_policy"></a> [data\_protection\_policy](#input\_data\_protection\_policy) | A map of data protection policy statements | `string` | `null` | no |
| <a name="input_default_sender_id"></a> [default\_sender\_id](#input\_default\_sender\_id) | A string, such as your business brand, that is displayed as the sender on the receiving device. | `string` | `""` | no |
| <a name="input_default_sms_type"></a> [default\_sms\_type](#input\_default\_sms\_type) | The type of SMS message that you will send by default. Possible values are: Promotional, Transactional. | `string` | `"Transactional"` | no |
| <a name="input_delivery_policy"></a> [delivery\_policy](#input\_delivery\_policy) | The SNS delivery policy. | `string` | `null` | no |
| <a name="input_delivery_status_iam_role_arn"></a> [delivery\_status\_iam\_role\_arn](#input\_delivery\_status\_iam\_role\_arn) | The ARN of the IAM role that allows Amazon SNS to write logs about SMS deliveries in CloudWatch Logs. | `string` | `""` | no |
| <a name="input_delivery_status_success_sampling_rate"></a> [delivery\_status\_success\_sampling\_rate](#input\_delivery\_status\_success\_sampling\_rate) | The percentage of successful SMS deliveries for which Amazon SNS will write logs in CloudWatch Logs. The value must be between 0 and 100. | `number` | `50` | no |
| <a name="input_display_name"></a> [display\_name](#input\_display\_name) | The display name for the SNS topic. | `string` | `""` | no |
| <a name="input_enable_default_topic_policy"></a> [enable\_default\_topic\_policy](#input\_enable\_default\_topic\_policy) | Specifies whether to enable the default topic policy. Defaults to `true` | `bool` | `true` | no |
| <a name="input_enable_sms_preference"></a> [enable\_sms\_preference](#input\_enable\_sms\_preference) | Boolean indicating whether or not to update SNS SMS Preference. | `bool` | `false` | no |
| <a name="input_enable_sns"></a> [enable\_sns](#input\_enable\_sns) | Boolean indicating whether or not to create sns. | `bool` | `false` | no |
| <a name="input_enable_topic"></a> [enable\_topic](#input\_enable\_topic) | Boolean indicating whether or not to create topic. | `bool` | `false` | no |
| <a name="input_enabled"></a> [enabled](#input\_enabled) | Boolean indicating whether or not to create sns module. | `bool` | `true` | no |
| <a name="input_environment"></a> [environment](#input\_environment) | Environment (e.g. `prod`, `dev`, `staging`). | `string` | `""` | no |
| <a name="input_event_delivery_failure_topic_arn"></a> [event\_delivery\_failure\_topic\_arn](#input\_event\_delivery\_failure\_topic\_arn) | SNS Topic triggered when a delivery to any of the platform endpoints associated with your platform application encounters a permanent failure. | `string` | `""` | no |
| <a name="input_event_endpoint_created_topic_arn"></a> [event\_endpoint\_created\_topic\_arn](#input\_event\_endpoint\_created\_topic\_arn) | SNS Topic triggered when a new platform endpoint is added to your platform application. | `string` | `""` | no |
| <a name="input_event_endpoint_deleted_topic_arn"></a> [event\_endpoint\_deleted\_topic\_arn](#input\_event\_endpoint\_deleted\_topic\_arn) | SNS Topic triggered when an existing platform endpoint is deleted from your platform application. | `string` | `""` | no |
| <a name="input_event_endpoint_updated_topic_arn"></a> [event\_endpoint\_updated\_topic\_arn](#input\_event\_endpoint\_updated\_topic\_arn) | SNS Topic triggered when an existing platform endpoint is changed from your platform application. | `string` | `""` | no |
| <a name="input_failure_feedback_role_arn"></a> [failure\_feedback\_role\_arn](#input\_failure\_feedback\_role\_arn) | The IAM role permitted to receive failure feedback for this application. | `string` | `""` | no |
| <a name="input_fifo_topic"></a> [fifo\_topic](#input\_fifo\_topic) | Boolean indicating whether or not to create a FIFO (first-in-first-out) topic | `bool` | `false` | no |
| <a name="input_gcm_key"></a> [gcm\_key](#input\_gcm\_key) | Application Platform credential. See Credential for type of credential required for platform. The value of this attribute when stored into the Terraform state is only a hash of the real value, so therefore it is not practical to use this as an attribute for other resources. | `string` | `""` | no |
| <a name="input_http_failure_feedback_role_arn"></a> [http\_failure\_feedback\_role\_arn](#input\_http\_failure\_feedback\_role\_arn) | IAM role for failure feedback. | `string` | `""` | no |
| <a name="input_http_success_feedback_role_arn"></a> [http\_success\_feedback\_role\_arn](#input\_http\_success\_feedback\_role\_arn) | The IAM role permitted to receive success feedback for this topic. | `string` | `""` | no |
| <a name="input_http_success_feedback_sample_rate"></a> [http\_success\_feedback\_sample\_rate](#input\_http\_success\_feedback\_sample\_rate) | Percentage of success to sample. | `number` | `100` | no |
| <a name="input_key"></a> [key](#input\_key) | Application Platform credential. See Credential for type of credential required for platform. The value of this attribute when stored into the Terraform state is only a hash of the real value, so therefore it is not practical to use this as an attribute for other resources. | `string` | `""` | no |
| <a name="input_kms_master_key_id"></a> [kms\_master\_key\_id](#input\_kms\_master\_key\_id) | The ID of an AWS-managed customer master key (CMK) for Amazon SNS or a custom CMK. For more information. | `string` | `""` | no |
| <a name="input_label_order"></a> [label\_order](#input\_label\_order) | Label order, e.g. `name`,`application`. | `list(any)` | <pre>[<br>  "name",<br>  "environment"<br>]</pre> | no |
| <a name="input_lambda_failure_feedback_role_arn"></a> [lambda\_failure\_feedback\_role\_arn](#input\_lambda\_failure\_feedback\_role\_arn) | IAM role for failure feedback. | `string` | `""` | no |
| <a name="input_lambda_success_feedback_role_arn"></a> [lambda\_success\_feedback\_role\_arn](#input\_lambda\_success\_feedback\_role\_arn) | The IAM role permitted to receive success feedback for this topic. | `string` | `""` | no |
| <a name="input_lambda_success_feedback_sample_rate"></a> [lambda\_success\_feedback\_sample\_rate](#input\_lambda\_success\_feedback\_sample\_rate) | Percentage of success to sample. | `number` | `100` | no |
| <a name="input_managedby"></a> [managedby](#input\_managedby) | ManagedBy, eg 'info@cypik.com'. | `string` | `"info@cypik.com"` | no |
| <a name="input_monthly_spend_limit"></a> [monthly\_spend\_limit](#input\_monthly\_spend\_limit) | The maximum amount in USD that you are willing to spend each month to send SMS messages. | `number` | `1` | no |
| <a name="input_name"></a> [name](#input\_name) | Name  (e.g. `app` or `cluster`). | `string` | `""` | no |
| <a name="input_override_topic_policy_documents"></a> [override\_topic\_policy\_documents](#input\_override\_topic\_policy\_documents) | List of IAM policy documents that are merged together into the exported document. In merging, statements with non-blank `sid`s will override statements with the same `sid` | `list(string)` | `[]` | no |
| <a name="input_platform"></a> [platform](#input\_platform) | The platform that the app is registered with. See Platform for supported platforms like 'APNS' 'GCM'. | `string` | `""` | no |
| <a name="input_policy"></a> [policy](#input\_policy) | The fully-formed AWS policy as JSON. For more information about building AWS IAM policy documents with Terraform. | `string` | `""` | no |
| <a name="input_repository"></a> [repository](#input\_repository) | Terraform current module repo | `string` | `"https://github.com/cypik/terraform-aws-sns"` | no |
| <a name="input_signature_version"></a> [signature\_version](#input\_signature\_version) | If SignatureVersion should be 1 (SHA1) or 2 (SHA256). The signature version corresponds to the hashing algorithm used while creating the signature of the notifications, subscription confirmations, or unsubscribe confirmation messages sent by Amazon SNS. | `number` | `null` | no |
| <a name="input_source_topic_policy_documents"></a> [source\_topic\_policy\_documents](#input\_source\_topic\_policy\_documents) | List of IAM policy documents that are merged together into the exported document. Statements must have unique `sid`s | `list(string)` | `[]` | no |
| <a name="input_sqs_failure_feedback_role_arn"></a> [sqs\_failure\_feedback\_role\_arn](#input\_sqs\_failure\_feedback\_role\_arn) | IAM role for failure feedback. | `string` | `""` | no |
| <a name="input_sqs_success_feedback_role_arn"></a> [sqs\_success\_feedback\_role\_arn](#input\_sqs\_success\_feedback\_role\_arn) | The IAM role permitted to receive success feedback for this topic. | `string` | `""` | no |
| <a name="input_sqs_success_feedback_sample_rate"></a> [sqs\_success\_feedback\_sample\_rate](#input\_sqs\_success\_feedback\_sample\_rate) | Percentage of success to sample. | `number` | `100` | no |
| <a name="input_subscribers"></a> [subscribers](#input\_subscribers) | Required configuration for subscibres to SNS topic. | <pre>map(object({<br>    protocol = string<br>    # The protocol to use. The possible values for this are: sqs, sms, lambda, application. (http or https are partially supported, see below) (email is an option but is unsupported, see below).<br>    endpoint = string<br>    # The endpoint to send data to, the contents will vary with the protocol. (see below for more information)<br>    endpoint_auto_confirms = bool<br>    # Boolean indicating whether the end point is capable of auto confirming subscription e.g., PagerDuty (default is false)<br>    raw_message_delivery = bool<br>    # Boolean indicating whether or not to enable raw message delivery (the original message is directly passed, not wrapped in JSON with the original message in the message property) (default is false)<br>    filter_policy = string<br>    # JSON String with the filter policy that will be used in the subscription to filter messages seen by the target resource.<br>    delivery_policy = string<br>    # The SNS delivery policy<br>    confirmation_timeout_in_minutes = string<br>    # Integer indicating number of minutes to wait in retying mode for fetching subscription arn before marking it as failure. Only applicable for http and https protocols.<br>  }))</pre> | `{}` | no |
| <a name="input_success_feedback_role_arn"></a> [success\_feedback\_role\_arn](#input\_success\_feedback\_role\_arn) | The IAM role permitted to receive success feedback for this application. | `string` | `""` | no |
| <a name="input_success_feedback_sample_rate"></a> [success\_feedback\_sample\_rate](#input\_success\_feedback\_sample\_rate) | The percentage of success to sample (0-100). | `number` | `100` | no |
| <a name="input_topic_policy_statements"></a> [topic\_policy\_statements](#input\_topic\_policy\_statements) | A map of IAM policy [statements](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam_policy_document#statement) for custom permission usage | `any` | `{}` | no |
| <a name="input_tracing_config"></a> [tracing\_config](#input\_tracing\_config) | Tracing mode of an Amazon SNS topic. Valid values: PassThrough, Active. | `string` | `null` | no |
| <a name="input_usage_report_s3_bucket"></a> [usage\_report\_s3\_bucket](#input\_usage\_report\_s3\_bucket) | The name of the Amazon S3 bucket to receive daily SMS usage reports from Amazon SNS. | `string` | `""` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_arn"></a> [arn](#output\_arn) | The ARN of the SNS platform application. |
| <a name="output_id"></a> [id](#output\_id) | The ID of the SNS platform application. |
| <a name="output_topic-arn"></a> [topic-arn](#output\_topic-arn) | The ARN of the SNS topic. |
| <a name="output_topic-id"></a> [topic-id](#output\_topic-id) | The ID of the SNS topic. |
<!-- END_TF_DOCS -->