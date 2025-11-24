---
title: "We Go Gym"
author: "Salochi"
layout: "single"
github: "https://pointerpointer.com/"
---

## Walkthrough 

Within this pcap file there are a series of curl requests. 
Each of these curl requests returns some data, though this data is just random gibberish. However, you will notice there are 22 curl requests, this is the length of the flag. 
Each curl request has a mangled TTL value which is a decimal encoded character of the flag. These packets are in order, so once you notice this pattern you can extract all the TTL bytes and convert them from hex to recover the flag. 

The challenge name is meant to be a hint as CURL and gym go hand in hand, i.e. Bicep Curl exercises.


[The PCAP file](/files/capture.pcap)