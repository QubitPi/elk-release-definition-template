ELK Release Definition Template
===============================

[![Screwdriver CD badge][Screwdriver CD badge]][Screwdriver CD URL]
[![HashiCorp Packer badge][HashiCorp Packer badge]][HashiCorp Packer URL]
[![HashiCorp Terraform badge][HashiCorp Terraform badge]][HashiCorp Terraform URL]
[![Apache License badge]][Apache License URL]
[![GitHub Workflow Status][GitHub Workflow Status badge]][GitHub Workflow Status URL]

A [Screwdriver CD template] that deploys [immutable][Immutable Infrastructure] instances of ELK to AWS. It uses the
[screwdriver-template-main npm package] to assist with template validation, publishing, and tagging. The template tags
the latest versions with the `latest` tag.

> [!TIP]
> [ELK Release Definition Template][elk-release-definition-template] is a satellite project of [hashicorp-aws] and more
> documentation can be found in its [dedicated page for ELK deployment support]
> <img src="https://github.com/QubitPi/QubitPi/blob/master/img/8%E5%A5%BD.gif?raw=true" height="40px"/>

How to Use the Templates
------------------------

> [!TIP]
> Before preceding, please note that it is assumed [the template](./templates/sd-template.yaml) have already been
> installed in Screwdriver. If not, please see documentation on [publishing a template in Screwdriver]

### The Example

[Create a Screwdriver pipeline that uses this template][Screwdriver - create pipeline from template] with the following
contents:

```yaml
---
jobs:
  main:
    requires: [~pr, ~commit]
    template: QubitPi/elk-release-definition-template@latest
    secrets:
      - AWS_ELK_PKRVARS_HCL
      - SSL_CERTIFICATE
      - SSL_CERTIFICATE_KEY
      - AWS_ELK_PKRVARS_HCL
      - AWS_ELK_TFVARS
      - AWS_ACCESS_KEY_ID
      - AWS_SECRET_ACCESS_KEY
```

The following [Screwdriver CD Secrets] needs to be defined before running this template:

