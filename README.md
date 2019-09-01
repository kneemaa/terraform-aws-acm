# AWS Certificate Manager (ACM) Terraform module

Terraform module which creates ACM certificates and validates them using Route53 DNS (recommended) or e-mail.

## Usage with Route53 DNS validation (recommended)

```hcl
module "acm" {
  source = "terraform-aws-modules/acm/aws"

  domain_name  = "my-domain.com"
  zone_id      = "Z2ES7B9AZ6SHAE"

  subject_alternative_names = [
    "*.my-domain.com",
  ]

  tags = {
    Name = "my-domain.com"
  }
}
```

## Examples

* [Complete example with DNS validation (recommended)](https://github.com/terraform-aws-modules/terraform-aws-acm/tree/master/examples/complete-dns-validation)
* [Complete example with EMAIL validation](https://github.com/terraform-aws-modules/terraform-aws-acm/tree/master/examples/complete-email-validation)

## Conditional creation and validation

Sometimes you need to have a way to create ACM certificate conditionally but Terraform does not allow to use `count` inside `module` block, so the solution is to specify argument `create_certificate`.

```hcl
module "acm" {
  source = "terraform-aws-modules/acm/aws"

  create_certificate = false
  # ... omitted
}
```

Similarly, to disable DNS validation of ACM certificate:

```hcl
module "acm" {
  source = "terraform-aws-modules/acm/aws"

  validate_certificate = false
  # ... omitted
}
```

## Notes

* For use in an automated pipeline consider setting the `wait_for_validation = false` to avoid waiting for validation to complete or error after a 45 minute timeout.

<!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| certificate_transparency_logging | Enables certificate transparancy log. Options are ENABLED or DISABLED | string | `"DISABLED"` | no |
| create\_certificate | Whether to create ACM certificate | string | `"true"` | no |
| domain\_name | A domain name for which the certificate should be issued | string | `""` | no |
| subject\_alternative\_names | A list of domains that should be SANs in the issued certificate | list | `[]` | no |
| tags | A mapping of tags to assign to the resource | map | `{}` | no |
| validate\_certificate | Whether to validate certificate by creating Route53 record | string | `"true"` | no |
| validation\_allow\_overwrite\_records | Whether to allow overwrite of Route53 records | string | `"true"` | no |
| validation\_method | Which method to use for validation. DNS or EMAIL are valid, NONE can be used for certificates that were imported into ACM and then into Terraform. | string | `"DNS"` | no |
| wait\_for\_validation | Whether to wait for the validation to complete | string | `"true"` | no |
| zone\_id | The ID of the hosted zone to contain this record. | string | `""` | no |

## Outputs

| Name | Description |
|------|-------------|
| this\_acm\_certificate\_arn | The ARN of the certificate |
| this\_acm\_certificate\_domain\_validation\_options | A list of attributes to feed into other resources to complete certificate validation. Can have more than one element, e.g. if SANs are defined. Only set if DNS-validation was used. |
| this\_acm\_certificate\_validation\_emails | A list of addresses that received a validation E-Mail. Only set if EMAIL-validation was used. |

<!-- END OF PRE-COMMIT-TERRAFORM DOCS HOOK -->

## Authors

Module managed by [Anton Babenko](https://github.com/antonbabenko).

## License

Apache 2 Licensed. See LICENSE for full details.
