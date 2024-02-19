---
layout: post
title:  "Reasons of choosing Debian"
date:   2024-02-18 12:10:03 +0800
categories: style
---

In the past years, I've tried several Linux distributions. Starting from Ubuntu, to Arch Linux,
to Slackware, at last I settle down on Debian. Despite all distributions has its own advantages,
Debian might be the best choice for me.
This blog explains the reason and gives a comparison among them.
The comparison focuses on three aspects, policy, usability and packaging.

## Policy

There is a set of predefined settings or customs in all software about how to use them.
The set is particularly large and complex in using an operating system, some of
them come in together with the system, some of them are established by users.
For example, "where to place executable files",
"which desktop environment to use", "how to name a set of files that have some features in common",
just to list a few.

A user's policies will always confict with the ones given by system in some degree. So to choose
a distribution with minimal conflicts is important for the user's mental health. To fix
the conficts with underlying operating system is not only cumbersome but also unpleasant.

Slackware claims itself as the most Unix-like Linux distribution. It's right and I really like it.
Few of policies is imposed on users, so users must build their own
set of policies to efficiently work on it. Slackware itself is just a collection of well cooperating programs
and a minimal configuration framework.

Debian has a little more constraints compared to Slackware.  Instead of regarding programs as
equivalent executable files, it organizes and classfies them, which might be the reason it
replaced classic System-V init script with systemd.
Arch Linux is similar to Debian in a lot of ways, with fewer pre-settings.

The policies of Ubuntu is basically a superset of Debian, or if you don't agree with this statement,
even worse in this case, built on Debian but having a lot of conflicts with it.
Nevertheless, Ubuntu is good for people who agree with Ubuntu's choosing or don't care carrying out
policies themselves.

The point here is to balance the effort that you must do to fix the "wrong" default behaviours
and the effort that you build your own policies. For me, all except Ubuntu is at good point.

## Usability

The assessment of usability is based on two criterias, how many programs it ships, and how many
devices it runs on. I will just give my opionion.
First tier, Ubuntu and Debian. Second tier, Arch Linux. Last, Slackware.

It's possible to use Slackware in any situation and run any program on it, but too hard to.

I have an ongoing embedded system project, and if possible, I'd rather choose a distribution
that I can use both as desktop and in production environment. Debian wins here.

## Packaging

This is a big topic, maybe next time.

