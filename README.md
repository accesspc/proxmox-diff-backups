# diff-backups

Differential backups patch

# Package - file list
```
pve-manager:		/usr/share/pve-manager/js/pvemanagerlib.js
pve-container:		/usr/share/perl5/PVE/LXC/Create.pm
libpve-storage-perl:	/usr/share/perl5/PVE/Storage.pm
qemu-server:		/usr/share/perl5/PVE/QemuServer.pm
libpve-storage-perl:	/usr/share/perl5/PVE/Storage/Plugin.pm
pve-manager:		/usr/share/perl5/PVE/VZDump.pm
pve-container:		/usr/share/perl5/PVE/VZDump/LXC.pm
qemu-server:		/usr/share/perl5/PVE/VZDump/QemuServer.pm
```

# HELP - in case patch/revert doesn't work any more
apt install --reinstall libpve-storage-perl pve-container pve-manager qemu-server

# Installation
* Update Proxmox up to 5.2 version
* Clone this repository (you should've done this by now)
* Run the following commands:

```
wget http://ayufan.eu/projects/proxmox-ve-differential-backups/pve-xdelta3_3.0.6-1_amd64.deb
dpkg -i pve-xdelta3_3.0.6-1_amd64.deb
bash pve-5.2-diff-backup-addon apply
```

[Source](https://ayufan.eu/projects/proxmox-ve-differential-backups/)

# Notes
* Get new version
`grep -A 1 'version_text' /usr/share/perl5/PVE/pvecfg.pm | tail -1 | cut -d "'" -f 2 | cut -d "/" -f 1`

* Pull latest (not-patched) files
```
rm -rf temp
mkdir -p temp

scp -r root@10.1.40.6:/usr/share/pve-manager ./temp/
scp -r root@10.1.40.6:/usr/share/perl5/PVE ./temp/

mkdir -p ./pve-manager/js ./PVE/{LXC,Storage,VZDump}

mv temp/pve-manager/js/pvemanagerlib.js ./pve-manager/js/pvemanagerlib.js
mv temp/PVE/LXC/Create.pm ./PVE/LXC/Create.pm
mv temp/PVE/Storage.pm ./PVE/Storage.pm
mv temp/PVE/QemuServer.pm ./PVE/QemuServer.pm
mv temp/PVE/Storage/Plugin.pm ./PVE/Storage/Plugin.pm
mv temp/PVE/VZDump.pm ./PVE/VZDump.pm
mv temp/PVE/VZDump/LXC.pm ./PVE/VZDump/LXC.pm
mv temp/PVE/VZDump/QemuServer.pm ./PVE/VZDump/QemuServer.pm

rm -rf temp
```

* Commit, branch and patch
git add .
git commit -am "$version"
git checkout -b $version