---
layout: master
category: webdev
title: Scrapbook Chrome extension development
---

Install here: <a href="https://chrome.google.com/webstore/detail/chrome-scrapbook/ibnjlibjbocfijndlhlfdkjihmjlmibd">https://chrome.google.com/webstore/detail/chrome-scrapbook/ibnjlibjbocfijndlhlfdkjihmjlmibd</a>

This morning I've just re-checked my 6-month old Chrome extension and was surprised to see there is around 350 weekly users. It's doing not that bad for a throw-away extension. I will be walking you through how I built this just to refresh my memory as well as give an insight on the Chrome extension scripting.

The idea came up when I was browsing around and found myself constantly copy things from the site to a note in TextEdit. I then realise I could make an extension and have it stored right in Chrome. In the early version, the extension used `localStorage` to store the note; since then I've improved it to use Google cloud storage to store the data. It means if the data will be available accross different Chrome installation (given they are logged in).

**icon128.png** - This graphic file is at 128x128 resolution and can be named as you wish given it's declared correctedly in the manifest.json file.

**manifest.json** - This is the description file to contain all details related to the extension

``` js
{
  "manifest_version": 2,
  "name": "Chrome Scrapbook",
  "description": "This is a quick and dirty way to save notes across your Chrome.",
  "version": "1.3",
  "icons": {"128":"icon128.png"},
  "content_security_policy": "script-src 'self' https://ssl.google-analytics.com; object-src 'self'",
  "browser_action": {
    "default_icon": "icon128.png",
    "default_popup": "popup.html"
  },
  "permissions": [
    "storage"
  ]
}
```

**popup.html**

``` html
<!doctype html>
<html>
<head>
    <title>Chrome Scrapbook</title>
    <style>
        body {
            font-family: "Helvetica Neue", "Helvetica", san-serif;
            font-size:12px;
            min-width: 357px;
            min-height:400px;
            overflow-x: hidden;
            padding:0px;
            margin:0px;
            border:0px;
        }
        img {
            margin: 5px;
            border: 2px solid black;
            vertical-align: middle;
            width: 75px;
            height: 75px;
        }

        header{
            font-weight: bold;
            text-align: center;
            text-transform: uppercase;
            background:white;
            font-size:14px;
            padding: 4px 0px 6px 0px;
            border-bottom: 1px dashed black;
        }

        textarea{
            min-width:341px;max-width:341px;
            min-height:384px;max-height:384px;
            padding:8px;
            font-family: "Helvetica Neue", "Helvetica", san-serif;
            font-size:12px;
            outline: none;border:none;
            line-height: normal;
            resize: none;
        }

        #save{
            font-weight: bold;
            text-align: center;
            padding:5px 0px;
            color:black;
            text-transform: uppercase;
            opacity: 0.1;
        }
    </style>
    <script src="popup.js"></script>

</head>
<body>
<header>
    CHROME SCRAPBOOK
</header>
<div class="body">
    <textarea id="textform">This is some text for your thought.</textarea>
    <div id="save">SAVED</div>
</div>
</body>
</html>
```

**popup.js** - Our hero javascript file that will drive the extension

``` js
// Copyright (c) 2016 Alvin Nguyen <alvin@alvinnguyen.net>. All rights reserved.

document.addEventListener('DOMContentLoaded', function () {
    var textarea = document.getElementById('textform');
    console.log(window.getSelection().toString());
    chrome.storage.sync.get('value', function (object) {
        textarea.focus();
        textarea.value = "";
        textarea.value = object['value'];
        var x = setTimeout(function () {
            moveCursorToEnd(textarea);
        }, 100);
    });

    textarea.addEventListener("input", function () {

        var theValue = textarea.value;
        // Check that there's some code there.
        if (!theValue) {
            // message('Error: No value specified');
            return;
        }

        //Save it using the Chrome extension storage API.
        chrome.storage.sync.set({'value': theValue}, function () {
            // Notify that we saved.
            document.getElementById("save").style.opacity = 1;
            setTimeout(function () {
                document.getElementById("save").style.opacity = 0.1;
            }, 1000);
            // message('Settings saved');
        });
    });


});

function moveCursorToEnd(el) {
    if (typeof el.selectionStart == "number") {
        el.selectionStart = el.selectionEnd = el.value.length;
    } else if (typeof el.createTextRange != "undefined") {
        el.focus();
        var range = el.createTextRange();
        range.collapse(false);
        range.select();
    }
}

var _gaq = _gaq || [];
_gaq.push(['_setAccount', 'UA-64405613-1']);
_gaq.push(['_trackPageview']);

(function() {
    var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
    ga.src = 'https://ssl.google-analytics.com/ga.js';
    var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
})();
```