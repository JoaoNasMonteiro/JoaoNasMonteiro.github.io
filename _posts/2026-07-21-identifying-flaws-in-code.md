---
layout: post
title: "DevLog: finding flaws code"
date: 2026-07-21 10:00:00 -0300
project: chip-8
---

>!static analysis as good for saying "this is dangerous", but not good fot saying "this is safe", basically they have very high precision (low rate of false positves, most of the code it flags is actually bad), but very low recall (high rate of false negatives - low sensitivity -, out of all the bad code it comes across it only flags a small percentage of it, it misses a lot)

