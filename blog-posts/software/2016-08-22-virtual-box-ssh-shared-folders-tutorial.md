# VirtualBox Shared Folders + SSH from OSX Host to Ubuntu Server Guest Tutorial

[VirtualBox is a powerful virtualization product that allows an unmodified operating system with all of its installed software to run in a special environment, on top of your existing operating system](https://www.virtualbox.org/wiki/Virtualization). Since I became a full time independent software developer, I have frequently found myself working on very different software environments. VirtualBox has helped me to setup an environment for each of my clients and easily manage them.

In this tutorial, I will show you how to setup a virtual machine instance with shared folders and SSH from your host to the guest. This tutorial assumes you have already setup an Ubuntu server virtual machine.

### SSH from Host into Guest Machine Setup:
SSH is a protocol used to securely log onto remote systems. It is the most frequent way to access remote Unix-like servers.

#### Instructions:
  - Start the VM
  - Go to ``Settings > Network > Advanced > Port Forwarding``
  - Click in the ``Add new port forwarding rule`` green button in the top right of the window
  - Rule example setup:

```
| Name | Protocol | Host IP | Host Port | Guest IP | Guest Port |
|------|----------|---------|-----------|----------|------------|
|  SSH |    TCP   |         |    2281   |          |     22     |
```

  - Click ``Ok`` to add rule
  - Install SSH server on guest

```
sudo apt-get update
sudo apt-get install openssh-server
```

  - And that's it! Now type ``ssh username@localhost -p 2281`` to SSH from your host machine to the guest

### Shared Folders Setup:
Shared folders provide an easy way to exchange files between the host and the guest machine. This means you can access and modify your local files with your favorite tools and still run the server and its dependencies on your virtual machine.

#### Instructions:
  - Install VirtualBox Guest Additions:
    - Locate the VirtualBox Guest Additions at ``/Applications/VirtualBox.app/Contents/MacOS/VBoxGuestAdditions.iso``
    - Copy it to a location that is accessible when browsing for files
    - Start the VM
    - Click the CD icon in the bottom right task bar
    - Select ``Choose disk image...`` and search for the ``VBoxGuestAdditions.iso``
    - In the guest terminal type (you can also do this from the host terminal if you SSH into it):

```
  sudo su
  apt update
  apt upgrade
  apt-get install dkms build-essential linux-headers-generic gcc make
  mount /dev/cdrom /mnt
  cd /mnt
  sh ./VBoxLinuxAdditions.run
  reboot
```

  - Setting up the shared folder:
    - Go to ``Settings > Shared Folders``
    - Click in the ``Add new port forwarding rule``   green button in the top right of the window:
      - Search and select the folder you would like to share
      - Select the ``Auto-mount`` and ``Make Permanent`` options
    - Start the VM
    - Add user to the ``vboxsf`` group by running ``sudo adduser username vboxsf`` and reboot the VM
    - ``cd`` into the ``/etc`` directory and open the file ``rc.local`` with sudoer access
    -  Type the following command right above ``exit 0``:

```
# 'folder_name' = given in the shared folders configuration
# 'path/to/shared/folders' = guest path to access the shared folders from
sudo mount -t vboxsf -o uid=1000,gid=1000 shared_folder_name path/to/shared/folders
```

  - And that's it! You should now be able to see your shared folders at ``path/to/shared/folders`` in the guest

### Conclusion
I hope you found this tutorial easy to follow and useful. I have also been reading a lot about Vagrant, as it seems to be another popular choice by developers. I plan on giving it a try in the following months, and will let you know my opinion about it. If you have any questions or suggestions, feel free to leave a comment below.
