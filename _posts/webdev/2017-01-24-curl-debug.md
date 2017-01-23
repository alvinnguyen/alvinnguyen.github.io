---
layout: master
category: webdev
title: Debugging CURL request (PHP)
---

I faced a strange issue with CURL not working correctly. Although the URLs look exactly the same, they output two completely different results.

It was due to htmlentities function being used in one case. Using the CURL debugging snipped below was a live saver.

```
// Other CURL things
//

curl_setopt($handle, CURLOPT_VERBOSE, true);
$verbose = fopen('php://temp', 'w+');
curl_setopt($handle, CURLOPT_STDERR, $verbose);
$result = curl_exec($handle);

rewind($verbose);
$verboseLog = stream_get_contents($verbose);
echo "Verbose information:\n<pre>", htmlspecialchars($verboseLog), "</pre>\n";

```