<!-- BEGIN_TF_DOCS -->
## Terraform-AWS-route53-record-set
Terraform AWS Route53 Record Module
This Terraform code provides the ability to create Route 53 records within a given hosted zone in AWS.

## Resources
aws_route53_record.app
This resource creates DNS records in AWS Route 53 based on the subdomain_records input variable. The for_each construct allows for creating multiple DNS records as defined by the map of subdomain_records.

## Attributes:
zone_id: The ID of the hosted zone where the record should be created.
name: The name of the record.
type: The record type (A, CNAME, etc.).
ttl: The Time To Live (TTL) of the record.
records: A list of record values (e.g., IP addresses or domain names).
module "subdomain-records"
This module manages the creation of multiple subdomain records. The actual implementation of the module is abstracted (presumably located in a different directory or source).
subdomain: Defines the primary subdomain name.
subdomain_records: A map defining each subdomain's name, type, TTL, and record values.
zone_id: The ID of the hosted zone.
depends_on: Ensures that the specified resource (in this case, aws_route53_zone.appsubdomain) is created before this module is executed.

## Example Usage
The provided code includes an example of how to use the subdomain-records module:

```
## This will create Private Record Set

module "private_hosted_zone_subdomain-records" {
  source    = "../../"
  subdomain = "dev.012345.private.jlldrs.net"
  create_private_hosted_zone_record_set = true
  subdomain_records = {
    "test000.dev.012345.private.jlldrs.net" = {
      type    = "A"
      ttl     = "30"
      records = ["5.4.3.2"]
    },
    "aliastest111.dev.012345.private.jlldrs.net" = {
      type    = "CNAME"
      ttl     = "30"
      records = ["test000.dev.012345.private.jlldrs.net"]
    }
  }
}

## This will create Public Record Set

module "public_hosted_zone_subdomain-records" {
  source    = "../../"
  subdomain = "dev6.a00003261.public.jlldrs.net"
  subdomain_records = {
    "test333.dev6.a00003261.public.jlldrs.net" = {
      type    = "A"
      ttl     = "30"
      records = ["5.4.3.2"]
    },
    "aliastest444.dev6.a00003261.public.jlldrs.net" = {
      type    = "CNAME"
      ttl     = "30"
      records = ["test333.dev6.a00003261.public.jlldrs.net"]
    }
  }
}

```


## Dependencies
An AWS account and configured AWS CLI or AWS credentials.
Terraform installed.
Existing Route 53 hosted zone (data.aws_route53_zone.appsubdomain refers to a data source that fetches information about a pre-existing hosted zone).

## Notes
The commented out depends_on within the aws_route53_record.app resource suggests there may have been a circular dependency or some order of creation issues in past iterations. Review and confirm whether it's necessary based on the overall infrastructure setup.
The actual module code for subdomain-records is abstracted in the given code, so ensure that the module's source path is correctly defined and available when using this code in practice.

## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 0.15 |
| <a name="requirement_aws"></a> [aws](#requirement\_aws) | ~> 4.0 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_aws"></a> [aws](#provider\_aws) | ~> 4.0 |

## Modules

No modules.

## Resources

| Name | Type |
|------|------|
| [aws_route53_record.app](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/route53_record) | resource |
| [aws_route53_zone.appsubdomain](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/route53_zone) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_create_private_hosted_zone_record_set"></a> [create\_private\_hosted\_zone\_record\_set](#input\_create\_private\_hosted\_zone\_record\_set) | Whether Route53 zone is private | `bool` | `false` | no |
| <a name="input_subdomain"></a> [subdomain](#input\_subdomain) | Name of <env>-<appid>-private.jlldrs.net | `string` | n/a | yes |
| <a name="input_subdomain_records"></a> [subdomain\_records](#input\_subdomain\_records) | Map of DNS records to add to zone | `map(any)` | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_route53_record_fqdn"></a> [route53\_record\_fqdn](#output\_route53\_record\_fqdn) | FQDN built using the zone domain and name |
| <a name="output_route53_record_name"></a> [route53\_record\_name](#output\_route53\_record\_name) | The name of the record |
<!-- END_TF_DOCS -->
