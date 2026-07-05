+++
title="How to Setup Your Personal Home Server - For Scientists and Engineers"
date=2026-07-05
authors = ["Victor Steinborn"]

[taxonomies] 
tags=["HowTo", "Server"]
+++

> "Those who know nothing of foreign languages know nothing of their own."
> 
> -- **_Johann Wolfgang von Goethe_**

# Getting Computers to Communicate Simply

I was recently interested in setting up a home server so that I can 
[host my own large language models using Ramalama](@/posts/2026-02-12-ramalama.md) 
on an older computer with a dedicated graphics card.

However, when I started learning more about what it takes to set up such a server, 
I quickly realized a lot of the solutions out there relied on using 3rd party apps or 
software.

I personally prefer to keep my setup as simple as possible using system defaults 
and pre-installed software. Ultimately I would like the setup to feel like a computing cluster 
that can be found at a university for scientific and engineering applications with 
some websites that people can access from the browser (like one website for chatting with a local LLM).

Additionally, I also like to apply scientific experimental 
principles to my setup. Specifically, the most important principles for me are
**reproducibility** and **simplicity**. When running the server, I want my setup to be 
easy to replicate on another system. Additionally, I don't want to be dependent 
on an excessive amount of 3rd party software as that also impacts reproducibility 
and simplicity when the software I depend on requires an update that introduces 
breaking changes.

This article does not go into great detail on how to get everything running, but instead 
is intended to provide an outline with links to free resources that provide step-by-step 
instructions.

# Step 1 - Get a Server Running in Your Home Network

This was one of the steps that conceptually confused me the most when first starting out. 
Many of the guides on getting home servers running usually focus on one specific piece 
of hardware (e.g. a Rasberry Pi) and getting one specific piece of software running on it 
(e.g. Nextcloud).

What confused me at the time was the guides made it seem like a home 'server' is a very specific setup, 
however a server is quite a general concept. In short, paraphrasing [Wikipedia](https://en.wikipedia.org/wiki/Server_(computing)), 
_a server is simply a computer that can provide data, resources or services to other computers 
in a network_. This means almost _any_ computer can potentially be configured to act as a server. 

{{ character(name="pineapple", body="This may be obvious, however it is important to be aware 
that a server is ultimately just another computer for our purposes.") }}

For our setup, taking inspiration from computing clusters that may be found at universities, I 
ultimately decided to setup my server using [Fedora Server](https://fedoraproject.org/server/) after 
looking through what options exist from the popular Linux distribution providers.
You can also use other operating systems or solutions, however for my use case it was simplest to
go with Fedora Server.

To set up Fedora Server on my system I followed this excellent [video guide](https://www.youtube.com/watch?v=kOEcTGZWiUQ) from TechHut. This guide also explains how to get some simple services running 
using [Podman](https://podman.io/) (an alternative software to Docker), which comes pre-installed with Fedora Server.

Additionally as a side note, using Ramalama you can easily create Podman/Docker Compose files for serving LLMs on your server via the `--generate=compose` flag.

For example, you can create a compose file for running smollm using the following command 
from the Ramalama documentation:
```bash
ramalama serve --name=my-smollm-server --port 1234 --generate=compose smollm:135m
```

# Step 2 - Setting up an NGINX Reverse Proxy

At this point you might already be happy with your server. You would need to open up a 
port in your Server's firewall for each service you want to make available in your home network.
To avoid opening up more ports you can make use of an NGINX reverse proxy that would route traffic 
from a single port to different services.

For this I benefited from watching Cameron McKenzie's [video guide](https://www.youtube.com/watch?v=ZmH1L1QeNHk) on how to setup an NGINX reverse proxy. Many guides I have seen on this topic 
make use of 3rd party software that manage the reverse proxy for you, however this guide makes use of 
docker and uses the base NGINX image. To follow this guide with Podman you would simply need to 
replace `docker` with `podman` in each command.

One thing I would like to note is that I have seen the term 'reverse proxy' used in some guides to 
describe a setup where you want computers outside of your home network (i.e. computers not directly 
connected to your router) to access your server. The video does *not* setup your server for that 
type of configuration. Here we want the reverse proxy to route traffic coming _from_ within the 
home network _to_ a specific service running on our server (such as a site hosting an LLM). The 
service would not be accessible outside of our home network.

# Step 3 - Setup Self-Signed SSL Certificates

At this point you have a server that has services accessible through a single port, however you might
have noticed that if you try to access these services you have a warning in your browser that the 
connection is not secure. If you ignore the warning you can access your service. 

However, the only issue with this approach is that anyone/ any device in your home network can read 
anything you send over this connection because you are sending data over the HTTP protocol. To fix 
this and use the more secure HTTPS protocol (the S stands for 'secure') you can use self-signed 
SSL certificates. There are also other ways to tackle this issue, however this worked for my use case
and it did not require me to install extra software or pay for a domain name.

For learning about the theory behind SSL certificates and for a guide on getting HTTPS setup between devices in the home network, I greatly benefited from Christian Lempa's [video guide](https://www.youtube.com/watch?v=VH4gXcvkmOY).

# Closing Thoughts

Setting up my own home server was a rewarding experience for getting practical experience in computer 
networking and deploying services. Additionally, I found setting up a home server was a great way 
to take steps to save money by opting for self-hosted services (as opposed to cloud solutions) and 
taking control of personal data.

There are many ways you can setup your server so I hope that you benefited from this guide by helping 
you decide what setup is best for your use case. 

```bibtex
@misc{2026-vsteinborn-serverhowto,
    author = {Victor Steinborn},
    title = {How to Setup Your Personal Home Server - For Scientists and Engineers},
    year = {2026},
    url = {https://vsteinborn.github.io/posts/personal-server/}
}
`
