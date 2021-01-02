# Using Git for Example Code and Assignments

We will be using Git for accessing example code and submitting assignments on Github. Git is a very powerful tool, but the way we will use it for the course will be quite simple. In all cases, repositories will be created for you and you will "clone" the repositories to get started.

You will need a [GitHub account](https://github.com/join) if you don't already have one to perform the steps below, and to submit assignments and challenges in the course.

We ask that you follow these below carefully, and only stray from them if you know what you are doing. There are lots of great git learning resources online like the free [Pro Git Book](https://git-scm.com/book/en/v2).

## Example Code

To access example code for the course there is a repository setup on the course's [Github organization page](https://CS-3035-Fall-2020.github.io). This repository will provide you with examples to get you started and that we have covered in class. The code in the repository will grow and change over time, so you will want to regularly "pull" changes and new code, so that you have the latest versions.

### Accessing Example Code in IntelliJ

In IntelliJ do the following:

- ```File -> New -> Project From Version Control -> Repository URL```
- Enter ```https://github.com/CS-3035-Fall-2020/CS-3035-example-code.git``` for the URL
- You can open a new window for the CS3035-example-code project or you can open in the same window (which closes your current project)

#### Running Examples

- Each example has a directory (or package) in the ```src``` directory
- For each example you will generally find a Main class that initializes the app
- You can run any app by ```right click it's Main class -> left click "Run Main.main()"```

### Pull Examples Often

Remember to pull the code from the examples repo often

#### Watch for Changes

To help you keep up to date, you can watch the examples repository... that way you can get frequent updates:

- Visit <https://github.com/CS-3035-Fall-2020/CS-3035-example-code>
- Click 'watch' in the top right hand corner

#### Pull Changes

To pull changes from the repository in IntelliJ you can do ```ctrl + t``` on Windows, ```click the blue arrow``` on the toolbar that points to the lower left, or ```VCS->Git->Pull```

## Assignments

Assignments will be hosted and submitted on GitHub. You will need to follow the links below. For convenience we have also set up an example that will be your first Challenge exercise for class.

***Assignment instructions*** will always be found in the assignment repository's readme.md file.

- For assignments you will be provided a repository invitation link.
- You will follow the link, login to GitHub (if not already logged in) and accept the invitation.
- This will create a new repository for you.
- A link will be provided for a repository that has been automatically created on your account
- Follow the link to view your Repo
- In IntelliJ Start a "New Project from Version Control"
- Choose GitHub and make sure you are logged in
- You should be able to select your repo for the assignment from your list or search for it
- Select the repo and click Clone
- Once you have completed your changes. You need to commit all of them.
- To ***commit***:
  - Right click on the top level directory of your project in your "Project" sidebar
  - Click Git -> Commit Directory...
  - Type in a descriptive commit message outlining your changes and click "commit"
- Finally you can ***push*** your commits to GitHub to submit your challenge/assignment
  - Click the "Green arrow" on the toolbar
  - or, ```ctrl + shift + k``` on Windows
  - or, ```VCS -> Git -> Push```
  - Click "Push"
- Finally, visit your repo on GitHub to make sure your changes have been pushed.

## Challenge 1 - Practicing Submitting Assignments

Your first weekly challenge is to get practice with starting an assignment on GitHub and submitting your changes to the repo on GitHub to submit your Challenge assignment.

**This challenge is due on Tuesday, September 15 at 4pm.**

- Review the instructions for [setting up your development software](CS3035-software-setup.md) and the instructions above for submitting assignments.
- Accept the Challenge invitation using this link: [Invitation to Challenge 1](https://classroom.github.com/a/kCfudltV)
- From IntelliJ create a "New Project from Version Control" using GitHub
- Search and/or Select Challenge-1-Your_GitHub_Username from the list of repositories
- Select your repo from the list and click Clone
- Find and edit your readme.md file
- Follow the steps above for ***committing*** your changes and ***pushing*** them to GitHub
- Verify that your updated readme.md file has been updated on GitHub
