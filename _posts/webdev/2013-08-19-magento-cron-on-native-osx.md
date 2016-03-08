---
layout: master
category: magento
title: Magento cron on native OSX
revised-date: 2015-03-08
---

_Revised note_

> This is not the best practice for development since it interferes with native host.<br/>
Please consider using Vagrant & Virtual Box as for web development.

This tutorial is for myself to note some bugs along the way, it could be useful for you too.

Open Mac OS Terminal and type in `crontab -e` to open all the cron tasks in vi.

(To view all current cron task, use `crontab -l`)

``` shell
#Optional, it will send you an email to report, which I found useful when developing
MAILTO=alvin@yourdomain.com

#This is the line when we make the Magento cron job run every 5 minutes
*/5 * * * *  /magento/absolute/path/cron.sh
```

Save the cron by pressing Esc, then type `:wq`, then Enter. You should be familiar with this if vi is your favourite command line editor.

Only finishing this, I got report email from my cron with one single line "expr: syntax error".

That's when I realise the script might not be fully customised to run on Mac OS environment, although Mac is based on UNIX. Well, the simple solution is to replace

``` shell
if [ "$INSTALLDIR" != "" -a "`expr index $CRONSCRIPT /`" != "1" ];then
```

with

``` shell
if [ "$INSTALLDIR" != "" -a "`echo $CRONSCRIPT | sed -n 's/[/].*//p' | wc -c`" != "1" ];then
```

And that's it, it should be running smoothly now.

I'm not sure if this is new, but I've found another behaviour on the cron that it's throwing the error of `SQLSTATE[HY000 [2002]`

The solution for this is to change the host in `app/etc/local.xml` from localhost to `127.0.0.1`