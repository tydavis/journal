# Life in Chrome

>Published on 2017-03-07

My day-to-day computer activities are mostly through my work-provided laptop.
Installing custom compilers or other untested software is expected for my role,
and I've been granted administrative rights to my laptop, but we also have
corporate antivirus and collective host configuration management. After a chat
with the security team, I reminded myself that the security of our endpoints is
more important, and deserves my attention.

Being firmly in the middle between Dev and Ops, I have a VM provisioned for my
work requirements and it does the job. I don't actually need my local host's
terminal to get my job done.

As an experiment, I created a cheap machine in [Google Cloud Platform][1] and
set it up with my non-work environment, then wiped-and-restored my work laptop,
only installing Chrome (and one piece of videoconference software we use) to my
user's Applications folder.

Magic! Chrome can SSH to other hosts [via an extension][2], we use Google Apps
for Office purposes, and my VM at work took care of the rest. There's even a
[chrome extension][3] that makes the GCP SSH window work better. Best of all,
any "personal projects" I work on are isolated and run little-to-no risk of
infecting my work environment and vice versa.

Now if I ever need to wipe the laptop or have it replaced, I can be back up and
running in a matter of minutes. Given our upcoming security compliance audits, I
can work within every security change because my host does nothing but act as a
dumb terminal.

~~If it weren't for my need to have a self-contained laptop at home when the
internet needs repair, I could~~ [I can][4] get away with working exclusively on
a Chromebook, even when I break my router/access point.

[1]:https://cloud.google.com/compute/docs/
[2]:https://chrome.google.com/webstore/detail/secure-shell/pnhechapfaindjhompbnflcldabbghjo
[3]:https://chrome.google.com/webstore/detail/ssh-for-google-cloud-plat/ojilllmhjhibplnppnamldakhpmdnibd
[4]:https://www.amazon.com/dp/B011DDXGVC/