# GitLab

---

* [How to set up an action to update a server on push](#e4187d6c-3e9d-447f-832e-2179a5bdcb14)

---




<div id="e4187d6c-3e9d-447f-832e-2179a5bdcb14">

## How to set up an action to update a server on push

</div>

Let's say you have a repo on gitHub, and you want to have the repo hosted on your own web server, and you want GitHub
to update the server whenever you push to the repo.  To do so:

1. Go into GitHub, under your **Profile->Developer Settings->Personal Access Tokens->Fine-Grained Tokens**.  Create a
   Personal Access Token (PAT), select the repo (or all if you prefer) and make sure it has the **Contents** permission.
   You may want to make it non-expiring, but that's a matter of preference.  This token will be used to access the
   repo from your server since GitHub doesn't support username/password anymore.

1. Next, create a public/private keypair to be able to SSH into your server with:

     `ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa`

   Add the public key (**id_rsa.pub**) to the **~/.ssh** directory on the server.  Or, if you're using a host that has
   CPanel installed, look for an **SSH Access** option, where you can do all of this in the UI.

2. Copy the public key (**id_rsa**) into the **~/.ssh** directory on the machine you want to connect to the server with.
   This is (a) to test the keys, and (b) do an initial pull of the repo.  Make sure permissions are set properly:

   `chmod 600 ~/.ssh/id_rsa`

   SSH won't work unless the key is properly locked down.

3. Log into the server:

     `ssh <USERNAME>@<SERVER>`

   Add port on the end if not the standard SSH port.

4. Create a directory for your repo, somewhere that it will be served (**public_html** on some servers).  Ensure it has
   proper permissions and ownership, if applicable.

5. Do an initial pull of the repo:

     `git clone https://<GITHUB_PAT>@github.com/<USERNAME>/<REPO_NAME>.git .`

   That should use your PAT to pull the repo down.  At this point, make sure it's accessible as you expect from a
   browser.

6. Back in GitHub, go to the repo you're working with, go to **Settings->Secrets And Variables->Actions**.  There, in
   the **Repository Secrets** section, add a secret named **<REPO_NAME>_SSH_KEY** and set its value to the private key.

7. In your repo, create a file named **deploy.yml** in the **.github/workflows** subdirectory off the root of the repo
    (create it if needed).  The contents of the file should be:

       name: Deploy to my server
       on:
         push:
           branches:
             - main
       jobs:
         deploy:
           runs-on: ubuntu-latest

           steps:
             - name: Checkout
               uses: actions/checkout@v4

             - name: Set up SSH
               uses: webfactory/ssh-agent@v0.9.0
               with:
                 ssh-private-key: ${{ secrets.<REPO_NAME>_SSH_KEY }}

             - name: Deploy over SSH
               run: |
                 ssh -o StrictHostKeyChecking=no <USERNAME>@<SERVER> '
                   cd <FULL_PATH_TO_REPO_DIRECTORY> &&
                   git pull
                 '

That should do it.  This action should execute on any push to the repo.  Check for problems in the **Actions** tab of
the repo.  Oh, and be sure actions are enabled for the repo and set to the **Allow all actions and reusable workflows**
permission.

Note this assumes there will NEVER be changes to the repo on the server.  If that may not be true, add the
**--ff-only** option to the **git pull** command to be safe.
