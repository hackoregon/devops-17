# Summary
TravisCI is used by HackOregon to build and test frontend and backend code for each of their projects.

Every time a developer commits code to a Hack Oregon repo, the connected TravisCI 'builder' (if any) will run all properly-included tests.  The Job log provides detailed logging of all actions performed by Travis in that build.

If a developer commits something that won't build/deploy correctly, the TravisCI log is the first place to look to determine what went wrong and what should be corrected.


## Viewing TravisCI build results: other members of the team
- the person who initially setup the TravisCI build from your team's repo is able to watch every build already
- however, the rest of the team members may wish to see what happens during the build (in case errors occur that they want to fix)
- for those additional team members who are contributors to the team's GitHub repo, they'll have to "activate"  the repo in their own TravisCI account:
  - look for the My Repositories area of your [Travis account](https://travis-ci.org), and click the *+* link
  - click the *Sync acccount* button to get the repo to show up in your account's listings
  - browse back to the [main Travis page](https://travis-ci.org)
  - You should see the project listed there now, with the results of the last build displayed by default
