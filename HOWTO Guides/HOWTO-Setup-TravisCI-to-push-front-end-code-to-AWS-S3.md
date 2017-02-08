#Summary
Initial CI (Continuous Integration) pipeline to enable Hack Oregon front end teams to start automating the test and upload of front end assets to AWS.

The architecture approach taken by Hack Oregon in the Winter/Spring 2017 season for front end code is to host that code in AWS S3 buckets (one bucket per Hack Oregon project), and push that code using TravisCI.  TravisCI is hooked to the project team's front end github repo (e.g. https://github.com/hackoregon/housing-frontend), and will automatically run test and build on every commit to master branch.  If all tests pass, assets will be automatically pushed to the associated S3 bucket in AWS.

#Steps
(This content to be cleaned up and elaborated)
- Nominate a "lead" from the front end developers who will work with their devops contact to setup the initial pipeline
- Create a new Hack Oregon repo (if one doesn't already exist) to house the project team's front end code
- take a copy of the "frontend starter" repo (https://github.com/hackoregon/hackoregon-frontend-starter) and push into the project's repo:
  - clone your new repo (e.g. housing-frontend)
  ```
  git clone <newRepo>
  cd <newRepo>
  ```
  - add the upstream remote
  ```
  git remote add upstream <upstreamRepo>
  ```
  - pull from the upstream repo
  ```
  git pull upstream master  
  ```
  - push back to your new repo
  ```
  git push origin master
  ```
- login to TravisCI https://travis-ci.org/ (create an account if you don't already have one) and authorize the Hack Oregon organization
- enable the new repo in TravisCI for your frontend team (e.g. HackOregon/housing-frontend)
- install travis if you haven't already
```
sudo gem install travis
```
- configure the .travis.yml file, including the AWS access key and project's S3 bucket
- encrypt the AWS secret key
  - run this command
  ```
  travis encrypt --add deploy.secret_access_key
  ```
  - press [Enter]
  - press [Ctrl]-D
- ```
git add .
```
- ```
git push origin master
```
- check in Travis-CI to see the progress of the first build - if any errors or warnings, give your devops contact a shout (or drop a note to the #dev_ops channel in Slack)
