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
  source = "cypik/sns/aws"

  name         = local.name
  environment  = local.environment
  enable_topic = true

  subscribers = {
    newrelic = {
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
