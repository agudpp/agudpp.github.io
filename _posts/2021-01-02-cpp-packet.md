---
title: "cpp packet lib"
categories:
  - cpp
tags:
  - cpp
  - packet
  - header-only
  - packing
  - unpacking
---


# Packet lib

This is a very short blog post since its a small `c++` `header-only` lib I created, hence the full documentation would be in the [github repo](https://github.com/agudpp/packet).
The `packet` library is intended to provide an easy "API" for packing and unpacking data (messages) so they can, for example, be safely sent over the wire (TCP). Whenever you need to send information over the wire is important to know:

- They are respecting some kind of protocol (otherwise you may want to close the connection).
- What you are getting is complete (need to know when the message start / end).

TCP ensures you that the data will be sent in order, but doesn't ensure that the calls are atomic (reads / writes). Hence, whatever you the data you are sending, needs to be "packed" / "unpacked". For example, `protocol buffers` is a library that serializes messages, but doesn't handle where the message start / end.

## How it works

The packets have the following shape:
`[ head_pattern | pkt_content_len | content | tail_pattern ]`

- head_pattern: (optional) a user defined pattern to detect early wrong or invalid messages over the wire
- pkt_content_len: field indicating the size of the content buffer
- content: the data / content itself
- tail_pattern: (optional) ensure that the packet size was correct and respects the protocol.

The intention of the `packet` library, is to provide an API that let the user read as few characters as needed while checking the validity of the packet while reading it.

