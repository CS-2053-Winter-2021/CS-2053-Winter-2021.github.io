# Using Git for Example Code and Assignments

We will be using Git (the distributed source control tool) and GitHub (a fancy website that provides tools and hosting for Git code repositories) for completing assignments, doing your course project and doing the lab exams. Git is a very powerful tool, but the way we will use it for the course will be quite simple. In all cases, repositories will be created for you and you will "clone" the repositories to get started.

Before you get started with the details in this page you should complete the steps in [Software Setup and Requirements page](pages/cs2053-requirements-and-setup.md).

## Create and Register a GitHub Account
You will need a GitHub Account. Please see these instructions: [To create and register your GitHub account](https://cs-2053-winter-2021.github.io/en_CA/#!pages/cs2053-requirements-and-setup.md)

When using Git we ask that you follow these instructions below carefully, and only stray from them if you know what you are doing. 

## Completing Assignments and Lab Exams

Assignments and lab exams will be hosted and submitted on GitHub. You will need to follow the instructions below and we will record an example of how to do this for you during our first lab, which will be posted to our [Course Videos on Streams](https://web.microsoftstream.com/channel/8661cb6d-aa10-4b66-8ccb-fafdfd06081b). 

## Starting a Lab Assignment or Lab Exam by Cloning a Repository
These steps will be repeated for Lab Exams and Lab Assignments:

1. For assignments you will be provided a repository invitation link.
2. You will follow the link, login to GitHub (if not already logged in) and accept the invitation.
3. This will create a new repository for you and a link will be provided for a repository that has been automatically created on your account.
   1. This repo provides you with all of the details and instructions for completing your work.
   2. You can follow the link to your repo to view it.
4. Open GitHub Desktop and Click "File -> Clone Repository"
5. Make sure that "GitHub.com" is selected at the top, and filter by typing in the name of your repo... For example, "Lab-Assignment-1" (without the quotes).
6. Select your repo from the list and choose a place on your local drive to "clone it to" (i.e., download it), and click "Clone".
7. Once cloned your newly cloned repository should be selected in your list under "Current Repository" in GitHub Desktop. You can change your selection in that list. If you need to work with different projects.
   1. Your repos for coursework will be private and should not be shared with any other students in the class. They are accessible by the instructor and TAs.
8. Once cloned you can open your repository in Unity. Open Unity Hub, make sure "Projects" is selected on the left hand menu, and click "Add". Navigate to the location on your computer where you cloned your repository to and click "Select Folder" on the top level directory of the project repo.
9. Next, select the "Unity Version" for your project. It should be the latest 2019.4.xx 
10. Finally, click on your project in the list to open it.
    1.  You can complete your work as normal, following the provided instructions. 

## Submitting Your Lab Assignment or Lab Exam - "committing" and "pushing" your code

GitHub provides you with a lot of flexibility with how you can complete your work. However, we will use it in a very simple way... and just "commit" your changes to your local repository, and "pushing" your commits to GitHub.

### Committing

1. First you must commit any changes you have. Open GitHub Desktop and make sure you have the correct repository selected under "current repository"
2. Once selected, confirm that all changes you have made are listed under the changes. You can inspect the contents of the files and their difference from previous versions, by selecting them in the "changed files" list.
3. Once you are satisfied, type in a short summary that describes your changed and click "Commit".
   1. Note you can commit as frequently as you like. This stores all incremental changes since the last commit locally on your computer (but not on GitHub)
4. Next you will need to Push your code to store it on the GitHub website so that it can be saved off of your computer and be accessible for grading.

### Pushing Your Code

You can also "push" code as frequently as you like to GitHub. This allows you to store all of your work and commit history to the "cloud".

For assignments and lab exams, we will grade only the most recent version of your code posted to GitHub before the deadline (unless otherwise agreed upon).

1. Make sure you have your project selected in GitHub Desktop and you have "committed" all your work (see steps listed above).
2. In the main window click the "Push origin" button. 
3. Your code should now be uploaded and stored on GitHub. To be safe it is always good practice that you can view your latest commits on the GitHub website to make sure everything as worked as intended.

## More Details on How to Use Git
There are lots of great Git learning resources online like the free [Pro Git Book](https://git-scm.com/book/en/v2), if you would like to know more about how to use Git.
