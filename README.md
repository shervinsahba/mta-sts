# MTA-STS

> This repo hosts a MTA-STS security file for use with my custom email. Below are the instructions, which I learned from [this blog](https://emailsecurity.blog/hosting-your-mta-sts-policy-using-github-pages
). In short, we create a GitHub Page that serves a raw text file, and the custom email domain uses this for its MTA-STS policy.

## Guide

1. Create a repo (or fork this one to skip steps) and disable the Jekyll service that GitHub Pages uses by creating a blank `.nojekyll` file
2. Create `.well-known/mta-sts.txt` with contents
```
      version: STSv1
      mode: enforce
      mx: your-mx-record-here.com
      mx: your-mx-record-here.com
      max_age: 604800
```
Here you want to edit in your MX records from your DNS. For the mode, enforce is the end goal, but you can set to testing at first.
3. At your DNS provider, create a CNAME record with **Host:** `mta-sts` and **Value:** `your-github-username.github.io`
4. On the GitHub repo go to Settings and then Pages. Set the source to the `main` branch and save
5. Under custom domain enter `mta-sts.yourdomain.com` and save
6. Wait a few minutes and then refresh the page so you can enable **Enforce HTTPS**

You should now see the MTA-STS policy at https://mta-sts.yourdomain.com/.well-known/mta-sts.txt

7. At your DNS provider, create a TXT record with **Host:** `_mta-sts` and **Value:** `v=STSv1; id=1234567890`. The `id` value here can be any numerical string, but it might be nice to use the epoch date (i.e. `date +%s`)
8. To receive TLS-RPT reports, you need a mailbox to receive them or a service like [MailHardener](mailhardener.com). Create a TXT record with **Host:** `_smtp._tls` and **Value:** `v=TLSRPTv1; rua=mailto:reporting_service@example.com`

boomshakalakalaka
