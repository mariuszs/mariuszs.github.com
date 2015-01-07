title=Broken GPU Acceleration for Google Chrome on Intel
date=2014-11-22
type=post
tags=blog, chrome, intel, dri, gpu
status=published
~~~~~~

Update
----

This issue is already fixed in Google Chrome Beta (40.0.2214.45 beta).

<del>Ugly fix</del>
----

    set -x LIBGL_DRI3_DISABLE 1
    google-chrome

Result
----

![alt text](/res/chrome-gpu.png "chrome://gpu screen")

Source
----

* http://keithp.com/blogs/chromium-dri3/
* https://code.google.com/p/chromium/issues/detail?id=415681