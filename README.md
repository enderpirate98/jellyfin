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

## Section 2: Setting up a Sambashare

Samba uses the smb Protocol which is an industry standard for shared network folders and we will use that to setup a Sambashare which will give us easy access to our media on our Jellyfin server.

Install Samba and enable it
```
sudo apt install samba samba-common-bin && sudo systemctl enable --now smb && sudo systemctl enable --now nmb
```
With Samba installed we need to create a config file for it so that it knows where everything will be and what permissions it has

Edit the config file
```
sudo nano /etc/samba/smb.conf
```
Copy and paste this config to the bottom of the config file (change to fit your needs of course)
```
[Sambashare]
   comment = Sambashare
   path = /sambashare
   writable = yes
   force create mode = 770
   force directory mode = 770
```

This will make the Sambashare be in the /sambashare directory and the 770 means that the owner of the directory and files as well as their group be able to read, write, and execute within that directory (if you want to change permisions use the chmod calculator [here](https://chmod-calculator.com))

Restart samba to apply the configuration that we just changed
```
sudo systemctl restart smb && sudo systemctl restart nmb
```
Add your user to the Sambashare
```
sudo smbpasswd -a youruserhere
```
Test that the config is solid using the following command and if there are no errors then you are good to go!
```
smbclient -L localhost -U %
```
On your client device go into your file manager and type into the title bar ``\\192.168.your.ip`` and it will prompt you for your username and password that you have set on your system

Now that you are in, you can manage your files and folders like you would on your client device but now you are managing your files and folders remotely from another device!

## Section 3: Linking the two together

Create a folder called media and this is where you will be storing everything that Jellyfin will access

Create a Movie folder and Shows Folder for your movies and shows

For our test media that we will be using in Jellyfin, we will download the movie ``Big Buck Bunny`` from [here](https://download.blender.org/peach/bigbuckbunny_movies/)

We will put it in the Movies folder under the name ``Big Buck Bunny (2008)`` because that is the way that Jellyfin does formating

Go into CasaOS and click on ``settings`` in the 3-dot menu, scroll down to the ``Volumes`` section and put ``/sambashare/media`` into the ``host`` section and ``/media`` into the ``Container`` section, after that is sorted out hit ``save``

Back in Jellyfin go to ``Administration``>``Dashboard``>``Libraries`` and add your 2 media types, ``/media/movies`` for Movies and ``/media/shows`` for shows

After you save go back to the home page and Big Buck Bunny should be there for you to watch and if it shows up the your config is correct!

In the future when you want to add shows just create a folder with the name of the show and then in the folder create different folders for the different seasons, for example ``S01`` for season 1 and ``S02`` for season 2, within those season folders name the video file ``S01E01.mp4`` if that is the first episode of the first season of the show

Congrats! You just basically made your own Netflix that you have total control over!

This guide was written by enderpirate98 under the GPLv3 but you can freely modify this guide and use it how you like as long as you link back to this page.
