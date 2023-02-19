Source: https://developer.hashicorp.com/vagrant/tutorials/getting-started/getting-started-project-setup

Install Vagrant: https://developer.hashicorp.com/vagrant/docs/installation

Create directory for new project

```
/c/Users/michi/OneDrive - Hogeschool Gent/Semester 2/SEL/Vagrant
```

Initialize the project

```
michi@LAPTOP-AU8VQ3TK MINGW64 ~/OneDrive - Hogeschool Gent/Semester 2/SEL/Vagrant/SEL
$ vagrant init almalinux/9
A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.


```

Start up AlmaLinux

```
michi@LAPTOP-AU8VQ3TK MINGW64 ~/OneDrive - Hogeschool Gent/Semester 2/SEL/Vagrant/SEL
$ vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Box 'almalinux/9' could not be found. Attempting to find and install...
    default: Box Provider: virtualbox
    default: Box Version: >= 0
==> default: Loading metadata for box 'almalinux/9'
    default: URL: https://vagrantcloud.com/almalinux/9
==> default: Adding box 'almalinux/9' (v9.1.20221117) for provider: virtualbox
    default: Downloading: https://vagrantcloud.com/almalinux/boxes/9/versions/9.1.20221117/providers/virtualbox.box
    default:
    default: Calculating and comparing box checksum...
==> default: Successfully added box 'almalinux/9' (v9.1.20221117) for 'virtualbox'!
==> default: Importing base box 'almalinux/9'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'almalinux/9' version '9.1.20221117' is up to date...
==> default: Setting the name of the VM: SEL_default_1676818962721_30998
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default:
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default:
    default: Inserting generated public key within guest...
    default: Removing insecure key from the guest if it's present...
    default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
    default: The guest additions on this VM do not match the installed version of
    default: VirtualBox! In most cases this is fine, but in rare cases it can
    default: prevent things such as shared folders from working properly. If you see
    default: shared folder errors, please make sure the guest additions within the
    default: virtual machine match the version of VirtualBox you have installed on
    default: your host and reload your VM.
    default:
    default: Guest Additions Version: 6.1.40
    default: VirtualBox Version: 7.0
==> default: Mounting shared folders...
    default: /vagrant => C:/Users/michi/OneDrive - Hogeschool Gent/Semester 2/SEL/Vagrant/SEL

```

Check if AlmaLinux is running

```
$ vagrant status
Current machine states:

default                   running (virtualbox)

The VM is running. To stop this VM, you can run `vagrant halt` to
shut it down forcefully, or you can run `vagrant suspend` to simply
suspend the virtual machine. In either case, to restart it again,
simply run `vagrant up`.
```

SSH into machine

```
michi@LAPTOP-AU8VQ3TK MINGW64 ~/OneDrive - Hogeschool Gent/Semester 2/SEL/Vagrant/SEL
$ vagrant ssh
[vagrant@localhost ~]$
```
