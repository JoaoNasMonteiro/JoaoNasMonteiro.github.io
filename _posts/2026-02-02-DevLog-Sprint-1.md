---
layout: post
title:  "Devlog: Orange Sentry Sprint 2"
date:   2026-01-23 12:00:00 -0300
project: iot-honeypot
---

# DevLog: Sprint 2 - MQTT Client Implementation

## Introduction

This week's mission was to create a functional MQTT pub/sub client that can grab it's payloads to publish from FIFO pipes (`mkfifo`) and send them to the broker, as well as to subscribe to a topic and send the messages received through FIFO pipes to other processes to treat.

As you can maybe guess,this week's theme has been plumbing more than anything!

But I also learned more about build systems, why splitting functionality between multiple files is important and why writing utils is so.. useful..

My main blockers were my overall noviceness with the C programming language and trying to learn the Eclipse Paho C MQTT library.

So I dove deep into library docs, man pages and some AI code review to learn about industry best practices, in which the AI was surprisingly good and helpful, especially at providing guidance about best practices.

## The Engineering

As a summary, this week provided the mqtt-client implementation, complete with mqtt.h/.c auxiliary files. But also the logging.h util and the fifo-ipc.h/.c library.

The main challenge I faced was to hook everything up in a consistent and well structured manner, while providing adequate levels of abstraction through libraries and utils to avoid calling the vendor library directly from main.

I am planning to use the logging and fifo-ipc libs extensively through the rest of this project, both to keep things consistent and clear.

Speaking of pipes (FIFO is a very funny name btw), I chose to use this technology because I needed some sort of Inter Process Communication (IPC) to let my microservice-inspired architecture's processes talk to one another.

My options were basically Unix Sockets, Signals, Shared Memory, Direct Piping, and FIFO pipes.

Unix sockets looked promising at first, but they were kind of awkward to use (requiring handshakes), so I looked at the other options.

I eliminated signals basically right away, because They can't communicate text in a flexible manner.

Even ignoring the nightmare of memory management with Shared Memory, it was just a security disaster waiting to happen, so I dumped them right away.

Direct Piping just was not repeatable and robust enough.

So I looked at FIFO pipes, and they just fit so goldilockedly perfect. They were challenging but not impossible to implement right (they code basically the same as files), they can be easily debugged from the terminal by echoing to them, and they implement nicely with both C and Python (a technical requirement for this project). So I stuck with them.

They are not perfect, though, and need special treatment for concurrency reasons, and as with any other way of inputting data into a program, they need extensive input sanitization and treatment to avoid security and other kinds of bugs.

I ended up running out of time before I could actually finish implementing the subscribing part of the client, though from the docs it should not be that hard, since I've already done most of the groundwork to handle MQTT connections.

And since I did not have the adequate time to actually test out and debug stuff on ARM and on the board, as well as to clear some TODOS on the code, I will join those tasks into one micro sprint that should last less than a week, a week at most, and try to implement those things there. The plan is to have a really functional and kind of robust (I plan to have at least input sanitization and message formatting/parsing functionalities by the end) client to thoroughly test. If I want to add some more work on top of that I can maybe start to figure out some sort of system to test and benchmark my code on the board.

## The Next Steps

The next sprint, a micro one, is also related to MQTT. I plan to implement the subscribing functionality that I could not implement this week, as the complexity of the tasks was too big to handle. I plan to also do some general cleanup across the codebase and dedicate some time to validate and debug the functionality on the ARM board, As I have not been able to even boot up the board for time constraints

## Greatest Hits
1. Was trying to create a FIFO without checking if the file existed before. This is bad, especially because this program would be a long-running daemon that would probably restart many times lol

```C
//snippet of buggy code
int ipc_open_channel(IPC_Channel *channel, const char *path) {
  channel->path = path;
  if (mkfifo(path, 0666) == -1) {
    LOG_ERROR("Failed to create FIFO named pipe");
    return -1;
  }

  channel->fd = open(path, O_RDWR | O_NONBLOCK);
  if (channel->fd == -1) {
    LOG_ERROR("Failed to open FIFO named pipe");
    return -1;
  }

  return 0;
}
```

Error:
```bash
$ ./bin/x86/mqtt-client
[Error] Failed to create FIFO Named Pipe: File exists
<3>[MQTT_CLIENT] ERROR: Failed to open log FIFO at /tmp/test_fifo
```

Fixed code:
```C
int ipc_open_channel(IPC_Channel *channel, const char *path) {
  channel->path = path;
  if (mkfifo(path, 0666) == -1) {

    // throw error if there is an actual issue
    if (errno != EEXIST) {
      LOG_ERROR("Failed to create FIFO named pipe at %s", path);
      return -1;
    }

    // if it gets here then the file already existed, so we just update the
    chmod(path, 0666);
  }

  channel->fd = open(path, O_RDWR | O_NONBLOCK);
  if (channel->fd == -1) {
    LOG_ERROR("Failed to open FIFO named pipe at %s", path);
    return -1;
  }

  LOG_INFO("Channel opened successfully at %s", path);
  return 0;
}
```

2. I basically conflated payload with topic. The code was trying to change the topic based on FIFO pipe input rather than changing the payload.

## Conclusion

This was a very good and rewarding week for my first big milestone (yeah, I know it's only a sprint) of developing C code beyond just uni courses and quick Udemy projects. Feels nice to build something, and even nicer to ship it.

It may not be perfect, but it's mine and I leaned a lot along the way, and that is what matters.


