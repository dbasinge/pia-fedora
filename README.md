(All credit goes to Middle ge Geek at techworlds.io, just uploaded to GitHub for easy access)

Private Internet Access VPN provider offers its client software for many platforms, but Linux. There is however a guide that describes how to configure PIA VPN servers only for Ubuntu. If you use Fedora then you may find this post useful (this was tested on a Fedora 23 Workstation).

Preparation

First of all you must be root before you can execute the following steps. To become root you just need to open up a terminal and type the following command:

$ su -
You will then be asked for the root’s password.

Your computer needs to be connected to the Internet. These steps also assume that certain software is already installed on your computer. Although this software is pretty standard in any modern Linux distribution it’s always better to be sure, open a terminal and execute the following command:

# dnf -y install wget unzip python NetworkManager-openvpn-gnome util-linux coreutils
NOTE: if you’re using an older version of Fedora or any other Red Hat based Linux distribution (e.g. CentOS, Scientific Linux or Oracle Linux) then you might need to replace dnf by yum and the package names might be slightly different.

Have your PIA VPN username handy (the one that starts with a “p” followed by some digits).

Configuring PIA VPN

I put together the following shell script to make this process easier. Copy all the following lines, paste them into your favorite text editor and save it as a pure text file (a.k.a. ASCII file).

+ expand source
Once you have saved this shell script into a text file, certify that you can execute it. Assuming that you’ve saved this script into a file called pia.sh then you can do that by running the following command on a terminal:

# chmod a+x pia.sh
Please note that by no means this shell script should be taken as a model. It was developed to be small and to run once. There are many things that should be improved to make this a production class script, like handling software dependencies, checking return codes and issuing proper error messages, making sure the PATH is correctly defined, cleaning up and validating any input from the user, verifying the integrity of objects downloaded from the Internet, trapping signals, adding
proper comments, etc.

Ok, now it’s time to run it! On a terminal, execute:

# ./pia.sh
When asked, enter your PIA VPN username. After a few seconds you should see “Done”. Yay!

Making SELinux happy

This configuration process has created new files on your system, therefore, in order to leave everything in order, you need to fix the security context of these files according to SELinux policy. To do that just run the following commands:

# restorecon -F /etc/openvpn/ca.crt
# restorecon -F /etc/NetworkManager/system-connections/PIA\ *
Validation

If you didn’t see any error messages then it’s time to check if all PIA VPN servers were indeed configured:

# ls /etc/NetworkManager/system-connections/PIA\ *
You should see dozens of servers/locations. Now restart NetworkManager and enjoy it!

# systemctl restart NetworkManager
Leave a comment below if you liked this article or, if you didn’t,  please tell me how I can make it better.
