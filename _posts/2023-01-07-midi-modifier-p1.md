---
layout: post
title: MIDI Modifier - Part 1
category : hardware
tagline: "Idea, theory, what's next"
tags : [midi, arduino, esp8266]
---
{% include JB/setup %}

## Idea

![Idea diagram](/assets/images/midi-modifier-top-level-diagram.svg)

Basic idea is to build a device that allows user to send MIDI message in relation to MIDI input message with modification. For example, if we receive `noteOn` message on MIDI channel 0, we want to transpose third octave to MIDI channel 1; or really we want to be able to make any possible change, or do any possible operation that takes any MIDI message as input, and outputs new message based on our rules.

## Why

First of all, to have some sort of devkit/playground for MIDI signal processing in-a-box. Second, because I actually need one for other project, to correctly drive Korg Volca Sample v1 using single MIDI channel with working Control Change (CC) for each channel.

## Hardware of choice

I want to use Wemos D1 mini with ESP8266 on board as my MCU.

- Cheap
- Fast
- Easy to find tutorials for
- Simple to flash
- Integrated 5V over micro-USB and 3.3V voltage regulator
- Arduino (IDE) compatible
- Two hardware UART ports (of which one only transmits signal, perfect for MIDI output)
- WiFi for future integration (such as remote MIDI play maybe?)

## MIDI

MIDI messages are sent on top of [UART (wikipedia)](https://en.wikipedia.org/wiki/Universal_asynchronous_receiver-transmitter). See [MIDI Protocol Guide (hinton-instruments.co.uk)](http://hinton-instruments.co.uk/reference/midi/protocol/index.htm).

For reference on receiver side, see [Korg Volca Sample MIDI Implementation Chart (PDF)](https://cdn.korg.com/us/support/download/files/9bdc4997e44b7e96e771f612e06a90e5.pdf)

## User Interface

![User interface idea](/assets/images/midi-modifier-user-interface-idea.svg)

Here's an idea on how I see the device. Case would be 3D printed at first, although I have some ideas on PCB-based front panel, and Korg Volca -like design.

As for software side of things, I want to make it `iptables`-like. If we turn on the device for the first time, our "rules" table will be empty, and any input MIDI message will be sent to MIDI output. If we add new rules, any new input MIDI message will be compared against our rule; if it fits, we change it or create new message based on our rule and push it to output; else, we simply forward our input message.

## .?!

I want to make my hardware and software easily hackable for people to create new things, hence I want to make all software for projects like this in dedicated IDE's. I'll have some fun with raw SDK's **only** if I'll need them (for example for software-hardware optimizations), or when I'm done with simple to understand, compile and flash firmware.

