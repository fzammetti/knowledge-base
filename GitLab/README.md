# GitLab
----------

•	How to remove a CI/CD runner in GitLab:

    gitlab-runner unregister --name <name>
    gitlab-runner verify –delete
    gitlab-runner restart
    gitlab-runner verify
    gitlab-runner list

•	To configure SMTP in GitLab (using non-SSL SMTP server):

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

