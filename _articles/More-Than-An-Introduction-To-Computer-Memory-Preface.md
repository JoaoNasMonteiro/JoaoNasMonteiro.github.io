---
layout: article
title: "More Than an Introduction to Computer Memory: A Preface"
date: 2026-07-08 10:00:00 -0300
author: Joao Pedro Monteiro
---

Memory is arguably the most misunderstood concept in modern software engineering. We talk about it constantly — we leak it, we allocate it, we run out of it, we complain about how expensive it is - but very few times we actually stop to think more of it. As our tools become increasingly abstracted, fewer developers ever have to look under the hood to see how the bytes are actually being brushed.

Over the course of five chapters (or nine, depending on how you count it), we are going to explore computer memory from the bottom up. We will start with the physical wires and logic gates, climb up through the microprocessor and the operating system, and finally arrive at the application layer where our daily code lives.

## A preface

Ever since I've gotten into technology I've been in love with games. My second ever program, at around 15 years old, after the "hello' world", was a little space invaders clone made in python using pygame and some youtube tutorial that I've since forgotten. After that I've tried (unsuccessfully) to make games in platforms such as Unreal Engine and Godot.

I've always liked playing games as well, my godfather bought me an XBOX 360 for my 11th birthday, and I've put countless hours into skyrim (it even helped me to learn English!) and Rayman Origins, but as far as PC games went, I've had access to a computer very late in life, only at about 14 or so (and by that time most of my friends were into CS Go). It was a crappy HP laptop that struggled to run League at 30 FPS, so First Person Shooters were out of the question, and 3D games were heavily limited.

Because of that I got into the games my PC could actually run. After playing some platformers I got really into pokemon Nuzlocking videos, and that made me try emulating Pokemon games. I still remember to this day my team of 6 that beat Diantha: Delphox (my Ace), Goodra, Wiscash, Florges, Honedge, and mega Lucario (yes, I suffered a lot to Tyrantrum earthquake).

By now I've emulated many a pokemon game, but have always been extra fascinated by the gameboy games. How did the incredibly talented programmers of old manage to fit such fun and iconic experiences within 16KB of RAM escaped me for a long time. For context, opening notepad on an empty note in a standard Windows 11 installation circa June 2026 consumes ~30MB, which is about two thousand times bigger than the entire RAM of the gameboy.

From another perspective, there are about one million kilobytes in a gigabyte, so a standard computer of today with 16 gigs of RAM has about 1 million gameboys worth of RAM.

To accomplish such a feat the developers of yore had to have a deep and intuitive understanding of what they were working with: memory. What it was, how it worked, what made it tick and how to utilize every byte to it's greatest potential.

In the age of Vibe Coding® and The End of Software Engineering™ some would argue that these skills are but relics of a former era. I would disagree. Software engineering is essentially a cerebral skill, and knowledge is a thinking worker's biggest tool. Understanding the ins and outs of computer memory makes you closer to the machines you touch every day, and gives you the keen eye to avoid common traps that they throw at us.

Writing better software is about how well you can solve problems, and as the (two minutes) old adage (that I just made up) goes you can't solve problems you don't see, and you can't see problems you don't understand.

But even if you don't manage memory directly in your projects, or don't even code or work with computers at all, this type of knowledge is (in this humble bit brusher's opinion) deeply fascinating to learn about in it's own right.

In this series of blogs, we will take a look into computer memory: what it means, how it's made, what problems it solves, and how do the many parties in the computer world see and understand it, From the physical switches wrought of silicon, copper and gold, to the human behind the keyboard, wrought of anxiety and caffeine.

So let's dissect the metallic prefrontal cortex of our (sometimes) beloved machines, shall we?

### But before we start...

I am going to do my best to make this blog approachable to as wide of an audience as possible, but I am going to assume you already know some things.

I am going to assume that you are familiar with concepts such as:
    - What is a function, common types of function (linear, quadratic, exponential, sine, etc.)
    - The difference between current and voltage, basic electrical components (switches, capacitors, resistors)

I am also going to assume that you know your way around some basic computer code. If you can read the function below you should be good to go.

``` Python
def fibonacci(n):
    if n <= 0:
        return 0
    if n == 1:
        return 1

    a = 0
    b = 1

    print(a)
    print(b)

    for i in range(2, n + 1):
        temp = a + b
        a = b
        b = temp
        print(b)

    return b
```

Finally, I am also going to assume in every chapter that you have the knowledge of the previous parts. They are all written to be read individually, but they assume the prerequisite knowledge. Don't worry, there will be a clear indicator of what you need to know and pointers to additional resources.


