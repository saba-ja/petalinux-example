Creating PetaLinux project:

```
petalinux-create -t project -n plnx --template zynq
```

Change directory to the newly created PetaLinux folder:

```
cd plnx
```

Initial configuration:

```
petalinux-config --get-hw-description ../../export/system_wrapper.xsa
```

Change the root file system type to EXT4:

```
Prompt: EXT4 (SD/eMMC/SATA/USB)
     Location:
       -> Image Packaging Configuration
         -> Root filesystem type
           -> (EXT4 (SD/eMMC/SATA/USB))
```

Disable Network support during boot: (reason allow booting without network connection)

```
petalinux-config -c u-boot
```

From the main menu unselect "Networking support"



Enabaling packets:

```
petalinux-config -c rootfs
```

```
Prompt: curl
     Location:
       -> Filesystem Packages
         -> console
           -> network
   (1)       -> curl


     Location:
       -> Filesystem Packages
         -> libs
           -> network
   (1)       -> openssl


  Location:
       -> Filesystem Packages
         -> console
           -> network
   (1)       -> openssh


  Location:
       -> Filesystem Packages
         -> admin
   (1)     -> sudo


  Location:
       -> Filesystem Packages
         -> console
           -> network
   (1)       -> wget

```

Build the project:

```
petalinux-build
```

Packeting project boot files:

```
petalinux-package --boot --format BIN --fsbl images/linux/zynq_fsbl.elf   --fpga images/linux/system.bit --u-boot --force
```

Copying files to Boot folder (FAT32 format using fdisk):

```
sudo cp images/linux/BOOT.BIN /media/BOOT/
sudo cp images/linux/image.ub /media/BOOT/
sudo cp images/linux/boot.scr /media/BOOT/
```

Extracting rootfs file system to the (ext4 formated partion):

```
sudo tar -zxvf images/linux/rootfs.tar.gz -C /media/rootfs
```

At the end run the following to make sure all the buffers are coppied to the SD card:

```
sync
```