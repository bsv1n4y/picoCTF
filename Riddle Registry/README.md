# Riddle Registry - PicoCTF
----------------------------------

URL: https://play.picoctf.org/practice/challenge/530
Category: Forensics

> Challenge Description
Hi, intrepid investigator! 📄🔍 You've stumbled upon a peculiar PDF filled with what seems like nothing more than garbled nonsense. But beware! Not everything is as it appears. Amidst the chaos lies a hidden treasure—an elusive flag waiting to be uncovered.
Find the PDF file here Hidden Confidential Document and uncover the flag within the metadata.

[https://challenge-files.picoctf.net/c_amiable_citadel/85ff7059d0bc98abe4aa040e2dd70a342f9f9aaa8edd70ccba166efeb6249afa/confidential.pdf] (PDF Download link)
## Solution
-----------------------------------

Opening the contents of "confidential.pdf" gives the following text.
```pdf
Title: The Ultimate Guide to Flag Hunting

Welcome to the challenge!

Don’t worry, this might look like gibberish, but maybe there’s something hidden somewhere? I
spent so much time creating this PDF with care... or maybe not!



Here’s a Quick Story:

Once upon a time, in a land far, far away, there was a secret... But where could it be? Hidden
deep within the document? Maybe the text holds clues?

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer posuere erat a ante venenatis
dapibus posuere velit aliquet. Aenean lacinia bibendum nulla sed consectetur. Fusce dapibus,
tellus ac cursus commodo, tortor mauris condimentum nibh, ut fermentum massa justo sit amet
risus. Curabitur blandit tempus porttitor. Lorem ipsum dolor sit amet, consectetur adipiscing elit.

You thought this was important? Nah, it’s just random text. Keep looking. Or maybe, just
maybe, you’re in the wrong place?



Special Hidden Section:

The author have done a great and good job

Don’t bother trying to reveal the hidden text, it’s just nonsense anyway. Even if you somehow
manage to do it, all you’ll get is:

No flag here. Nice try though!

If you're still reading this, I’ll tell you a secret: the answer might not be here after all...



Good luck! You’ll need it!
```

Not much help, let's dig into the first step in forensics, i,e.. looking into the metadata, we use a tool called **exiftool** for this purpose.

```bash
exiftool confidential.pdf
```

Output is as follows:
```
ExifTool Version Number         : 13.25
File Name                       : confidential.pdf
Directory                       : .
File Size                       : 183 kB
File Modification Date/Time     : 2025:11:07 19:17:38+00:00
File Access Date/Time           : 2026:01:06 15:03:20+00:00
File Inode Change Date/Time     : 2026:01:06 15:03:17+00:00
File Permissions                : -rw-rw-r--
File Type                       : PDF
File Type Extension             : pdf
MIME Type                       : application/pdf
PDF Version                     : 1.7
Linearized                      : No
Page Count                      : 1
Producer                        : PyPDF2
Author                          : <REDACTED BASE64 STRING>
```

In the *Author* field, we see an unusual base64 string, decoding that,

```bash
echo -n "<REDACTED BASE64 STRING>"| base64 -d
```

we get the flag
```
picoCTF{REDACTED_FLAG}
```

The purpose I am not revealing is that not because it is easy or hard, its because you should try it out, If you are here, you  are here to find the solution, I know this ain't a hard challenge, but someone who is just starting, it is.

Thankyou for reading this, Hope you enjoyed and learnt from it...
