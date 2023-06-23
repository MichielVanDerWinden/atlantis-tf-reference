# Terraform Atlantis Reference Architecture for AWS

> **Warning**:
> This code is provided as-is — it is not meant to be used verbatim. No support is provided in any way.

## Introduction

This is an example setup of [Atlantis for terraform](https://www.runatlantis.io/), specifically set up to easily work with AWS. If you're looking for a best-practices terraform setup for AWS, check out [this accompanying repo](https://github.com/MichielVanDerWinden/aws-tf-reference)

## Getting started

### AWS Setup

To re-use this setup, the following roles are required in the AWS accounts that are either the host or the target of the Atlantis service:

- **Atlantis host account**: arn:aws:iam::xxxxxxxxxxxx:role/eks-role-ci
- **Deployment target accounts**: arn:aws:iam::xxxxxxxxxxxx:role/eks-role-ci-terraform

Policies should be applied in such a way that the host role `eks-role-ci` can assume the `eks-role-ci-terraform` role in all target accounts.
To prevent issues with timeouts during large `terraform apply` actions, it might be best to change the session timeout to something higher than the default 1 hour.

### Atlantis installation

Install Atlantis following their [installation guide](https://www.runatlantis.io/docs/installation-guide.html). Multiple options are provided, e.g. running it as a pod on your k8s cluster or as a docker container on a simple VPS.

### Atlantis setup

To use Atlantis to the fullest, we'll have to set up the correct workflows, projects and their powerful autoplan feature. This repo contains an example setup based on the values file that accompanies Atlantis' Helm Chart. All information can be re-used in different setups, but the syntax will most definitely not be the same. 

The example uses a Github App. This would be the most secure way of linking your Atlantis service to your Github projects. Any secrets will have to be added through a vault (e.g. [Hashicorp Vault](https://www.vaultproject.io/)) or an external secrets manager.

> **Note**: Make sure you've set up the correct profiles in your server side setup (check [helm-atlantis-values.yaml](./helm-atlantis-values.yaml)) as those are a prerequisite for the provided Atlantis workflow.