## Why do I need to install an Ubuntu VM anyway?

The reason we're using Ubuntu Linux is so that everyone in the class can use a common computing environment. This makes it much easier for me to provide scripts that install and setup software for you, and to troubleshoot problems you might be having.

It’s certainly possible to run all the software we’ll using on macOS and Windows. But installing and using it would be slightly different for everyone, depending on the flavor and version of OS you're running. If we can get Ubuntu running for everyone, it should be a less frustrating experience after that.

## Isn't there an easier way to achieve a common computing environment?

Yes, there is: we could use a cloud-based service such as [GitHub Codespaces](https://docs.github.com/en/codespaces). But that would cost money, and I don’t want to spend money, nor do I want you to.

## What do I need to have working?

There’s two things you need to have working:

1. A way to run programs in an Ubuntu Linux VM. It has to be be Ubuntu because I want to be able to write scripts that will use the Ubuntu package manager to install software for you.

2. A way to share files between your host OS (Windows or macOS) and the guest Ubuntu VM.

There are several possible ways to achieve the above, but I’m going to focus on two that will hopefully work for everyone: using [Multipass](https://multipass.run) and using [VirtualBox](https://www.virtualbox.org).

Of these two, Multipass is easier, but won’t work on everyone’s computer. VirtualBox *should* work for everyone. 

On Windows, unless you’re running Windows 10 or 11 Pro or Enterprise, you’ll need VirtualBox to run Multipass anyway. It’s still worth trying Multipass, though, as it does make some things easier.

## Using Multipass

First, install Multipass:
* [Windows instructions](https://multipass.run/docs/installing-on-windows)
* [macOS instructions](https://multipass.run/docs/installing-on-macos)

Unless you’re running Windows 10 or 11 Pro or Enterprise, note that you’ll have to also install VirtualBox and set it as the virtualization provider for Multipass. This process is covered in the instructions linked above.

After Multipass is installed, you can run the Multipass GUI app. This will put a little Multipass icon in your system tray (Windows) or menu bar (macOS).

To open a shell on an Ubuntu VM, you can either
* click on the Multipass icon in your system tray or menu bar and select `Open Shell`, or
* in a Windows command prompt or PowerShell or a macOS terminal, type `multipass shell` and press return.

Doing this should:
1. create a VM instance named `primary` using the latest version of Ubuntu,
2. start the instance, and
3. open a shell on the instance.

For more details, see the Multipass tutorial ([Windows](https://multipass.run/docs/windows-tutorial), [macOS](https://multipass.run/docs/mac-tutorial)).

### Sharing files between your host OS and the Ubuntu VM

The easiest way to share files between your host OS and the Ubuntu VM is to “mount” a host OS directory (i.e. folder) in your VM instance. When a directory is mounted, you can read from and write to it using either your host OS or the Ubuntu VM. That means you can use an editor like VS Code in your host OS, save files to the folder, and do things with those files in Ubuntu. Or you can create files in Ubuntu and then browse and open them from your host OS.

**On Windows, mounting directories is disabled by default.** This is for security reasons. However, unless you’re using your personal computer as a server on the Internet, it is safe to enable it. You can do this by opening a command prompt or PowerShell and running:

```
multipass set local.privileged-mounts=true
```

If you launch a VM instance named `primary` and mounting directories is enabled, then your home directory on your host OS will be automatically mounted at a directory named `Home` in your Ubuntu VM. The full path to the directory will be `/home/ubuntu/Home`.

If for some reason you don't see the `Home` directory mounted in your Ubuntu VM, you can mount your home directory manually:

Windows:
```
multipass mount %HOMEPATH% primary:/home/ubuntu/Home
```

macOS:
```
multipass mount $HOME primary:/home/ubuntu/Home
```

To see what directories are mounted on your `primary` instance, you can run:
```
multipass info primary
```

If you don't want to mount a directory in your instance for some reason, there is also a `transfer` command you can use to move files back and forth between the host OS and the VM. This is a little less convenient, but handy if you just want to quickly move a file. See [How to share data with an instance](https://multipass.run/docs/share-data-with-an-instance) for more details.

## Using VirtualBox

If Multipass isn't working for you, you can try using VirtualBox instead. VirtualBox has been around a lot longer, so it should work even on relatively old computers.

If you’re using VirtualBox, you’ll need to download [the Ubuntu server install ISO](https://releases.ubuntu.com/22.04/ubuntu-22.04.1-live-server-amd64.iso) (this is what will be used to install Ubuntu on your VM).

VirtualBox has comprehensive documentation—perhaps *too* comprehensive. You’ll want to at least read about:

* [installing VirtualBox](https://www.virtualbox.org/manual/ch02.html)
* [creating a VM](https://www.virtualbox.org/manual/UserManual.html#gui-createvm) 
* [running your VM](https://www.virtualbox.org/manual/UserManual.html#intro-running) (this is the step where you’ll use your ISO to install Ubuntu)
* [installing Guest Additions for Linux](https://www.virtualbox.org/manual/UserManual.html#additions-linux) (this is required to get shared folders working)
* [setting up shared folders](https://www.virtualbox.org/manual/UserManual.html#sharedfolders) (so that you can access a folder from both your host OS and the guest VM)
