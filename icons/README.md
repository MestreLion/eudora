Eudora Icons
============

Place here any icons you'd like the Eudora installer to use. You can delete,
replace or add more icons of different sizes. The ones present here are the
official, default icons Eudora uses in Windows.

Filename should be:

	anyname-WIDTH.png

Where `WIDTH` is, duh, the width of the image. For example, `eudora-48.png` for
a 48x48 icon. Image does not have to be actually that size, it will only be
registered as such.

Also, `WIDTH` is optional: installer can auto-detect image size if you have
ImageMagick installed, which Ubuntu already ships by default.


Generating icons
----------------

You can generate Eudora icons yourself by extracting them from Eudora.exe:

First, install ImageMagick and IcoUtils:

	sudo apt-get install imagemagick icoutils

Now download the Eudora installer:

	wget http://www.eudora.com/download/eudora/windows/7.1/Eudora_7.1.0.9.exe

Install Eudora in a temp folder:

	WINEPREFIX="$PWD/temp" WINEDLLOVERRIDES=winemenubuilder.exe=d \
	wine Eudora_7.1.0.9.exe 2> /dev/null  # wine is too chatty

After install completes, extract the icons as PNGs:

	wrestool --extract --name=64 "$(find temp -name Eudora.exe)" |
	convert -set filename:area '%w' ico:-[6-8] 'eudora-%[filename:area].png'

And now you should have 3 files:

	eudora-16.png
	eudora-32.png
	eudora-48.png

And you can delete the temporary files:

	rm -rf Eudora_7.1.0.9.exe temp

The `eudora-128.png` present here  was generated in a similar way, but using the
Eudora for Mac icon.


In a nutshell
-------------

For the copy-and-paste lazy ones like me:

	sudo apt-get --yes install imagemagick icoutils &&
	wget http://www.eudora.com/download/eudora/windows/7.1/Eudora_7.1.0.9.exe &&
	WINEPREFIX="$PWD/temp" WINEDLLOVERRIDES=winemenubuilder.exe=d \
	wine Eudora_7.1.0.9.exe 2>/dev/null &&
	wrestool --extract --name=64 "$(find temp -name Eudora.exe)" |
	convert -set filename:area '%w' ico:-[6-8] 'eudora-%[filename:area].png' &&
	rm -rf Eudora_7.1.0.9.exe temp &&
	ls -l

***Ta-da!***


Details
-------

`WINEDLLOVERRIDES` argument disables desktop integration. Since this is a temp
install, we don't want wine to create menus, icons and mime entries


`--name=64` is the ID of one of the 2 icon sets present in Eudora.exe. If you
want to see the other set instead, use `--name=368`. You can list them using:

	wrestool --list --type=group_icon "$(find temp -name Eudora.exe)"  # icons only
	wrestool --list "$(find temp -name Eudora.exe)"  # all resources

That icon group consists of 9 images, 3 color depths sets with 3 different
dimensions each. `[6-8]` tells convert to only extract images 6, 7 and 8,
which are hi-color 16-bit, ignoring the 4-bit and 8-bit ones.

You can list all images and their properties using:

	wrestool --extract --name=64 "$(find temp -name Eudora.exe)" |
	identify ico:-

Finally the 2 mentions of `filename:area` are simply a way to generate files
with the image width `'%w'` already in the filename, instead of the default
sequencial numbering.


Legalese
--------

Strickly speaking, I could not upload the Eudora official icons in this
repository: they are copyrighted by Qualcomm Inc and, while gratis to use,
distribuition is forbidden. So legally speaking these icons should not be here,
for the same reason I do not include Eudora_7.1.0.9.exe installer and Eudora.exe
binaries.

But I've included the icons as a *convenience* to users, so the installer does
not depend on icoutils or imagemagick to work. Also, I'm helping spread Eudora
to a platform Qualcomm never dreamed about, and enabling users to use their
product. And I'm not charging Qualcomm (or anyone) for such work.

But if Qualcomm disagrees, and does not like their now defunct (but still loved)
7-year old discontinued gratis software to be easily and conveniently integrated
in GNU/Linux, using official icons, without costing them a single penny for such
work, please let me know and I will promptly (but sadly) remove the icons.
