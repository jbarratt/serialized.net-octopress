--- 
wordpress_id: 366
layout: post
title: Getting around tmpfs 'noexec' problems
wordpress_url: http://serialized.net/?p=366
---
In general, running your /tmp (or /var/tmp) without the execute bit set is a good idea. And sometimes, you don't have a choice -- for example, when running in a hosting environment running Virtuozzo.

You're liable to see a mount that looks like this:
{% codeblock lang:console %}
$ mount | grep noexec
/dev/simfs on /tmp type simfs (rw,noexec)
/dev/simfs on /var/tmp type simfs (rw,noexec)
devpts on /dev/pts type devpts (rw,nosuid,noexec)
{% endcodeblock %}

However, sometimes that's a problem, for example if you run the fantastic [checkinstall](http://www.asic-linux.com.mx/~izto/checkinstall/) tool to package software. You might see an error message like this:

{% codeblock lang:text %}
/usr/bin/installwatch: /var/tmp/tmp.SuogJyYftZ/installscript.sh: /bin/sh: bad interpreter: Permission denied
{% endcodeblock %}

The noexec permission has gotten us. How to work around it?
Here's a quick, easy to roll back method:

{% codeblock lang:console %}
# make a new temporary directory for this use
mkdir ~/tmptmp
# use mount --bind to overlay this on our 'real' /var/tmp for now
$ sudo mount --bind ~/tmptmp /var/tmp
# do your work
$ sudo checkinstall make install
# restore the natural order to the universe
$ sudo umount /var/tmp 
{% endcodeblock %}

You could also (potentially) remount /tmp without that option temporarily, but that isn't always possible. (See 'virtuozzo' above.)
