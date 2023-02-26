# checkpoint
Arquivos para instalação checkpoint no MacOS 64bits

Erros na instalação da VPN no MacOS após mudança na arquitetura para 64bits MACOS 12+

install: /usr/bin/snx: Read-only file system

Erro:
Something went wrong during installation of MAB Portal Agent
Solução:
https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk164117

Change the owner of the LaunchAgents file to the correct user with the following command: 
'# sudo chown "username" LaunchAgents'



Erro descrito no portal checkpoint:

https://supportcenter.checkpoint.com/supportcenter/portal

Ok, I find for months, call Apple Support and Checkpoint when help a solution to install extended SNX but no success.

So, I found a solution. 

Well, we have a 2 problems:

1 -  The installation is a 32bits Binary snx and not work in MacOS 12+

We found a install binary 64 bits in GitHub 

2 - The installation copy binaries to /usr/local/bin and javascript call extended in /usr/bin folder. Wen the javascript call extender, an error occurred:



My Mac is a 2020 with Ventura 13.2.1:



The solution to make this work is:

Reboot Mac in safe mode, disable SIP (authenticated-root too), reboot Mac normally, mount your root fs, copy binary and make a new snapshot.

Here is a workaround in my Mac With Ventura 13.2.1 :

1 - Boot in safe mode:

csrutil authenticated-root disable

csrutil disable

2 - Boot Normal:

Create a folder ex:

mkdir /tmp/mnt

Mount your root fs in /tmp/mnt:

mount -o nobrowse -t apfs /dev/dis

k1s1 /tmp/mnt/

Find the binary in /usr/local/bin (In my case, the installation copy binaries to /usr/local/bin but I need files in /usr/bin/)

cd /tmp/mnt/usr/bin/

copy yours binaries to folder:

cp -pr /usr/local/bin/snx .

3 - Now, the most important is make a notable snapshot(Note the root fs is mount "In my case" in /tmp/mnt):

bless --folder /tmp/mnt/System/Library/CoreServices --bootefi --create-snapshot

4 - Umount root fs and reboot your Mac.


Now, you can see the snx in /usr/bin like a image:



This is workaround executed:



Now, the javascript success call snx in /use/bin/snx:



Well done... VPN works !!





