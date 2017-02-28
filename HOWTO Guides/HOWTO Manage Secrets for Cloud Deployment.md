
# Purpose
This secrets-protection scheme is intended to enable cloud-based build, test and deploy of the Django app for each Hack Oregon project, without committing the secrets to a publicly-readable repository like GitHub.

This infrastructure is separate from the infrastructure used to deploy frontend code to the projects' S3 buckets for frontend. (See [this article](https://github.com/hackoregon/devops-17/blob/master/HOWTO%20Guides/HOWTO%20Keep%20all%20your%20S3%20Buckets%20straight.md) for more detail on the purpose of the various S3 buckets assigned to each project team.)

# Naming Scheme for 'config' Bucket
- Each Hack Oregon Project is assigned one S3 bucket to store all secrets for the project's backend application configuration
- The bucket name will follow the form `hacko_PROJECT_config`
- The buckets to be used in the 2017 winter season are:
  - hacko_budget_config
  - hacko_emergency_response_config
  - hacko_homelessness_config
  - hacko_housing_config
  - hacko_transportation_config

# What your S3 'config' Bucket should contain
- There should be at least two folders in that bucket, corresponding to the environments to which your Django app will deploy:
  - INTEGRATION
  - PRODUCTION
- In each folder there should be two files:
  - ???
  - ???

# How to use these files
These files support the deployment pattern documented [here]().  Ensure that the above files exist, and they have been properly populated with all required secrets.

# Why not just use Travis environment variables?
- An alternative approach to managing all these secrets in Python scripts would be to manually enter them in each project's Travis repo as environment variables
- The security risks are similar:
- - If an attacker discovered the AWS credentials for the project's S3 bucket, they can read the secrets (database password, Django secret)
- - If an attacker discovered the Travis credentials to access the project's Travis repo, they can't read the secrets but they can deploy whatever code they like to the project's container
- - (??? Is this true? )
