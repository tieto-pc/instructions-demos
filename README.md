# Instructions for All Demonstrations  <!-- omit in toc -->


# Table of Contents  <!-- omit in toc -->
- [Introduction](#introduction)
- [Consistent Tagging of Resources](#consistent-tagging-of-resources)


# Introduction

The demonstrations are divided regarding public cloud providers: aws and azure (possibly later also gcp).

Demonstrations are also divided to short demonstrations and medium size demonstrations. Certain bigger demonstrations are hosted by a dedicated repostory.

You can use the demos as a quick ramp-up for your project's own infrastructure code.

Tieto / Application Services / Public Cloud also provides consultation for our projects how to quickly ramp-up public cloud projects using our demonstrations as a starting point. You can contact us using our corporate email:
- Petri Ohtonen, Head of Public Cloud
- Kari Marttila, Cloud Mentor

You can read also our Cloud Mentor's cloud related blog articles in [Medium Publication](https://medium.com/@kari.marttila).


# Consistent Tagging of Resources

In all demonstrations use consistent tagging of resources.



```terraform
# Environment parameters (e.g. env.tf)
locals {
  my_region = "eu-west-1"
  # Use unique environment names, e.g. dev, custqa, qa, test, perf, ci, prod...
  my_env    = "dev"
  # Use consisten prefix, e.g. <cloud-provider>-<demo-target/purpose>-demo, e.g. aws-ecs-demo  
  my_prefix = "aws-ecs-demo"
}
...

# Environment definition (e.g. env-def.tf)
module "env-def" {
  source     = "../../modules/env-def"
  prefix     = "${local.my_prefix}"
  env        = "${local.my_env}"
  region     = "${local.my_region}"
}

# Module (e.g. ecr.tf):
locals {
  my_name  = "${var.prefix}-${var.env}-ecr-repo"
  my_env   = "${var.prefix}-${var.env}"
}
resource "aws_ecr_repository" "aws-ecs-simple-repository" {
  name = "${local.my_name}"
  tags {
    Name        = "${local.my_name}"
    Environment = "${local.my_env}"
    Prefix      = "${var.prefix}"
    Env         = "${var.env}"
    Region      = "${var.region}"
    Terraform   = "true"
  }
}
```

So, use the following tag names:
- Name: <prefix>-<env>-<name-of-the-mofule>
- Environment: <prefix>-<env>
- Prefix: <prefix>
- Env: <env>
- Region: <region>
- Terraform: "true"

This way we can easily find resources using these tags. E.g:
- Env="dev" => all demonstrations that have been deployed to "dev" environment
- Environment="aws-ecs-simple-demo-dev" => aws-ecs-simple-demo in dev environment.
- Prefix="aws-ecs-simple" => aws-ecs-simple-dev, aws-ecs-simple-qa, aws-ecs-simple-prod...

