Linux HP Laserjet 1010/1015 paper type settings
==
## WHAT IT IS

This patch restores the lost paper type setting for Hewlet Packard Laserjet 1010 and Laserjet 1015

Thick paper (250 g/m<sup>2</sup>)

![Fuser settings on thick paper](fuser_settings.jpg)
Fuser settings on thick paper.

"Light" mode | "Plain" mode | "Rough" mode
-------------|--------------|-------------

Correct speed and fuser temperature are essential for high-quality prints.

The printer itself supports 14 types of paper:

Menu entry   | ?Printer mode?
-------------|---------------
Bond         | Thick
Cardstock    | Extra Rough
Color        | Normal
Envelope     | Envelope
Heavy        | Thick
Labels       | Thick
Letterhead   | Normal
Light        | Light
Plain        | Normal
Preprinted   | Normal
Prepunched   | Normal
Recycled     | Normal
Rough        | Thick
Transparency | Light

## INSTALLATION
(My OS Ubuntu 23.10)

Enable source code repositories
In the **Software & Updates** (software-properties-gtk) by checking **Ubuntu Software -> Source Code**

(Alternatively, you can enable source code repositories by uncommenting deb-src repositories in /etc/apt/sources.list)


Then
```bash
sudo apt update

# change working directory to something what you want
mkdir ~/Desktop/printer_patch
cd ~/Desktop/printer_patch

# clone this repository
sudo apt install git
git clone https://github.com/sovahc/hplip_Laserjet_1010_1015_patch.git

# install hplip build dependencies
sudo apt build-dep hplip

# get hplip source code (It will be unpacked to the current directory)
apt source hplip

# go to hplip source code directory
cd hplip-3*

# apply this patch
patch -p1 <../hplip_Laserjet_1010_1015_patch/my.patch

# three files will be patched

# this is also needed
sudo apt install fakeroot

# maybe this
sudo apt build-dep hello

# build .deb package
dpkg-buildpackage -rfakeroot -uc -b

# up to generated .deb files
cd ..

# install compiled driver (Only this part is updated with a patch)
sudo dpkg -i hplip_3*.deb

```

Go to **Settings->Printers**.
Select your Laserjet 1010/1015 and Press **Remove Printer**

Turn off the printer, wait 10 seconds and turn it on again.
The driver will be reinstalled.

Go to **Printing Options** and press **Paper Type**

Now you should see 14 types of paper instead of one.

## USAGE

In CUPS **Printing options**:

* For thin paper select: Light
* For thick paper select: Rough (the printer will take longer to start and print slower)
* Otherwise select default: Plain

## UNINSTALLATION

* Just restore the original module

```shell
sudo apt reinstall hplip
```

* Press **Remove Printer**
Then turn the printer off and on again.

* Remove your build directory.
