---
layout: master
category: webdev
title: Varnish logging and grep outputing to file
---

Just a quick note to log the Varnish logging

Manual: <a href="http://www.varnish-cache.org/docs/trunk/reference/varnishncsa.html">varnishncsa</a>

```
nohup varnishncsa -F "%{Set-Cookie}o %U %{Varnish:handling}x" | grep --line-buffered "frontend=" >> varnish.out &
```

My current requirement is to monitor all the Set-Cookie header being returned from Varnish since it'll contain the user's session.

Ideally, the user session should only be set for certain route, hence the monitor is to identify any potential problems of cross-session.

I also learnt that without `--line-buffered` option for grep, the piping to output file just doesn't seem to work.