---
layout: page
title: Guidelines for submitting a bug
---

### Introduction

The RootTalk [mailing list](https://root.cern.ch/roottalk-digest) and
[forum](https://root-forum.cern.ch) can  be used to pass information of general
interest to the ROOT user community or to submit problems that you cannot solve
with the information provided in the
[Documentation](https://root.cern.ch/documentation). Several thousands users read
the list and forum and my provide you with solutions to your problem.

**Only if you are convinced that your problem is a real bug**  of the ROOT
software then follow the instructions on
[how to report a bug with JIRA](https://root.cern.ch/node/3608).

### When you submit a bug, please follow these guidelines:

 1. If you submit a problem, always indicate your **ROOT version number**
    (e.g. 6.02.00, 5.34.22) and the **platform** you are running on
    (e.g. Ubuntu 14 gcc49, SLC6 clang35).
 2. The bug title subject line should clearly identify the problem.
 3. If you use a non recent version of ROOT, try to test your problem with newer
    version. If this is too difficult, consult the release and development notes
    to check if your problem has not already been solved.
 4. Try to quickly search the internet and look in the forum if your problem has
    been already solved.
 5. Try to share the maximum amount of information. A bug report which only says
    "My macro crashes when running on kubuntu but on windows works" is much less
    likely to be solved quickly than a complete message with a gdb stacktrace and
    a reproducer (see next item).
 6. Send a minimal program or macro reproducing the problem. Make sure that your
    script can be executed directly. If it uses a data file, send the smallest
    possible data file as a mail attachment, or indicate how to get this file.
 7. If for some reason it is not possible to share a minimal reproducer, try to
    make sure that your explanations are sufficient to speed up the whole fixing
    procedure.