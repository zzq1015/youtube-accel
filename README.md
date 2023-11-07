# youtube-accel (YouTube Acceleration)

No more buffering or pixelated videos when watching YouTube!!!

As of late 2023, YouTube has become the de-facto library of human knowledge. Its operating company, Google, [has YouTube servers all around the globe](https://www.google.com/get/videoqualityreport/#how_video_gets_to_you), while [peering with every major ISP](https://peering.google.com/). Therefore, in theory, you should be able to watch YouTube videos smoothly without any buffering.

However, there are still some people who have decent internet, but YouTube buffers all the time. It's time to find out why.

## YouTube Video Servers (gvs)

While watching YouTube videos, YouTube grabs your IP address, maps it to your current location, routes your request to the fastest server near you, and sends you the video data.

There are a few known endpoints YouTube uses to map your IP address to the closest server:

- https://redirector.googlevideo.com/report_mapping (Most common, used on YouTube Mobile/Tablet/TV App)
- https://redirector.gvt1.com/report_mapping (I haven't seen that yet)
- http://redirector.c.youtube.com/report_mapping (I haven't seen that yet, also no HTTPS support)
- https://www.youtube.com/ (If you are using a desktop browser)
- https://m.youtube.com/ (If you are using a mobile browser)

If you manually [open the debug page](https://redirector.googlevideo.com/report_mapping "YouTube Report Mapping"), You'll see something like this:

```
xxx.xxx.xxx.xxx => xxxxxx-xxxx          (xxx.xxx.xxx.xxx/xx)
^^^^^^^^^^^^^^^    ^^^^^^^^^^^          ^^^^^^^^^^^^^^^^^^^^
Your IP address    YouTube-Server ID    Your IP CIDR range

Debug Info: (Appears to be encrypted)
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
xxxxxxxxxxxxxxx...
```

And then, your browser/app will get the video from somewhere looking like this:

```
rx---sn-xxxxxx-xxxx.googlevideo.com
rrx---sn-xxxxxx-xxxx.googlevideo.com
rrx---sn-xxxxxx-xxxx.c.youtube.com
```

To determine the exact home name, use "Inspect Element" (Or press F12), go to the `Network` tab, and refresh the page when watching a video.

You can try `nslookup` the address in Command Pronpt/Terminal like this:

```
nslookup rrx---sn-xxxxxx-xxxx.googlevideo.com
```

If you encounter the error `nslookup: 'rx---sn-xxxxxx-xxxx.googlevideo.com' is not a legal IDNA2008 name` while using Linux, please execute `export IDN_DISABLE=1` and try again.

`nslookup` will fetch both the IPv4 and v6 addresses for you, which is nice.

And then `ping` the IP address you just got. Make sure there's no packet loss and latency less than 50ms.

You can also `tracert` (Windows) or `traceroute` (MacOS/Linux) the IP addresses.

The steps above will locate the issue of why your YouTube buffers. Depending on the result, you either complain to/switch your ISP or re-configure your network.
