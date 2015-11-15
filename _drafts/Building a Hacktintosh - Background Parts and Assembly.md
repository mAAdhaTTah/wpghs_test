# Background

Before we begin with the actual building of the computer, there are some aspects of how I got here that will affect how I why I took some of the steps I did. As I explained in my previous piece, I owned a Macbook Pro for 4 years or so. Eventually, the logic board gave out, and the repair guys at Tekserve told me it would have to be replaced, costing me upwards of $1,000. It would make more sense to just buy a new computer.

At the time I purchased my MBP, my dad got his own MBP with the same specs which he barely used - many of the programs he required were Windows-only. I opened up my laptop and placed the hard drive in an enclosure. Using SuperDuper!, I wiped the internal drive on his laptop and copied my data in off the external drive. One of the great things about OS X is you can boot from an external drive, which Windows can't, which made this whole process super easy.

Having gotten the bug for tinkering with parts, I bought a 750GB laptop drive, opened the computer back up again, and put the new drive in. This drive was partitioned and Bootcamped to install Windows 7.

Eventually, this computer also died - I left it unplugged overnight and it wouldn't boot the next morning (part of me thinks the computer might still have been salvageable, but all my googling and tinkering skills couldn't fix it). I then pulled the drive out of the laptop and put it into a desktop hard drive enclosure, which I put into a 2006 Mac Pro I used to use for recording.

The OS X install (which was on Snow Leopard at the time) would boot without a problem, but the Windows 7 install was 64-bit, and the old Mac Pro wouldn't support that install. I was able to get access to the partition through VirtualBox, cobbled together [with](https://kindlevsmac.wordpress.com/2011/10/14/how-to-run-windows-7-bootcamp-in-virtualbox/) [these](http://wouter.kloos.me/2012/01/running-windows-7-from-the-bootcamp-partition-in-virtualbox/) [four](http://phaq.phunsites.net/2011/03/05/sharing-windows-7-between-boot-camp-and-virtualbox/) [guides](http://luckyviplav.blogspot.com/2011/06/windows-7-on-mac-os-x-through-virtual.html). Disk2VHD, provided by Microsoft, allows you to copy a disk drive to a virtual drive, and VBox allows you to copy the .vhd file to the OS X partition through a shared folder. This virtual disk can be mounted on VBox, and I was able to access my data again.

So I now had a .vhd file with my Windows boot. This will come into play when dual-booting Windows.

# Parts

