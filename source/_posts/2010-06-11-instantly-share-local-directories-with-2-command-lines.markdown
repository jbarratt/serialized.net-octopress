--- 
wordpress_id: 385
layout: post
title: Instantly share local directories with 2 command lines
wordpress_url: http://serialized.net/?p=385
---
Just came across this useful trick to let anyone browse/download directories you want to share in seconds.

All you need is a Mac or Linux machine you are logged into, and ssh access to a server on the internet. It uses [this trick](http://www.lylebackenroth.com/blog/2009/05/03/serve-your-current-directory-using-a-simple-webserver-python/) I just saw the other day.

{% codeblock lang:console %}
$ python -m SimpleHTTPServer &
$ ssh -R 8080:localhost:8000 my.remote.host
{% endcodeblock %}

Now someone can go to http://my.remote.host:8080/ and be browsing your local directory. Rad. And you see the access that's happening come up in your screen -- SimpleHTTPServer spits it out to you.

When they've got what they need, just 
{% codeblock lang:console %}
my.remote.host$ exit
$ fg # Brings python back, CTRL-C to exit
{% endcodeblock %}

Sharing over, nice and securely back to normal.

If this isn't working, the most common reason is that on the remote server, you do need the 'GatewayPorts' set to 'yes'  in your server's /etc/ssh/sshd_config for this to work. (And you'll need to restart sshd after you make that change.)
