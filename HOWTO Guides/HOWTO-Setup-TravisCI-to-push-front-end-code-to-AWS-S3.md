# Summary
Initial CI (Continuous Integration) pipeline to enable Hack Oregon front end teams to start automating the test and upload of front end assets to AWS.

The architecture approach taken by Hack Oregon in the Winter/Spring 2017 season for front end code is to host that code in AWS S3 buckets (one bucket per Hack Oregon project), and push that code using TravisCI.  TravisCI is hooked to the project team's frontend github repo (e.g. https://github.com/hackoregon/team-budget-frontend), and will automatically run test and build on every commit to master branch.  If all tests pass, assets will be automatically pushed to the associated S3 bucket in AWS.

# Steps
## Prerequisites:
- we've tested this with Ruby 2.3.x and Git 2.7.x-2.10.x
- Aside: nominate a "lead" from the front end developers who will work with their devops contact to setup the initial pipeline

## Sync the frontend starter kit with your team's frontend repo
- Create a new Hack Oregon repo (if one doesn't already exist) to house the project team's front end code e.g. if your team's base repo is named *team-budget*, then the frontend repo should be named *team-budget-frontend*
- make a copy of the "frontend starter" repo and push into the project's repo:
  - clone your new repo (e.g. `team-budget-frontend` as `<new-Repo>` and https://github.com/hackoregon/team-budget-frontend as `<new-Repo-URL>`):
  ```
  git clone <new-Repo-URL>
  cd <new-Repo>
  ```
  - add the frontend starter's repo as upstream:
  ```
  git remote add upstream https://github.com/hackoregon/hackoregon-frontend-starter
  ```
  - pull from the upstream repo:
  ```
  git pull upstream master --allow-unrelated-histories
  ```
  - NOTE: you may run into merge conflicts if the `<newRepo>` contains any files that already exist in the upstream repo.  You'll have to complete the usual fun of merging conflicting files.  Good luck, and yell for help from your team if you run into trouble.  Git's a great tool that allows you to tie yourself in all sorts of knots.
  - NOTE: if you're running git 2.9 or later (e.g. 2.10.1 is found on our Macs as of 2017-02-14), you may see the following output if you skip the `--allow-unrelated-histories` parameter (see [this article](http://stackoverflow.com/questions/37937984/git-refusing-to-merge-unrelated-histories#37938036) for details):
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
  - (give git a good reason why you're merging...hint: any reason will do, but your team may not like you if you're not making it clear why you made the change)
  - Then git add/commit:
  ```
  git add .
  git commit -m "(leave a good commit message for your teammates to read)"
  ```
  - push the current code in your local to the `<new-Repo>`:
  ```
  git push origin master
  ```

## Connect TravisCI to your team's frontend repo
- login to TravisCI at https://travis-ci.org/
  - (you may need to create an account in TravisCI - which just means logging in with your Github.com credentials - if you don't already have one)
  - Note: first time we added a Hack Oregon repo, we needed to authorize the Hack Oregon organization - this shouldn't be necessary for anyone else
- enable the new repo in TravisCI for your frontend team (e.g. HackOregon/housing-frontend)
  - click the *+* next to My Repositories to add a new repo
  - click on "Hack Oregon" under the Organizations list on the left-hand side
    - NOTE: if this isn't listed, click on the *Review and add* link
    - If you're not currently a member of Hack Oregon, you'll need to get that level of access (i.e. you'll need to be added [here](https://github.com/orgs/hackoregon/people) or find someone who has it
  - look for the `<new-Repo>` - if you don't see it listed there, click the *Sync account* button in the top-right corner of the TravisCI page
  - NOTE: you will need *Admin* permission level in the `<new-Repo>` to be able to actually enable the repo in TravisCI - until then, the repo will instead be listed under the heading "You require admin rights to enable these repositories" and you won't be able to toggle in 'on'
- configure the .travis.yml file, including the project's S3 `bucket` and `AWS_ACCESS_KEY_ID` (see [this page](https://github.com/hackoregon/hacku-devops-2017/wiki/Assignment-2---Travis-CI-Introduction#travisfile)
  - use the S3 `bucket` that was assigned to your project, which will be the repository for your project's frontend code
  - use an `AWS_ACCESS_KEY_ID` that has write access privileges on the S3 bucket you're using. Your travis file should look something like this:
  ```yaml
  language: node_js
  node_js:
  - '6.0'
  install: npm install
  deploy:
    provider: s3
    access_key_id: "${AWS_ACCESS_KEY}"
    bucket: "${AWS_SECRET_KEY}"
    region: us-west-2
  ```
- In the travis-ci.org config for your repo, add the following environment variables with your AWS credentials making sure that your keys are not exposed:

```
AWS_ACCESS_KEY
AWS_SECRET_KEY
```
- run the usual *git add* and *git push* commands to enable TravisCI to see a new commit
- check in Travis-CI to see the progress of the first build - if any errors or warnings, give your devops contact a shout (or drop a note to the `dev_ops` channel in Slack)
- NOTE: if you see errors related to an inability of Travis to upload the files to the S3 bucket, the bucket policy may be blocking access.  The bucket policy can be adjusted by those with admin-level access to each S3 bucket - check with your devops contact for assistance.  In the Travis build log you the error may look something like:
```
Oops, It looks like you tried to write to a bucket that isn't yours or doesn't exist yet. Please create the bucket before trying to write to it.
failed to deploy
```

## Viewing TravisCI build results: other members of the team
- the person who initially setup the TravisCI build from your team's repo is able to watch every build already
- however, the rest of the team members may wish to see what happens during the build (in case errors occur that they want to fix)
- for those additional team members who are contributors to the team's GitHub repo, they'll have to "activate"  the repo in their own TravisCI account:
  - look for the My Repositories area of your [Travis account](https://travis-ci.org), and click the *+* link
  - click the *Sync acccount* button to get the repo to show up in your account's listings
  - browse back to the [main Travis page](https://travis-ci.org)
  - You should see the project listed there now, with the results of the last build displayed by default