Now, picking parts: I basically followed [Tonymacx86's guide](http://tonymacx86.blogspot.com/2012/08/building-customac-buyers-guide-2012.html). The parts he recommends makes it really easy to get everything up and running as quickly as possible. Here's what I selected:

![](http://jamesdigioia.com/app/uploads/2015/11/processor.jpeg)

First things first: the processor. There are a couple things which dictate how fast the computer runs, and this is high up in the list. For my use case, the computer will be dual-booted, with the OS X side being used for documents, social media, and other (primarily productivity) tasks. While this experience should be smooth, its the gaming side in Windows 7 that really dictated how powerful the computer should be. i5 at a minimum made the most sense, and given my experience with the processor in my Macbook Pro, getting at least quad-core and 3.1 GHz would definitely be optimal. I went with 3.4 GHz, opting to "future-proof" the build a bit.

![](http://jamesdigioia.com/app/uploads/2015/11/ram.jpeg)

I went with 16GB of RAM for the same reason as the faster processor; so far, the computer has never used more than 6GBs at a time, future programs may change that, although who knows how long that will take. Definitely get at least 8GBs, but for the extra $20, I splurged for the 16GBs.

![](http://jamesdigioia.com/app/uploads/2015/11/video-card.jpeg)

At first, I was really unsure what video card to pick here. It made more sense to purchase either a high-end card or a low-end; the trade-off for the mid-range cards didn't really make sense for me, in keeping with the idea of either "future-proofing" the computer or just going with the cheapest option and upgrading later. [The GT 640 works out of the box](http://www.tonymacx86.com/graphics/64826-gigabyte-gt-640-working-oob.html), although it didn't work perfectly for me, but it makes for an easier install for a first-time builder. Before you purchase everything, make sure you have a DVI cable, not a VGA, or you won't get the full resolution possible with the video card; VGA isn't fully supported by OS X.

![](http://jamesdigioia.com/app/uploads/2015/11/motherboard.jpeg)

In all honesty, I didn't (and still don't) know what the difference between motherboards are. Tonymacx86 recommends going with a Gigabyte motherboard because they are the easiest to configure with OS X and do not require a [DSDT](http://www.tonymacx86.com/general-help/3280-basics-about-dsdt-video.html). USB 3.0 support is still inactive as of this writing, so perhaps looking for a motherboard with more USB 2.0 ports than 3.0 ports may be better, but either way, just make sure you get a Gigabyte mobo.

![](http://jamesdigioia.com/app/uploads/2015/11/hard-drive.jpeg)
![](http://jamesdigioia.com/app/uploads/2015/11/power-supply.jpeg)

This is the power supply and hard drive. The solid state drives are really expensive for the amount of space they provide, so a regular old HDD is what you see here. This was before Apple announced their new FusionDrive, and I'll be looking into using that technology with two drives (one SSD, and one HDD) in the future. The power supply is the recommended one - not much more to say about that. Addtionally, I purchased a CD drive that I didn't take a picture of. You may laugh and say "CD drive? I don't need no stinkin' CD drive!" but you do if you want to install Windows or Snow Leopard.

# Computer Assembly

The first thing I'll say is that I didn't read instruction manuals before installing anything, and the second thing I'll is that is a _ **terrible** _ idea. For the most part, parts and plugs fit into the spots they're supposed to, so DO NOT FORCE ANYTHING. The video card and the RAM do take some pushing to get them into their slots, but for the most part, things fit where they're supposed to.

## Installing the CPU

1. There are two parts in the CPU box - the cooling fan and the processor itself. Be careful removing the processor from the box, as the pins on the bottom (which are small) are somewhat fragile and will violate your warranty if they're broken, leaving you with a very expensive, very useless piece of technology.
2. Take the motherboard out of the box and find the spot where CPU will sit. Pull back the lever that holds the plastic cover down and take it off.
3. BE VERY CAREFUL HERE. Seat the processor in the slot - you'll find there are two notches on the processor that sit into two notches on the slot. Make sure they line up when placing the processor.
4. Push the lever down and reseat it so it locks into place.
5. Place the cooling fan on top of processor, lining up the pegs on the fan with the holes in the motherboard. Push down to snap the pegs into place. Note that you can rotate the cooling fan anyway you want, so check to see that the power cable and plug line up so you don't have a lot of extra cable floating around
6. Plug the power supply for the cooling fan into the motherboard. The power supply cable pretty much just fits where it goes, so that's easy enough - just be aware that you want to make sure the cable doesn't get whacked by the fan as it spins, or you'll end up with an annoying buzzing noise and eventually it could cut through the cable.

## Mounting the Motherboard, Power Supply, and Other Parts

1. Open up the case - there are a lot of dangling little cables coming from the top-front of the case that will likely get in your way. Just don't damage them. Additionally, there are a bunch of screws and zipties, as well as sheet of paper explaining where all the screws go. You'll need this, but won't need most of the screws unless you end up expanding the components of your computer.
2. Pull off both sides of the case - two fat screws on each one will do the trick. Lay the case down on "shorter" side - the one with less open space.
3. On the bigger side, you'll see a little nub that you can place the motherboard onto, through the little hole in the middle of it. There are then four screws which you have to line up and screw on. These are a pain, but just keep fiddling, you'll get it eventually.
4. The power supply will go on the bottom of the case, pushed into the far corner. It'll fit pretty snug there, and then just screw it in.
5. There are four hard drive bays in this case. Squeeze the tabs together and pull one out and mount of the drive in it before sliding it back in. There are a few screws for that - it's pretty easy.
6. For the CD drive, the protective cover comes off the inside with tabs, similar to the hard drive bays, but you can slide the drive in the front, which makes it easier if you've already mounted everything
