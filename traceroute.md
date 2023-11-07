# How to use `tracert`/`traceroute`:

## Windows

Open "Command Prompt" and execute `tracert <hostname or IP>`.

If you do not want to wait too long, you could do `tracert -w 1000 -d <hostname or IP>` instead. It reduces the timeout to 1 second and skips the reverse DNS lookup. More info is [here](https://www.lifewire.com/tracert-command-2618101).

For some reason, the reverse DNS lookup in Windows is very slow. The culprit is [NetBIOS over TCP/IP](https://en.wikipedia.org/wiki/NetBIOS_over_TCP/IP). It slows down reverse DNS lookups and introduces security vulnerabilities. You might want to [turn it off](https://woshub.com/how-to-disable-netbios-over-tcpip-and-llmnr-using-gpo/) as soon as possible.

If you have done the steps above, you can do `tracert -w 1000 <hostname or IP>` since lookups are much faster now.

Note: `tracert` only supports ICMP, therefore not reflecting the most accurate routing from your network to YouTube. You could use some third-party traceroute tools, but make sure to scan for malware!

## Linux

Open "Terminal", and execute `traceroute <hostname or IP>`.

However, personally, I would add some flags:

```
-N 3        Specifies the number of probe packets sent out simultaneously. Sending several probes concurrently can speed up traceroute considerably. The default value is 16. Note that some routers and hosts can use ICMP rate throttling. In such a situation specifying too large number can lead to loss of some responses.

-w 1        Set the time (in seconds) to wait for a response to a probe (default 5.0 sec).

-z 0.1      Minimal time interval between probes (default 0). If the value is more than 10, then it specifies a number in milliseconds, else it is a number of seconds (float point values allowed too). Useful when some routers use rate-limit for icmp messages.
```

Also, your browser/app will send and receive data from YouTube servers through HTTPS, so you may also want to use TCP port 443.

Last but not least, `traceroute` using ICMP or TCP will need `root` access. Therefore, the command will look like this:

```
sudo traceroute -N 3 -w 1 -z 0.1 -T -p 443 <hostname or IP>
```

More info is [here](https://linux.die.net/man/8/traceroute).
