title=Broken GPU Acceleration for Google Chrome on Intel
date=2014-11-22
type=post
tags=blog, chrome, intel, dri, gpu
status=published
~~~~~~

Ugly fix
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