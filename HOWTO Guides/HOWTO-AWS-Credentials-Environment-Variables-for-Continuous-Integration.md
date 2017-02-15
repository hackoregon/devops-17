# Summary
When setting up an automated Continuous Integration pipeline, scripts that access AWS from a remote server (e.g. TravisCI's build servers) will need AWS credentials to act on your (our) behalf.  The recommended approach is to configure TravisCI to use environment variables and store the AWS credentials in that variable configuration in TravisCI.  This enables us to use the same script both locally (for initial sanity testing) and remotely (for automation), and use different credentials in each environment.  It also protects your keys so they never get checked into a git repo by accident.

This further means that you'll have to configure the same environment variables on your development computer, rather than using the more-commonly-documented *aws configure* command that stores credentials in e.g. ~/.aws/credentials on a Mac/Linux computer.

This outlines how to set this up for those unfamiliar with configuring persistent environment variables on a Mac/Linux box.  Let us know if you're trying this from a Windows computer and _not_ using cygwin or Bash on Ubuntu for Windows10.

# Procedure

Edit your shell's configuration file e.g. for Mac users, the default shell is bash, so you'll edit the .bashrc file found in your home directory.  Using a simple menu-driven editor like nano, here's how you'd get into the file to edit it:

```
nano ~/.bashrc
```

Add the following *export* statements, substituting your personal access keys after the equals signs:

```
export AWS_ACCESS_KEY_ID=MYACCESSKEYID
export AWS_SECRET_ACCESS_KEY=MYSECRETACCESSKEY
```

# Notes
Additional discussion of AWS credentials can be found here:
http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html
