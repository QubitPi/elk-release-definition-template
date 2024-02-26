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

<div align="center">

<img width="80%" src="https://github.com/QubitPi/hashicorp-aws/blob/master/docs/docs/img/elk-deployment-diagram.png?raw=true" />

</div>

> [!TIP]
> [ELK Release Definition Template][elk-release-definition-template] is a satellite project of [hashicorp-aws] and more
> documentation can be found in its [dedicated page for ELK deployment support]
> <img src="https://github.com/QubitPi/QubitPi/blob/master/img/8%E5%A5%BD.gif?raw=true" height="40px"/>

How to Use This Template
------------------------

> [!NOTE]
> Before preceding, please note that it is assumed [the template](./templates/sd-template.yaml) have already been
> installed in Screwdriver. If not, please see documentation on [publishing a template in Screwdriver]

[Create a Screwdriver pipeline that uses this template][Screwdriver - create pipeline from template] with the 
`screwdriver.yaml` file of

```yaml
---
jobs:
  main:
    requires: [~pr, ~commit]
    template: QubitPi/elk-release-definition-template@latest
```

The following [Screwdriver CD Secrets] needs to be defined before running the pipeline:

- [`AWS_ACCESS_KEY_ID`](https://qubitpi.github.io/hashicorp-aws/docs/setup#aws)
- [`AWS_SECRET_ACCESS_KEY`](https://qubitpi.github.io/hashicorp-aws/docs/setup#aws)
- `SSL_CERTIFICATE` - the content of SSL certificate file serving HTTPS-enabled DNS name of the EC2 hosting our ELK
  instance. This is the same as the `SSL_CERTIFICATE` from the [general SSL setup of hashicorp-aws]
- `SSL_CERTIFICATE_KEY` - the content of SSL certificate key file serving HTTPS-enabled DNS name of the EC2 hosting our
  ELK instance. This is the same as the `SSL_CERTIFICATE_KEY` from the [general SSL setup of hashicorp-aws]

To run the pipeline, fill in the AWS-related **parameters** first

<div align="center">

<img width="30%" src="https://github.com/QubitPi/QubitPi/blob/master/img/elk-release-definition-template-parameters.png?raw=true" />

</div>

Then hit "**Submit**" to start deploying.

> [!IMPORTANT]
> This template is the Screwdriver CD implementation of the
> [general ELK deployment by hashicorp-aws framework][dedicated page for ELK deployment support]. Once the pipeline
> deploys ELK, we must remember to do the
> [post setup in EC2 instance](https://qubitpi.github.io/hashicorp-aws/docs/elk#post-setup-in-ec2-instance).
> 
> The password for user 'elastic' can be found _packer-build_ step logs. Here is an example:
> 
> ![elk deploy password](https://github.com/QubitPi/QubitPi/blob/master/img/elastic-password-from-log.png?raw=true)

Troubleshooting
---------------

**Q**: I updated template (such as a template parameter name) but it was not taking effect in the pipeline that uses 
this template.

**A**: This is because the pipeline is still referencing the old template definition. Note that when template is 
republished, it doesn't automatically refresh the pipeline that uses this template. There are 2 approaches for solving
this problem:

1. Simply delete and recreate the pipeline to pick up the updated template definition
2. Re-sync pipeline by navigating to **Options** tab of the pipeline UI and then click sync button next to the 
   **Pipeline** in **Sync** section (shown below). _Lastly_, refresh the pipeline **Builds** page.

  <div align="center">

  <img width="70%" src="https://github.com/QubitPi/QubitPi/blob/master/img/resync-pipeline.png?raw=true" />

  </div>

License
-------

The use and distribution terms for [elk-release-definition-template] are covered by the [Apache License, Version 2.0].

<div align="center">
    <a href="https://opensource.org/licenses">
        <img align="center" width="50%" alt="License Illustration" src="https://github.com/QubitPi/QubitPi/blob/master/img/apache-2.png?raw=true">
    </a>
</div>

[Apache License badge]: https://img.shields.io/badge/Apache%202.0-F25910.svg?style=for-the-badge&logo=Apache&logoColor=white
[Apache License URL]: https://www.apache.org/licenses/LICENSE-2.0
[Apache License, Version 2.0]: http://www.apache.org/licenses/LICENSE-2.0.html

[dedicated page for ELK deployment support]: https://qubitpi.github.io/hashicorp-aws/docs/elk
[elk-release-definition-template]: https://github.com/QubitPi/elk-release-definition-template

[general SSL setup of hashicorp-aws]: https://qubitpi.github.io/hashicorp-aws/docs/setup#ssl
[GitHub Workflow Status badge]: https://img.shields.io/github/actions/workflow/status/QubitPi/elk-release-definition-template/ci-cd.yml?branch=master&logo=github&style=for-the-badge
[GitHub Workflow Status URL]: https://github.com/QubitPi/elk-release-definition-template/actions/workflows/ci-cd.yml

[hashicorp-aws]: https://qubitpi.github.io/hashicorp-aws/
[HashiCorp Packer badge]: https://img.shields.io/badge/Packer-02A8EF?style=for-the-badge&logo=Packer&logoColor=white
[HashiCorp Packer URL]: https://qubitpi.github.io/hashicorp-packer/packer/docs
[HashiCorp Terraform badge]: https://img.shields.io/badge/Terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white
[HashiCorp Terraform URL]: https://qubitpi.github.io/hashicorp-terraform/terraform/docs

[Immutable Infrastructure]: https://www.hashicorp.com/resources/what-is-mutable-vs-immutable-infrastructure

[publishing a template in Screwdriver]: https://qubitpi.github.io/screwdriver-cd-guide/user-guide/templates#publishing-a-template

[screwdriver-template-main npm package]: https://github.com/QubitPi/screwdriver-cd-template-main
[Screwdriver - create pipeline from template]: https://qubitpi.github.io/screwdriver-cd-guide/user-guide/templates#using-a-template
[Screwdriver CD Secrets]: https://qubitpi.github.io/screwdriver-cd-guide/user-guide/configuration/secrets
[Screwdriver CD template]: https://qubitpi.github.io/screwdriver-cd-guide/user-guide/templates
[Screwdriver CD URL]: https://qubitpi.github.io/screwdriver-cd-homepage/
[Screwdriver CD badge]: https://img.shields.io/badge/Screwdriver%20CD-1475BB?style=for-the-badge&logo=data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iaXNvLTg4NTktMSI/Pg0KPCEtLSBVcGxvYWRlZCB0bzogU1ZHIFJlcG8sIHd3dy5zdmdyZXBvLmNvbSwgR2VuZXJhdG9yOiBTVkcgUmVwbyBNaXhlciBUb29scyAtLT4NCjxzdmcgaGVpZ2h0PSI4MDBweCIgd2lkdGg9IjgwMHB4IiB2ZXJzaW9uPSIxLjEiIGlkPSJMYXllcl8xIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHhtbG5zOnhsaW5rPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hsaW5rIiANCgkgdmlld0JveD0iMCAwIDUxMiA1MTIiIHhtbDpzcGFjZT0icHJlc2VydmUiPg0KPHBhdGggc3R5bGU9ImZpbGw6I2ZmZmZmZjsiIGQ9Ik01MDQuNzgzLDc3LjA5MWgtMC4wMDZIMzAzLjQ5NGMtMi4wMzYsMC0zLjk3NywwLjg1OS01LjM0NSwyLjM2Ng0KCWMtMS4zNjgsMS41MDgtMi4wMzgsMy41Mi0xLjg0Miw1LjU0OWwzLjkyNSw0MC42OTNjMC4zNDMsMy41NTIsMy4yMjgsNi4zMiw2Ljc5Miw2LjUxNmw0Mi4wNSwyLjMxNGwtODAuNzM2LDExMi4wMjNMMTgwLjI5Myw3MS4xNDINCglsNjMuNTYzLTIuNDNjMy44NDQtMC4xNDYsNi44OTYtMy4yNzYsNi45NDUtNy4xMjFsMC40NjQtMzYuMzkyYzAuMDIyLTEuOTMyLTAuNzI2LTMuNzkyLTIuMDgyLTUuMTY2DQoJYy0xLjM1Ni0xLjM3Mi0zLjIwNi0yLjE0Ny01LjEzNy0yLjE0N0g3LjIyYy0zLjk4OSwwLTcuMjIsMy4yMzEtNy4yMiw3LjIyMXY0MC41NThjMCwzLjk0NywzLjE3LDcuMTYzLDcuMTE1LDcuMjJsNjYuOTE3LDAuOTY0DQoJbDEyNy4yNTcsMjI0LjQ4OGwtMC41NjgsMTQwLjIwNWwtODguNDgsMy42MzFjLTMuODE3LDAuMTU1LTYuODUsMy4yNTktNi45MjMsNy4wNzdsLTAuNzE2LDM3LjUwNg0KCWMtMC4wMzcsMS45MzksMC43MDksMy44MSwyLjA2OSw1LjE5NmMxLjM1NiwxLjM4MywzLjIxMiwyLjE2MSw1LjE1MSwyLjE2MWgyNzYuNzYyYzEuOTgxLDAsMy44NzUtMC44MTMsNS4yMzktMi4yNDkNCgljMS4zNjMtMS40MzUsMi4wNzgtMy4zNjgsMS45NzQtNS4zNDZsLTEuOTMzLTM3LjEzMmMtMC4xOTItMy42OTItMy4xNC02LjY0MS02LjgzNS02LjgzNGwtNzYuMTI0LTMuOTkybC0xMS4wODItMTM2LjU3DQoJTDQzNi40NzksMTM3LjMzbDU1LjY0LTIuNTNjMy4wNjMtMC4xNDIsNS43MDQtMi4yMDIsNi41ODctNS4xMzlsMTIuOTA5LTQzLjAxOGMwLjI0OC0wLjcyOSwwLjM4NS0xLjUxNiwwLjM4NS0yLjMzMg0KCUM1MTIsODAuMzI1LDUwOC43NzIsNzcuMDkxLDUwNC43ODMsNzcuMDkxeiIvPg0KPC9zdmc+
