# Summary
Initial CI (Continuous Integration) pipeline to enable Hack Oregon front end teams to start automating the test and upload of front end assets to AWS.

The architecture approach taken by Hack Oregon in the Winter/Spring 2017 season for front end code is to host that code in AWS S3 buckets (one bucket per Hack Oregon project), and push that code using TravisCI.  TravisCI is hooked to the project team's front end github repo (e.g. https://github.com/hackoregon/housing-frontend), and will automatically run test and build on every commit to master branch.  If all tests pass, assets will be automatically pushed to the associated S3 bucket in AWS.

# Steps
## Prerequisites:
- we've tested this with Ruby 2.3.x and Git 2.7.x-2.10.x
- Aside: nominate a "lead" from the front end developers who will work with their devops contact to setup the initial pipeline

## Sync the frontend starter kit with your team's frontend repo
- Create a new Hack Oregon repo (if one doesn't already exist) to house the project team's front end code e.g. if your team's base repo is named *team-budget*, then the frontend repo should be named *team-budget-frontend*
- make a copy of the "frontend starter" repo and push into the project's repo:
  - clone your new repo (e.g. team-budget-frontend)
  ```
  git clone <new-Repo-URL>
  cd <new-Repo>
  ```
  - add the frontend starter's repo (https://github.com/hackoregon/hackoregon-frontend-starter) as upstream:
  ```
  git remote add upstream https://github.com/hackoregon/hackoregon-frontend-starter
  ```
  - pull from the upstream repo
  ```
  git pull upstream master --allow-unrelated-histories
  ```
  - NOTE: you may run into merge conflicts if the `<newRepo>` contains any files that already exist in the upstream repo.  The usual fun of merging conflicting files will have to be done.  YMMV.
  - NOTE: if you're running git 2.9 or later (e.g. 2.10.1 is found on our Macs as of 2017-02-14), you may see the following output if you skip the --allow-unrelated-histories flag (see this article for details http://stackoverflow.com/questions/37937984/git-refusing-to-merge-unrelated-histories#37938036)
  ```
  git pull upstream master
warning: no common commits
remote: Counting objects: 132, done.
remote: Total 132 (delta 0), reused 0 (delta 0), pack-reused 132
Receiving objects: 100% (132/132), 1017.82 KiB | 297.00 KiB/s, done.
Resolving deltas: 100% (40/40), done.
From https://github.com/hackoregon/hackoregon-frontend-starter
 * branch            master     -> FETCH_HEAD
 * [new branch]      master     -> upstream/master
fatal: refusing to merge unrelated histories
```
  - (give git a good reason why you're merging...hint: any reason will do)
  - Then git add/commit:
  ```
  git add .
  git commit -m "(leave a good commit message for your teammates to read)"
  ```
  - push the current code in your local to the `<new-Repo>`
  ```
  git push origin master
  ```

## Connect TravisCI to your team's frontend repo
- login to TravisCI https://travis-ci.org/
  - (you may need to create an account in TravisCI - which just means logging in with your Github.com credentials - if you don't already have one)
  - Note: first time we added a Hack Oregon repo, we needed to authorize the Hack Oregon organization - this shouldn't be necessary for anyone else
- enable the new repo in TravisCI for your frontend team (e.g. HackOregon/housing-frontend)
  - click the *+* next to My Repositories to add a new repo
  - click on "Hack Oregon" under the Organizations list on the left-hand side
    - NOTE: if this isn't listed, click on the *Review and add* link
    - If you're not currently a member of Hack Oregon, you'll need to get that level of access (i.e. you'll need to be added here: https://github.com/orgs/hackoregon/people) or find someone who has it
  - look for the `<new-Repo>` - if you don't see it listed there, click the *Sync account* button in the top-right corner of the TravisCI page
  - NOTE: you will need *Admin* permission level in the `<new-Repo>` to be able to actually enable the repo in TravisCI - until then, the repo will instead be listed under the heading "You require admin rights to enable these repositories" and you won't be able to toggle in 'on'
- install travis (if you haven't already installed it on your computer):
```
sudo gem install travis
```
- configure the .travis.yml file, including the AWS access key and project's S3 bucket [details to follow]
- encrypt the AWS secret key:
  - run this command:
  ```
  travis encrypt --add deploy.secret_access_key
  ```
  - press [Enter]
  - press [Ctrl]-D
- run the usual *git add* and *git push* commands to enable TravisCI to see a new commit
- check in Travis-CI to see the progress of the first build - if any errors or warnings, give your devops contact a shout (or drop a note to the #dev_ops channel in Slack)
