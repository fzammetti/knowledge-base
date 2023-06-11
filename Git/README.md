# Git

---

* [If you get an error about filenames being too long on a pull, run the following command in the git bin directory](#b14c4559-b743-444a-8ff1-21a1b774e509)
* [To copy a repo into a newly created remote repo, including all history, tags and branches](#10ab00f3-ec9e-4907-9e4b-90adc6b91e36)
* [Relocate a Git repo](#012d27d2-ae1a-4196-a65d-5e34349af109)
* [Dealing with SSL errors](#cb2d5c8b-f2d7-4801-b20a-85dbaf978e5f)
* [Clearing repo history](#588363eb-4599-4c06-9d8b-f4a663ac5582)
* [Count the number of lines in files in a Git repo](#95e87a15-94c5-465d-90cb-13e73b1a7d08)

---




<div id="b14c4559-b743-444a-8ff1-21a1b774e509">

## If you get an error about filenames being too long on a pull, run the following command in the git bin directory

</div>

    git config --system core.longpaths true




<div id="10ab00f3-ec9e-4907-9e4b-90adc6b91e36">

## To copy a repo into a newly created remote repo, including all history, tags and branches

</div>

First, clone original repo:

    git clone <original_repo_url>
    git branch -a
    git checkout <branch> (repeat this for all the branches you want, often it will just be master)
    git fetch –tags

Then, confirm you have all tags and branches you want to copy:

    git tag
    git branch -a

  Then, disconnect from original repo:

    git remote rm origin

Then, connect to new repo:

    git remote add origin <new_repo_url>

Finally, push all to new:

    git push origin --all
    git push --tags

New repo should now be a perfect duplicate of the original.

Note: if the new location had any history then you'll probably get some errors. Try this to fix:

    git pull --allow-unrelated-histories

Then re-do the two final push commands.




<div id="012d27d2-ae1a-4196-a65d-5e34349af109">

## Relocate a Git repo

</div>

This can be used when a repo moves, or when its name changes.  This procedure uses TortoiseGit.

Go into Windows Explorer, right-click the repo directory and select TortoiseGit->Settings.  Then, go to the
Git->Remote page, select origin in the Remote box and then update the URL as appropriate.




<div id="cb2d5c8b-f2d7-4801-b20a-85dbaf978e5f">

## Dealing with SSL errors

</div>

If you get SSL errors, like when cloning a repo from GitHub, for example, execute the following at a command prompt:

    git config --global http.sslVerify false

Note that this opens you to MITM attacks, so only do it temporarily and immediately revert it with:

    git config --global http.sslVerify false




<div id="588363eb-4599-4c06-9d8b-f4a663ac5582">

## Clearing repo history

</div>

For situations like when someone pushes sensitive information to a repo, the following procedure can be used.

But, be aware that doing this will delete all repo history, so it should be an absolute last resort and definitely don't do it without discussion with your team first!

This assumes you have the repo on your local machine.

First, delete the .git dir at the root of the repo.

Then, initialize a new repo there, add all files, and commit:

    git init
    git add .
    git commit -m "<valid_commit_message>"

Finally, force-push to the remote (note that you must unprotect the branch in GitLab first if it's protected):

    git remote add origin <repo_url_from_gitlab>
    git push -u --force origin master




<div id="95e87a15-94c5-465d-90cb-13e73b1a7d08">

## Count the number of lines in files in a Git repo

</div>

    git ls-files | xargs wc -l

To limit to only certain types of files, .css for example:

    git ls-files | grep css | xargs wc -l
