---
title: Why is @font-face throwing a 404 error on woff files?
categories:
  - 技术分享
  - IIS
date: 2017-02-26 05:54:08
tags: [IIS]
thumbnail: https://lh3.googleusercontent.com/-fgXtFbtGEjs/WLB6L8nmK2I/AAAAAAAABsw/jthPMpMQWtw/s0/2017-02-25_03-23-42.png
---
<!--excerpt-->

I also came across the same issue. I think doing this configuration from the server level would be better since it applies for all the websites.

1. Go to IIS root node and double-click the ``MIME Types`` configuration option
2. Click "Add" link in the Actions panel on the top right.
3. This will bring up a dialog. Add .woff file extension and specify ``application/x-font-wof`` as the corresponding MIME type.

Add MIME Type for ``.woff`` file name extension

![](https://lh3.googleusercontent.com/-BwnUFnLQpJA/WLJuyRrq9xI/AAAAAAAABtQ/5egeZtwfb5w/s0/2017-02-26_14-59-36.png)
![](https://lh3.googleusercontent.com/-S3VlMYYqh2g/WLJu8FHq3bI/AAAAAAAABtU/vRpmUMPhJQE/s0/2017-02-26_15-00-16.png)