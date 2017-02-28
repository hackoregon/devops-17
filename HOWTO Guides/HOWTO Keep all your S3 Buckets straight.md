# Summary

Hack Oregon's winter 2017 project season is advancing the use of AWS for managing configuration, deployment and maintenance of project systems.

One of those advances is increased reliance on AWS S3 for storage of various project assets.

# Different buckets, different purposes
- Each Hack Oregon project has at least three S3 buckets assigned to it:
  - Config
  - DB Dump
  - Frontend Assets

#  Naming Convention for Hack Oregon project S3 Buckets
## Config
- Each Hack Oregon project has one S3 bucket for all configuration secrets
- The bucket name will follow the form `hacko_PROJECT_config`
- The buckets to be used in the 2017 winter season are:
  - hacko_budget_config
  - hacko_emergency_response_config
  - hacko_homelessness_config
  - hacko_housing_config
  - hacko_transportation_config

## DB Dump
- Each Hack Oregon project is assigned one S3 bucket to store database backups
- This is only intended to for dumps from the cloud-based database servers
  - Database servers on local developer machines should be reconstructed from build artifacts in the team's project repo, or downloaded as a container image from the team's container registry
- The buckets to be used in the 2017 winter season are:
  - hacko_budget_???
  - hacko_emergency_response_???
  - hacko_homelessness_???
  - hacko_housing_???
  - hacko_transportation_???

## Frontend Assets
- Each Hack Oregon project is assigned one S3 bucket to store the deployed assets for their frontend web site
- this can contain any or all of HTML, CSS, JS, image files, npm modules (built from the collection of project assets)
- The buckets to be used in the 2017 winter season at the moment are:
  - hacko_budget_staging
  - hacko_emergency_response_staging
  - hacko_homelessness_staging
  - hacko_housing_staging
  - hacko_transportation_staging
- You should expect to see us stand up another set of buckets to manage production-destined frontend assets in the March-April 2017 timeframe
