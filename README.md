Overview
========
It does not add extra newlines.

It tries to check  for unmatched [ ]  { }  ""  ` `.  These errors are
written to a status file.

It does not recognise keywords (I know there aren't any! )

{  }   and   {:   :}  are treated the same, no extra indenting is done.
this may change.

Every [ and ]  is counted for indenting, but they only actually cause
indenting when at the start of a line, so:
```red[
[xx]  ]
```
becomes
```red
[
  [xx]   ]
```
and
```red
[
[[xxxx
]
]
]
```
becomes
```red
[
   [[xxxx
       ]
   ]
]
```

Program Design
==============
It could possibly be made much shorter if I used the 'code-as-data'
approach, getting the type of each item, but I wanted it to work with
incorrect code (i.e. report on possible errors).

The reason for the 2 output files, is to allow for a variety of uses - e.g.
as part of an IDE, or as a command-line tool.  An empty status file can be
detected, and the programmer can decide whether to update the original file
or not.  I'm a bit wary about trusting it yet, as I have not used it much.

It does not use recursion to handle `[nested]` blocks - to make it easy to
spot incorrect nesting.

The style is loosely based on Michael Jackson's JSP method.  Each section of
program assumes that the first character of the item it will process is in
the 'ch' variable.  After a section does its work, it does a 'read-ahead' to
prepare for the next section.

The reason for processing the comments and strings is because they might
contain quotes, brackets, etc.

Usage
=====

Example:
```bash
arturo artpp.art my-art-prog.art
```
(i.e. 1 command-line arg)

2 output files are always produced:

* artpp-result.art       (the reformatted code)
* artpp-status.txt:
   sometimes, code errors (e.g. wrongly-nested [...]) mean that
   formatting cannot be done properly.  Artpp checks for errors
   in "..."  {...} [...] (...) `x' (single characters).
   If the status file is empty, no errors were found, and the created .art
   file should??  be OK. If the status file is not empty, it contains
   error details, and the art output should not be used.  However,
   it CAN be useful for finding [...] errors - you can see where the
   nesting went wrong.

License
=======

MIT License
Copyright (c) 2021 Mike Parr

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
