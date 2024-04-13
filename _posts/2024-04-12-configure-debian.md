---
layout: post
title: My Desktop Environment
date: 2024-04-12 14:11:21 +0800
categories: style
---

My definition to a desktop environment is software plus configuring plus accessing.
Software and configuring is easy to understand, but what is accessing?
Here is an example: there are a lot of vi-like editors in Debian Archives, but if you
installed several such editors at the same time, Debian usually keep the name vi
for the last one you installed. The functionalities provided by others still exist
in your system, but the accessing to these functionalites through command vi
is variant so that it should be regarded as a seperated component of a working system.
This blog shares my way of extending classic accessing management and some considerations
for building a desktop environment.

# Platform

A desktop environment runs on a hardware platform. Its behaviour is influenced by its
underlying hardware, but the desktops built by a single user
on different hardwares usually share some common settings. So it's inefficient
to maintain several desktops without a way to manage the common part and the variant part.

I do this job by using m4 preprocessor for files shared by several desktops and save the
files dedicated to a particular desktop on a seperated directory.

In fact, operating system is also a factor here, but I choose to limit the choice
of operating system to Debian, so I can eliminate this factor.

# Yet another layer of indirection

People of computer science always struggle to build a consistent mapping from names to concepts.
They don't want to say _build_ here, _make_ there. They are also the ones who are eager
to invent new names. It's peaceful that if they all live in their own little island and don't disturb
each other, but, saddly, they all have to understand what others say for the whole career.
The ultimate tragedy is that I'm ruled by these mess.

I usually try my best to not to insert yet another layer of indirection, but consider the hours that
I've spent and going to spend on the computers, it might be good to maintain my mental consistency on desktop environment.

The indirection is done by using a simple mechanism of bash: function ``command_not_found_handle``.

I still use the traditional accessing management, which is done by aliasing, linking, and environment variable ``PATH``,
and regard the names introduced in this way as common sense shared with community. Beside it,
I selected a set names which is consistent in mapping to higher abstract concepts.
