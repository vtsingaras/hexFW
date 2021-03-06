# hexFW
hexFW is an attempt to provide a user friendly CFW solution for the Wii U.

# Summary
The code in this repository is divided into two main folders:
  + **"firmware"**: IOSU patching framework
  + **"launcher"**: exploit code chain responsible for injecting the patched IOSU image
  
Currently, **iosuhax** (by **smealum**) is the basis for the firmware patching framework. This project's goal is to build upon smealum's patching system to deliver a fully functional and customizable Wii U CFW.
The exploit chain used to inject the firmware's code uses **yellows8**'s **wiiu_browserhax_fright** and is a direct implementation of two distinct vulnerabilities documented by **hykem**, **naehrwert** and **plutoo**. The exploit is compiled using a stripped down version of the **libwiiu** project and is triggered from the Wii U's **Web Browser**.

# Dependencies
  + For Windows users: [Cygwin](https://www.cygwin.com/), [MinGW](http://www.mingw.org/) or a unix-like shell, environment or operating system. 
  + [devkitPRO](https://sourceforge.net/projects/devkitpro/) (devkitPPC and devkitARM)
  + [armips](https://github.com/Kingcom/armips) (for assembling ARM patches; you can find a precompiled binary at https://github.com/hexkyz/armips/releases)
  + [Python](https://www.python.org/) 2.x or 3.x
  + [XAMPP](https://www.apachefriends.org/index.html) or equivalent for self-hosting (optional)

# Building
  + Place your retail **"fw.img"** file (encrypted or decrypted but with the header attached) inside the folder **"firmware/img"**.
  + Copy **"armips.exe"** into the root of the **"firmware"** folder.
  + Edit **"firmware/scripts/anpack.py"** and manually replace the dummy ancast keys with the real ones.
  + Browse back to the main folder (**"hexFW"**) and run **"make"** from a shell.
  
# Usage
  + After building the project a new folder **"bin"** will be created in the root folder (**"hexFW"**) as well as two sub-folders **"www"** and **"sdcard"**.
  + Copy the **"fw.img"** file inside **"sdcard"** into the root of your SD card (FAT32 formatted, preferably).
  + Setup a server (e.g.: **localhost:8080**) and host the contents of **"www"**. After inserting the SD card (with the firmware image) into the Wii U, browse to **"wiiu_browserhax.php"** and pass along your target system's version (e.g.: **localhost:8080/wiiu_browserhax.php?sysver=550**).
  + The launcher will run (**"fwboot"**) and launch the firmware image from the SD card.
  
## hexcore
**hexcore** is the default program distributed with hexFW. It's code is injected into IOS-MCP and runs in a dedicated thread, similar to how the old wupserver works.
Upon launching the generated **"fw.img"**, you will be presented with a barebones recovery console with the following options:  

  + Dump OTP -> Dumps your console's OTP into the SD card
  + Dump SEEPROM -> Dumps your console's SEEPROM into the SD card
  + Dump SLC/SLCCMPT -> Dumps a raw image of the SLC and SLCCMPT into the SD card
  + Launch wupserver -> Sets up wupserver and proceeds with booting sysNAND
  + Shutdown -> Simply shuts the console down
  + Credits -> Displays the credits page for 10 seconds
  
You can browse the options' list by pressing the **"Eject"** button and confirm by pressing the **"Power"** button.
Please note that this is still a work in progress and is meant to showcase the potential for a complete CFW solution. More functionalities will be added in time and the general mode of operation may change at any time.
  
# Credits
  + **smealum** - iosuhax project (https://github.com/smealum/iosuhax)
  + **yellows8** - wiiu_browserhax_fright exploit (https://github.com/yellows8/wiiuhaxx_common and https://github.com/yellows8/wiiu_browserhax_fright)
  + **hykem**, **naehrwert** and **plutoo** - IOS-USB bad array index check (uhshax) vulnerability (http://wiiubrew.org/wiki/Exploits#IOS-USB_bad_array_index_check_.28uhshax.29); IOS_CreateThread unchecked memset vulnerability (http://wiiubrew.org/wiki/Exploits#IOS_CreateThread_unchecked_memset); extensive documentation and research on the Wii U
  + **libwiiu team** - libwiiu framework (https://github.com/wiiudev/libwiiu)
