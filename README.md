<h1>**No longer supported. iocage is being rewritten in a different language. To fetch 10.3-RELEASE or above, supply the release to iocage:
<h2>`iocage fetch release=10.3-RELEASE`
<h1>This will skip the interactive dialog and proceed.

iocage
======

**A FreeBSD jail manager.**

iocage is a zero dependency, drop in jail/container manager amalgamating some
of the best features and technologies the FreeBSD operating system has to offer.
It is geared for ease of use with a simple and easy to understand command syntax.

iocage is in the FreeBSD ports tree as sysutils/iocage.
To install using binary packages, simply run: `pkg install iocage`

## WARNING:
The `develop` branch may not be functional at any given point. This is where a lot of experimental work gets done. No guarantees for stability or migrations within the branch are given. Things can and do change rapidly. **_Please don't use it if you require stability._**

- **[DOCUMENTATION](http://iocage.readthedocs.org/en/latest/index.html)**
- **Mailing list**: https://groups.google.com/forum/#!forum/iocage
- **Contributing**: If you would like to submit a pull request, we kindly ask you to open it against the `develop` branch.

####Raising an issue:

We _like_ issues! If you are having trouble with `iocage` please open a GitHub [issue](https://github.com/iocage/iocage/issues) and we will ~~run around with our hair on fire~~ look into it. Before doing so, please give us some information about the situation:
- Tell us what version of FreeBSD you are using with something like `uname -ro`
- It would also be helpful if you gave us the output of `iocage version`
- Most importantly, try to be detailed. Simply stating "I tried consoling into a jail and it broke" will not help us very much. However, saying something like this helps much, much more:


"I tried to console into a _template_ jail, but it failed with _this error_ in which _I pasted below:_"
````
gasgiant ~ # iocage list -t
JID  UUID                                  BOOT  STATE  TAG     TYPE
-    XXXXXXXX-XXXX-XXXX-a5be-3417ebb13845  off   down   pepperd  template
gasgiant ~ # iocage console salted
cannot open 'zroot/iocage/jails/XXXXXXXX-XXXX-XXXX-a5be-3417ebb13845': dataset does not exist
cannot open 'zroot/iocage/jails/XXXXXXXX-XXXX-XXXX-a5be-3417ebb13845': dataset does not exist
[: -ne: unexpected operator
  ERROR: jail: pepperd is not running!
````
- Use the [Markdown Basics](https://help.github.com/articles/markdown-basics/#code-formatting) GitHub page for more information on how to paste lines of code and terminal output. 

**FEATURES:**
- Templates, clones, basejails, fully independent jails
- Ease of use
- Zero configuration files
- Rapid thin provisioning within seconds
- Automatic package installation
- Virtual networking stacks (vnet)
- Shared IP based jails (non vnet)
- Resource limits (CPU, MEMORY, etc.)
- Filesystem quotas and reservations
- Dedicated ZFS datasets inside jails
- Transparent ZFS snapshot management
- Binary updates
- Differential jail packaging
- Export and import
- And many more!

**QUICK HOWTO:**

Fetch a release:

`iocage fetch`

Create a jail:

`iocage create tag=myjail ip4_addr="em0|192.168.1.10/24"`

Start the jail:

`iocage start myjail`

**USAGE:**
```
  iocage activate ZPOOL
  iocage cap UUID|TAG
  iocage chroot UUID|TAG [command]
  iocage clean [-a|-r|-j]
  iocage clone UUID|TAG [UUID|TAG@snapshot] [property=value]
  iocage console UUID|TAG
  iocage create [-b|-c|-e] [release=RELEASE] [pkglist=file] [property=value]
  iocage deactivate ZPOOL
  iocage defaults
  iocage destroy [-f] UUID|TAG
  iocage df
  iocage exec [-u username | -U username] UUID|TAG|ALL command [arg ...]
  iocage export UUID|TAG
  iocage fetch [release=RELEASE | ftphost=ftp.hostname.org | ftpdir=/dir/ |
                ftpfiles="base.txz doc.txz lib32.txz src.txz"]
  iocage get property|all UUID|TAG
  iocage help
  iocage import UUID [property=value]
  iocage init-host IP ZPOOL
  iocage inuse UUID|TAG
  iocage limits [UUID|TAG]
  iocage list [-b|-t|-r]
  iocage package UUID|TAG
  iocage promote UUID|TAG
  iocage rcboot
  iocage reboot UUID|TAG
  iocage rcshutdown
  iocage record start|stop UUID|TAG
  iocage reset UUID|TAG|ALL
  iocage restart UUID|TAG
  iocage rollback UUID|TAG@snapshotname
  iocage runtime UUID|TAG
  iocage set property=value UUID|TAG
  iocage show property
  iocage snaplist UUID|TAG
  iocage snapremove UUID|TAG@snapshotname|ALL
  iocage snapshot UUID|TAG [UUID|TAG@snapshotname]
  iocage start UUID|TAG
  iocage stop UUID|TAG
  iocage uncap UUID|TAG
  iocage update UUID|TAG
  iocage upgrade UUID|TAG [release=RELEASE]
  iocage version | --version
  ```

**REQUIREMENTS**
- FreeBSD 9.3-RELEASE amd64 or newer
- ZFS file system
- Optional - Kernel compiled with:

        # This is optional and only needed if you need VNET and resource
        # limits

        options         VIMAGE # VNET/Vimage support
        options         RACCT  # Resource containers
        options         RCTL   # same as above

**OTHER CONSIDERATIONS**
- For resource limiting please read rctl(8)
- For the explanations on jail properties read jail(8)
- Create bridge0 and bridge1 interfaces

**HINTS**
- Use iocage set/get to modify properties
- To understand what most properties do read iocage(8).
- If using VNET consider adding the following to `/etc/sysctl.conf` on the host:

        net.inet.ip.forwarding=1       # Enable IP forwarding between interfaces
        net.link.bridge.pfil_onlyip=0  # Only pass IP packets when pfil is enabled
        net.link.bridge.pfil_bridge=0  # Packet filter on the bridge interface
        net.link.bridge.pfil_member=0  # Packet filter on the member interface
