#Summary
Setting up correct permissions in AWS S3 to be able to push build artifacts from TravisCI to an S3 bucket.

# Error message if permissions insufficient
- TravisCI will mark that build "errorored"
- in the Job log, near the end you should see a message like this:
'''
Oops, It looks like you tried to write to a bucket that isn't yours or doesn't exist yet. Please create the bucket before trying to write to it.
failed to deploy
'''

# Steps to address this error
(This needs to be elaborated)
- create S3 bucket
- create IAM user
- generate access keys for the user, save them in safe place
- add ACCESS KEY as access_key_id to .travis.yml and save the file
- run this command from command line:
  - travis encrypt --add deploy.secret_access_key
  - paste in the SECRET ACCESS KEY for your user, hit your Enter key, then type [Ctrl]-D
- *git add* and *git push* the code to your repo
- Now TravisCI will be able to use these credentials to try to access S3
- (configure TravisCI to enable that project to automatically build all commits to your repo)
- Permissions, "add bucket policy" ("edit bucket policy")
  - Principal: includes the ARN for the IAM user you've designated
  - Action: "s3:PutObject"
  - Resource: "arn:aws:s3:::NAME_OF_BUCKET/\*"
- make a change to a file in your repo, commit it, and git push that change
- Travis should initiate a build and at the end should attempt to upload the files to your bucket
