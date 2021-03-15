# legion-trackpad-i2c
## Compile and load a patched kernel touchpad i2c module for the legion 5
#### Ensure you have <code>linux-headers</code> and <code>build-essential</code> on Debian/Ubuntu, <code>base-devel</code> on Arch
On fedora I believe the package is called <code>development-tools</code> however I have not tested :/ <br>
Touchpad and trackpad are referring to the same thing in this context


- Download the patched touchpad module (**i2c-hid_standalone.zip**) from this github repo
- Extract
- Open terminal in downloads folder, wherever you have extracted
- <code>cd i2c-hid_standalone/i2c-hid_standalone/</code>
- <code>make</code>
- <code>sudo rmmod i2c-hid</code>
- <code>sudo insmod ./i2c-hid.ko polling_mode=1</code>
- Check that your touchpad works now, we will add a kernel parameter to ensure that it remains working even after reboots
- <code>ls /lib/modules/$(uname -r)/kernel/drivers/hid/i2c-hid/</code> and do either of these :
	- If the output is .xz
		- <code>xz ./i2c-hid.ko</code>
		- <code>sudo cp ./i2c-hid.ko.xz /lib/modules/$(uname -r)/kernel/drivers/hid/i2c-hid/i2c-hid.ko.xz</code>
	- If the output is .ko
		- <code>sudo cp ./i2c-hid.ko /lib/modules/$(uname -r)/kernel/drivers/hid/i2c-hid/i2c-hid.ko</code>
- <code>/etc/default/grub</code> and add the following
	- There are already some pre-existing arguments seperated by spaces we add <code>i2c_hid.polling_mode=1</code> to this
	- <code>GRUB_CMDLINE_LINUX_DEFAULT="[pre-existing stuff] i2c_hid.polling_mode=1"</code>
- Lastly we will rebuild the kernal and update grub
	- On Debian/Ubuntu
		- <code>sudo update-initramfs -u</code>
		- <code>sudo update-grub</code>
	- On Arch
		- <code>sudo mkinitcpio -P</code>
		- <code>sudo grub-mkconfig -o /boot/grub/grub.cfg</code>
	- On Fedora (i think this is it)
		- <code>dracut -v -f</code>
		- UEFI: <code>sudo grub2-mkconfig -o /boot/efi/EFI/fedora/grub.cfg</code>
		- UEFI: <code>sudo grub2-mkconfig -o /boot/grub2/grub.cfg</code>
<br>
## Now reboot your system and the touchpad should be working. 
