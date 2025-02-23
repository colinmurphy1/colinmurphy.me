+++
title = "About this website"
description = "About me and this website"
date = 2024-07-15
[extra]
  hidden = true # hide from the home page
+++

Hello and welcome to my website! This website serves as a personal blog where
I write about my personal interests and other things I may find others to be
useful.

## How this site is built

This website is built using the [Zola] static site
generator, and hosted using Cloudflare Pages. Content is written in Markdown
using Visual Studio Code. The source code for this website is available on
[GitHub][gh-source] if you would like to take a look!

## Cool stuff

### The Home Server

{{ figure(
  src="/img/server.jpg",
  alt="A picture of my home server",
  caption="An older photo of the home server; there have been some hardware
    changes since this photo was taken."
) }}

Specs:

* Intel Xeon E5-2618L 10-Core CPU
* 256GB DDR4 ECC RAM
* Supermicro X10SRi-F motherboard
* 500GB Samsung 970 EVO boot disk
* 4x WD HC310 hard drives on a Dell PERC H310 (for TrueNAS Scale)
* Nvidia Quadro P1000 GPU (for Plex transcoding)

### Meshtastic

One of my hobbies as of late is [Meshtastic], an off-grid communications
platform. I run a few nodes and am a member of the [Iowa Mesh][iamesh] network.

[gh-source]:https://github.com/colinmurphy1/colinmurphy.me
[zola]:https://www.getzola.org
[meshtastic]:https://meshtastic.org
[iamesh]:https://iowamesh.net