- [`AWS_ACCESS_KEY_ID`](https://qubitpi.github.io/hashicorp-aws/docs/setup#aws)
- [`AWS_SECRET_ACCESS_KEY`](https://qubitpi.github.io/hashicorp-aws/docs/setup#aws)
- `SSL_CERTIFICATE` - the content of SSL certificate file serving HTTPS-enabled DNS name of the EC2 hosting our ELK
  instance. This is the same as the `SSL_CERTIFICATE` from the [general SSL setup of hashicorp-aws]
- `SSL_CERTIFICATE_KEY` - the content of SSL certificate key file serving HTTPS-enabled DNS name of the EC2 hosting our
  ELK instance. This is the same as the `SSL_CERTIFICATE_KEY` from the [general SSL setup of hashicorp-aws]
- `NGINX_SSL_CONFIG_FILE` - the content of Nginx config file serving as the reverse proxy for the aforementioned
  SSL/HTTPS support
- `AWS_ELK_PKRVARS_HCL` - The [Packer Variables][HashiCorp Packer Variables] all in one
  [Screwdriver Secret][Screwdriver CD Secrets]

> [!CAUTION]
> We do not need to specify **ssl_cert_file_path**, **ssl_cert_key_file_path**, or **ssl_nginx_config_file_path** in
> this template, which will automatically load `SSL_CERTIFICATE`, `SSL_CERTIFICATE_KEY`, and `NGINX_SSL_CONFIG_FILE` and
> inject their proper locations into the "Packer Variables" list

- `AWS_ELK_TFVARS` - The [Terraform Variables][HashiCorp Terraform Variables] all in one
  [Screwdriver Secret][Screwdriver CD Secrets]

License
-------

The use and distribution terms for [Machine Learning model release definition template] are covered by the
[Apache License, Version 2.0].

<div align="center">
    <a href="https://opensource.org/licenses">
        <img align="center" width="50%" alt="License Illustration" src="https://github.com/QubitPi/QubitPi/blob/master/img/apache-2.png?raw=true">
    </a>
</div>

[Apache License badge]: https://img.shields.io/badge/Apache%202.0-F25910.svg?style=for-the-badge&logo=Apache&logoColor=white
[Apache License URL]: https://www.apache.org/licenses/LICENSE-2.0
[Apache License, Version 2.0]: http://www.apache.org/licenses/LICENSE-2.0.html
[AWS AMI]: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html
[AWS EC2 instance type]: https://aws.amazon.com/ec2/instance-types/
[AWS regions]: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html#Concepts.RegionsAndAvailabilityZones.Availability
[AWS Security Group]: https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html

[dedicated page for ELK deployment support]: https://qubitpi.github.io/hashicorp-aws/docs/elk
[elk-release-definition-template]: https://github.com/QubitPi/elk-release-definition-template

[general SSL setup of hashicorp-aws]: https://qubitpi.github.io/hashicorp-aws/docs/setup#ssl
[GitHub Workflow Status badge]: https://img.shields.io/github/actions/workflow/status/QubitPi/screwdriver-cd-template/ci-cd.yml?branch=master&logo=github&style=for-the-badge
[GitHub Workflow Status URL]: https://github.com/QubitPi/screwdriver-cd-template/actions/workflows/ci-cd.yml

[hashicorp-aws]: https://qubitpi.github.io/hashicorp-aws/
[HashiCorp Packer badge]: https://img.shields.io/badge/Packer-02A8EF?style=for-the-badge&logo=Packer&logoColor=white
[HashiCorp Packer URL]: https://qubitpi.github.io/hashicorp-packer/packer/docs
[HashiCorp Packer Variables]: https://qubitpi.github.io/hashicorp-aws/docs/elk#defining-packer-variables
[HashiCorp Terraform badge]: https://img.shields.io/badge/Terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white
[HashiCorp Terraform URL]: https://qubitpi.github.io/hashicorp-terraform/terraform/docs
[HashiCorp Terraform Variables]: https://qubitpi.github.io/hashicorp-aws/docs/elk#defining-terraform-variables

[Immutable Infrastructure]: https://www.hashicorp.com/resources/what-is-mutable-vs-immutable-infrastructure

[publishing a template in Screwdriver]: https://qubitpi.github.io/screwdriver-cd-guide/user-guide/templates#publishing-a-template

[screwdriver-template-main npm package]: https://github.com/QubitPi/screwdriver-cd-template-main
[Screwdriver - create pipeline from template]: https://qubitpi.github.io/screwdriver-cd-guide/user-guide/templates#using-a-template
[Screwdriver CD Secrets]: https://qubitpi.github.io/screwdriver-cd-guide/user-guide/configuration/secrets
[Screwdriver CD template]: https://qubitpi.github.io/screwdriver-cd-guide/user-guide/templates
[Screwdriver CD URL]: https://qubitpi.github.io/screwdriver-cd-homepage/
[Screwdriver CD badge]: https://img.shields.io/badge/Screwdriver%20CD-1475BB?style=for-the-badge&logo=data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iaXNvLTg4NTktMSI/Pg0KPCEtLSBVcGxvYWRlZCB0bzogU1ZHIFJlcG8sIHd3dy5zdmdyZXBvLmNvbSwgR2VuZXJhdG9yOiBTVkcgUmVwbyBNaXhlciBUb29scyAtLT4NCjxzdmcgaGVpZ2h0PSI4MDBweCIgd2lkdGg9IjgwMHB4IiB2ZXJzaW9uPSIxLjEiIGlkPSJMYXllcl8xIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHhtbG5zOnhsaW5rPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hsaW5rIiANCgkgdmlld0JveD0iMCAwIDUxMiA1MTIiIHhtbDpzcGFjZT0icHJlc2VydmUiPg0KPHBhdGggc3R5bGU9ImZpbGw6I2ZmZmZmZjsiIGQ9Ik01MDQuNzgzLDc3LjA5MWgtMC4wMDZIMzAzLjQ5NGMtMi4wMzYsMC0zLjk3NywwLjg1OS01LjM0NSwyLjM2Ng0KCWMtMS4zNjgsMS41MDgtMi4wMzgsMy41Mi0xLjg0Miw1LjU0OWwzLjkyNSw0MC42OTNjMC4zNDMsMy41NTIsMy4yMjgsNi4zMiw2Ljc5Miw2LjUxNmw0Mi4wNSwyLjMxNGwtODAuNzM2LDExMi4wMjNMMTgwLjI5Myw3MS4xNDINCglsNjMuNTYzLTIuNDNjMy44NDQtMC4xNDYsNi44OTYtMy4yNzYsNi45NDUtNy4xMjFsMC40NjQtMzYuMzkyYzAuMDIyLTEuOTMyLTAuNzI2LTMuNzkyLTIuMDgyLTUuMTY2DQoJYy0xLjM1Ni0xLjM3Mi0zLjIwNi0yLjE0Ny01LjEzNy0yLjE0N0g3LjIyYy0zLjk4OSwwLTcuMjIsMy4yMzEtNy4yMiw3LjIyMXY0MC41NThjMCwzLjk0NywzLjE3LDcuMTYzLDcuMTE1LDcuMjJsNjYuOTE3LDAuOTY0DQoJbDEyNy4yNTcsMjI0LjQ4OGwtMC41NjgsMTQwLjIwNWwtODguNDgsMy42MzFjLTMuODE3LDAuMTU1LTYuODUsMy4yNTktNi45MjMsNy4wNzdsLTAuNzE2LDM3LjUwNg0KCWMtMC4wMzcsMS45MzksMC43MDksMy44MSwyLjA2OSw1LjE5NmMxLjM1NiwxLjM4MywzLjIxMiwyLjE2MSw1LjE1MSwyLjE2MWgyNzYuNzYyYzEuOTgxLDAsMy44NzUtMC44MTMsNS4yMzktMi4yNDkNCgljMS4zNjMtMS40MzUsMi4wNzgtMy4zNjgsMS45NzQtNS4zNDZsLTEuOTMzLTM3LjEzMmMtMC4xOTItMy42OTItMy4xNC02LjY0MS02LjgzNS02LjgzNGwtNzYuMTI0LTMuOTkybC0xMS4wODItMTM2LjU3DQoJTDQzNi40NzksMTM3LjMzbDU1LjY0LTIuNTNjMy4wNjMtMC4xNDIsNS43MDQtMi4yMDIsNi41ODctNS4xMzlsMTIuOTA5LTQzLjAxOGMwLjI0OC0wLjcyOSwwLjM4NS0xLjUxNiwwLjM4NS0yLjMzMg0KCUM1MTIsODAuMzI1LDUwOC43NzIsNzcuMDkxLDUwNC43ODMsNzcuMDkxeiIvPg0KPC9zdmc+
