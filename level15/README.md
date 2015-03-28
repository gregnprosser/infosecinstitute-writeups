# Level 15

## Description

A plain page with a single form field titled by "DNS LOOKUP".

If you enter any text, it appears to run `dig <text>`.

## Solution

When faced with a page like this one, where your output appears to be fed to a *ix command line, it's possible that if the application is written incorrectly it might be vulnerable to [Command Injection](https://www.owasp.org/index.php/Command_Injection).  This allows arbitrary commands to be run on the host.

Using simply shell techniques we appear to be able to execute arbitrary commands.  For example, typing `foo >/dev/null && echo vulnerable` prints the text `vulnerable` into the page.

From this point, anything is possible.  If we run `foo >/dev/null && ls -al`. we get the output:

```text
total 16
drwxr-xr-x 2 root   root   4096 Mar 13 13:47 .
drwxr-xr-x 9 root   root   4096 Mar 13 04:50 ..
-rwxr-xr-x 1 ubuntu ubuntu   37 Mar 13 18:49 .hey
-rwxr-xr-x 1 root   root   3815 Mar 13 18:49 index.php
```

In *ix, files can be marked as hidden files and not seen in typical directory listings by starting their filename with a `.`.  The file `.hey` certainly looks suspicious.

Sure enough, if we use `foo >/dev/null && cat .hey` we see:

```text
Miux+mT6Kkcx+IhyMjTFnxT6KjAa+i6ZLibC
```

This doesn't seem to match the format of previous flags, so I'm assuming that this is not the flag for this level.

Because the character set contains only alphabetic numbers and the symbol '+', this looks a lot like a [Base64](http://en.wikipedia.org/wiki/Base64) encoded string, however trying to decode this results in gibberish.

```python
>>> import base64
>>> repr(base64.b64decode('Miux+mT6Kkcx+IhyMjTFnxT6KjAa+i6ZLibC'))
"'2+\\xb1\\xfad\\xfa*G1\\xf8\\x88r24\\xc5\\x9f\\x14\\xfa*0\\x1a\\xfa.\\x99.&\\xc2'"
```

The next segment of this write-up was done in an IPython notebook, so follow [this link](http://nbviewer.ipython.org/github/gregnprosser/infosecinstitute-writeups/blob/master/level15/Mock-Base64.ipynb).

## Flag

`Miux+mT6Kkcx+IhyMjTFnxT6KjAa+i6ZLibC`
