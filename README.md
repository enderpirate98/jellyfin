## Sections
- Introduction
- Installing Jellyfin
- Setting up a Sambashare
- Linking the two
- Using Jellyfin
## Section 0: Introduction

Tired of Big Tech always letting you down without facing any consequences? It's time to take back control! This guide will show you how to set up Jellyfin, an open-source alternative to Netflix that you can host yourself! We’ll be using Debian 12 and CasaOS, which we set up in our previous guide [here](https://github.com/enderpirate98/deb-server). Get ready to create your own personal streaming service!

We will install Jellyfin, link it with a Sambashare for easy access, and demonstrate it working with a public Domain Movie.

## Section 1: Installing Jellyfin

In CasaOS go into the appstore and install Jellyfin

After a minute the Jellyfin Icon should appear on the home screen, click on it and go through the initial setup.

Select your Language, choose a username and password, when it asks for a media Library just click next (we will ge to that later), select your metadata info, let the remote access stuff be default.

Now click done and login to your admin account, click on the Hamburger Menu and go into ``Administration > Dashboard > Users`` and create a non-Admin user for watching your future content (password is optional here for convienience but it's up to you)

Go to ``Playback`` and change the Hardware acceleration from ``None `` to the intel one if your cpu/gpu is intel and vice versa for AMD, hit ``save`` at the bottem, if you have Nvidia then you will have to research what to do on your own because Nvidia is such a nightmare on Linux that I want to explode when I mess with it.

More to come
