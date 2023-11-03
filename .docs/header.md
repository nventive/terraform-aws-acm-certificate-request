![nventive](https://nventive-public-assets.s3.amazonaws.com/nventive_logo_github.svg?v=2)

# terraform-aws-acm-certificate-request

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg?style=flat-square)](LICENSE) [![Latest Release](https://img.shields.io/github/release/nventive/terraform-aws-acm-certificate-request.svg?style=flat-square)](https://github.com/nventive/terraform-aws-acm-certificate-request/releases/latest)

Terraform module to request an ACM certificate.

---

## Source

This is simply a copy of `cloudposse/acm-request-certificate/aws` tag `v0.17.0`, with seperated providers for Route53 and ACM.

## Providers

This modules uses two instances of the AWS provider. One for Route 53 resources and one for the rest. The reason why is
that Route 53 is often in a different account (ie. in the prod account when creating resources for dev).

You must provide both providers, whether you use Route 53 or not. In any case, you can specify the same provider for
both if need be.

## Examples

**IMPORTANT:** We do not pin modules to versions in our examples because of the difficulty of keeping the versions in
the documentation in sync with the latest released versions. We highly recommend that in your code you pin the version
to the exact version you are using so that your infrastructure remains stable, and update versions in a systematic way
so that they do not catch you by surprise.

```hcl
module "cert" {
  source = "nventive/acm-certificate-request/aws"
  # We recommend pinning every module to a specific version
  # version = "x.x.x"

  providers = {
    aws.acm     = aws
    aws.route53 = aws.route53
  }

  domain_name                       = "example.com"
  process_domain_validation_options = true
  ttl                               = "300"
  subject_alternative_names         = ["*.example.com"]
}
```

Should you want to use the same AWS provider for both Route 53 and the default one.

```hcl
module "cert" {
  source = "nventive/acm-certificate-request/aws"
  # We recommend pinning every module to a specific version
  # version = "x.x.x"

  providers = {
    aws.acm     = aws
    aws.route53 = aws
  }

  # ...
}
```
