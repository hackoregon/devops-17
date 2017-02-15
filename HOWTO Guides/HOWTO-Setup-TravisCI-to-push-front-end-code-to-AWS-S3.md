#Summary
Initial CI (Continuous Integration) pipeline to enable Hack Oregon front end teams to start automating the test and upload of front end assets to AWS.

The architecture approach taken by Hack Oregon in the Winter/Spring 2017 season for front end code is to host that code in AWS S3 buckets (one bucket per Hack Oregon project), and push that code using TravisCI.  TravisCI is hooked to the project team's front end github repo (e.g. https://github.com/hackoregon/housing-frontend), and will automatically run test and build on every commit to master branch.  If all tests pass, assets will be automatically pushed to the associated S3 bucket in AWS.

#Steps
- Prerequisites: we've tested this with Ruby 2.3.x and Git 2.7.x-2.10.x

- Aside: nominate a "lead" from the front end developers who will work with their devops contact to setup the initial pipeline
- Create a new Hack Oregon repo (if one doesn't already exist) to house the project team's front end code e.g. if your team's base repo is named *team-budget*, then the frontend repo should be named *team-budget-frontend*
- make a copy of the "frontend starter" repo and push into the project's repo:
  - clone your new repo (e.g. team-budget-frontend)
  ```
  git clone <new-Repo-URL>
  cd <new-Repo>
  ```
  - add the frontend starter's repo (https://github.com/hackoregon/hackoregon-frontend-starter) as upstream:
  ```
  git remote add upstream <upstreamRepo>
  ```
  - pull from the upstream repo
  ```
  git pull upstream master  
  ```
  - push the current code in your local to the `<newRepo>`
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
- run the usual *git add* and *git push* commands to enable TravisCI to see a new commit
- check in Travis-CI to see the progress of the first build - if any errors or warnings, give your devops contact a shout (or drop a note to the #dev_ops channel in Slack)
