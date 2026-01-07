# Log Hunt - PicoCTF
--------------------------------

URL: https://play.picoctf.org/practice/challenge/527
Category: General Skills

> Challenge Description
Our server seems to be leaking pieces of a secret flag in its logs. The parts are scattered and sometimes repeated. Can you reconstruct the original flag?
Download the logs and figure out the full flag from the fragments.

LogFile URL = https://challenge-files.picoctf.net/c_amiable_citadel/49cec6157142f24a599f4164d5b63322c2494f801390d6f22eb91b3aa592bc66/server.log

## Solution
-------------------------------------------
I don't even know whether to write this writeup, just because I am determined to write writeup for every challenge on PicoCTF, you can even just look through the log file and build the flag that way, What I did was use some bash magic to retrieve the flag.

Looking through the file we find the pattern, 
```txt
[1990-08-09 10:00:10] INFO FLAGPART: picoCTF{us3_
[1990-08-09 10:00:16] WARN Disk space low
[1990-08-09 10:00:19] DEBUG Cache cleared
[1990-08-09 10:00:23] WARN Disk space low
[1990-08-09 10:00:25] INFO Service restarted
[1990-08-09 10:00:33] WARN Disk space low
[1990-08-09 10:00:38] ERROR Connection lost
[1990-08-09 10:00:46] ERROR Failed login attempt
[1990-08-09 10:00:48] INFO User logged in
[1990-08-09 10:00:50] INFO User logged in
[1990-08-09 10:00:59] ERROR Failed login attempt
[1990-08-09 10:01:04] DEBUG System check complete
[1990-08-09 10:01:05] WARN Disk space low
[1990-08-09 10:01:13] INFO Service restarted
[1990-08-09 10:01:17] INFO Service restarted
[1990-08-09 10:01:18] ERROR Failed login attempt
[1990-08-09 10:01:24] ERROR Connection lost
[1990-08-09 10:01:25] WARN High memory usage detected
[1990-08-09 10:01:31] ERROR Connection lost
[1990-08-09 10:01:36] INFO Service restarted
[1990-08-09 10:01:41] ERROR Connection lost
[1990-08-09 10:01:44] WARN High memory usage detected
[1990-08-09 10:01:53] INFO Scheduled task run
[1990-08-09 10:01:54] DEBUG System check complete
[1990-08-09 10:01:57] DEBUG System check complete
[1990-08-09 10:01:58] INFO Scheduled task run
[1990-08-09 10:02:04] WARN Disk space low
[1990-08-09 10:02:07] INFO Service restarted
[1990-08-09 10:02:16] ERROR Failed login attempt
[1990-08-09 10:02:26] DEBUG Cache cleared
[1990-08-09 10:02:30] DEBUG System check complete
[1990-08-09 10:02:37] ERROR Connection lost
[1990-08-09 10:02:45] INFO User logged in
[1990-08-09 10:02:55] INFO FLAGPART: y0urlinux_
[1990-08-09 10:03:03] WARN High memory usage detected
...
...
```

There is a pattern, the line specifically says **FLAGPART**, containing part of the flag

We can use grep to just display lines that contains FLAGPART
```bash
cat server.log| grep FLAGPART
```

OUTPUT:
```
[1990-08-09 10:00:10] INFO FLAGPART: picoCTF{us3_
[1990-08-09 10:02:55] INFO FLAGPART: y0urlinux_
[1990-08-09 10:05:54] INFO FLAGPART: sk1lls_
[1990-08-09 10:05:55] INFO FLAGPART: sk1lls_
[1990-08-09 10:10:54] INFO FLAGPART: cedfa5fb}
[1990-08-09 10:10:58] INFO FLAGPART: cedfa5fb}
[1990-08-09 10:11:06] INFO FLAGPART: cedfa5fb}
[1990-08-09 11:04:27] INFO FLAGPART: picoCTF{us3_
[1990-08-09 11:04:29] INFO FLAGPART: picoCTF{us3_
[1990-08-09 11:04:37] INFO FLAGPART: picoCTF{us3_
[1990-08-09 11:09:16] INFO FLAGPART: y0urlinux_
[1990-08-09 11:09:19] INFO FLAGPART: y0urlinux_
[1990-08-09 11:12:40] INFO FLAGPART: sk1lls_
[1990-08-09 11:12:45] INFO FLAGPART: sk1lls_
[1990-08-09 11:16:58] INFO FLAGPART: cedfa5fb}
[1990-08-09 11:16:59] INFO FLAGPART: cedfa5fb}
[1990-08-09 11:17:00] INFO FLAGPART: cedfa5fb}
[1990-08-09 12:19:23] INFO FLAGPART: picoCTF{us3_
[1990-08-09 12:19:29] INFO FLAGPART: picoCTF{us3_
[1990-08-09 12:19:32] INFO FLAGPART: picoCTF{us3_
[1990-08-09 12:23:43] INFO FLAGPART: y0urlinux_
[1990-08-09 12:23:45] INFO FLAGPART: y0urlinux_
[1990-08-09 12:23:53] INFO FLAGPART: y0urlinux_
[1990-08-09 12:25:32] INFO FLAGPART: sk1lls_
[1990-08-09 12:28:45] INFO FLAGPART: cedfa5fb}
[1990-08-09 12:28:49] INFO FLAGPART: cedfa5fb}
[1990-08-09 12:28:52] INFO FLAGPART: cedfa5fb}
```

Now using awk to seperate fields, and displaying fifth field (which is flag part}

```bash
cat server.log | grep FLAGPART| awk '/FLAGPART/ {print $5}'
```

OUTPUT:

```
picoCTF{us3_
y0urlinux_
sk1lls_
sk1lls_
cedfa5fb}
cedfa5fb}
cedfa5fb}
picoCTF{us3_
picoCTF{us3_
picoCTF{us3_
y0urlinux_
y0urlinux_
sk1lls_
sk1lls_
cedfa5fb}
cedfa5fb}
cedfa5fb}
picoCTF{us3_
picoCTF{us3_
picoCTF{us3_
y0urlinux_
y0urlinux_
y0urlinux_
sk1lls_
cedfa5fb}
cedfa5fb}
cedfa5fb}
```

Now we use uniq, to select only unique ones and tr to trim down new line (\n)
```bash
cat server.log | grep FLAGPART| awk '/FLAGPART/ {print $5}'| uniq| tr -d '\n'
```

OUTPUT:

```
picoCTF{us3_y0urlinux_sk1lls_cedfa5fb}picoCTF{us3_y0urlinux_sk1lls_cedfa5fb}picoCTF{us3_y0urlinux_sk1lls_cedfa5fb}
```

Hope you learnt something, I know there are other, best, efficient way to ahieve this very same goal, this is just how I did it...
