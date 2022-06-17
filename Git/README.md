# Git
----------

##  If you get an error about filenames being too long on a pull, run the following command in the git bin directory

    git config --system core.longpaths true

##  To copy a repo into a newly created remote repo, including all history, tags and branches

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