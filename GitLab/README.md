# GitLab

---

* [How to remove a CI/CD runner in GitLab](#0316097f-1ff4-42b5-b370-ca21806a237d)
* [To configure SMTP in GitLab (using non-SSL SMTP server)](#0c285472-9577-480e-ae55-fe88eb9400dc)
* [Skipping GitLab CI/CD pipeline](#ee700974-96aa-41b5-a0e8-40ee71994a7f)
* [Download a file from GitLab using the GitLab API](#3fa50e07-3a93-4096-86ff-e6c78e103253)

---




<div id="0316097f-1ff4-42b5-b370-ca21806a237d">

## How to remove a CI/CD runner in GitLab

</div>

    gitlab-runner unregister --name <name>
    gitlab-runner verify –delete
    gitlab-runner restart
    gitlab-runner verify
    gitlab-runner list




<div id="0c285472-9577-480e-ae55-fe88eb9400dc">

## To configure SMTP in GitLab (using non-SSL SMTP server)

</div>

NOTE: I suspect this has to be re-done after every GitLab update.

Edit /etc/gitlab/gitlab.rb, uncomment the following lines and fill in these values:

    gitlab_rails['smtp_enable'] = true
    gitlab_rails['smtp_address'] = "<smtp_server_address>"
    gitlab_rails['smtp_port'] = <smpt_server_port>
    gitlab_rails['smtp_user_name'] = <username>
    gitlab_rails['smtp_password'] = "<password>"
    gitlab_rails['smtp_domain'] = "<domain>"
    gitlab_rails['smtp_authentication'] = "login"
    gitlab_rails['smtp_enable_starttls_auto'] = false
    gitlab_rails['smtp_tls'] = false
    gitlab_rails['smtp_pool'] = false
    ...
    gitlab_rails['smtp_openssl_verify_mode'] = 'none'
    ...
    gitlab_rails['gitlab_email_from'] = '<a_valid_address_in_the_domain>'
    gitlab_rails['gitlab_email_reply_to'] = '<a_valid_address_in_the_domain>'

After editing the file execute:

    gitlab-ctl reconfigure

To test, launch console:

    gitlab-rails console -e production

Then execute these commands:

    ActionMailer::Base.delivery_method

Confirm it says SMTP.

    ActionMailer::Base.smtp_settings

Confirm the proper settings are shown.

    Notify.test_email('frank@zammetti.com', 'Hello World', 'This is a test message').deliver_now

Confirm email is received properly.




<div id="ee700974-96aa-41b5-a0e8-40ee71994a7f">

## Skipping GitLab CI/CD pipeline

</div>

To skip CI/CD pipeline, add **[ci skip]** or **[skip ci]** to commit message




<div id="3fa50e07-3a93-4096-86ff-e6c78e103253">

## Download a file from GitLab using the GitLab API

</div>

First, go into GitLab and go to Your Profile->Access Tokens.  Create a new PERSONAL access token (a project token will
NOT work), name it temp (or anything, it doesn't really matter), enable read_repository scope, and copy value it
generates.

Then, to download file:

    curl --header "PRIVATE-TOKEN: <PAT>" "https://<GITLAB_DOMAIN>/api/v4/projects/<PROJECT_ID>/repository/files/<FILENAME>/raw?ref=<BRANCH>" --output <FILENAME>

Replace the placeholders with real values, might need to alter path.  The Project ID can be found by going into GitLab,
going to the correct project, then going to Settings->General.
