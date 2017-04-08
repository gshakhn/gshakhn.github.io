---
title:  "On renewing Lets Encrypt certs AKA security is fun"
date:   2017-04-08 17:00:00
---

Every 75 days, I perform the tedious task of renewing SSL certs for this blog. Let's Encrypt offers free SSL certs, which is awesome, but they only last for 90 days. Given the long time frame, I always forget the commands and resort to `history | grep certbot` to figure out what I did last time. Shell history is awesome, but in the spirit of [taking notes]({% post_url 2017-03-25-on-note-taking %}) and writing a blog post once a week, I'm posting the commands here as well.

`certbot` supports a non-interactive auto-renewal mode, but that doesn't work if you're not running it on the actual web server. Since this blog is currently hosted on GitLab Pages, I have to renew it manually.

`sudo certbot certonly -a manual -d www.gshakhn.com -d gshakhn.com` renews the cert.

Once it's renewed, update the PEM files on GitLab pages by deleting and recreating the domain. On macOS, `pbcopy` makes things easy.

    sudo cat /etc/letsencrypt/live/www.gshakhn.com/fullchain.pem | pbcopy
    sudo cat /etc/letsencrypt/live/www.gshakhn.com/privkey.pem | pbcopy
    
I also backup the certs by `sudo cp -r /etc/letsencrypt/` to an encrypted disk on Dropbox.