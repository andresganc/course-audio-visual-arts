
# DAVINCI RESOLVE - INSTALL

## LINUX MAKERESOLVEDEB (Alternativa recomendada) 

- Muchos usuarios de Ubuntu prefieren usar la herramienta Makeresolvedeb, que convierte el instalador oficial en un archivo .deb estándar. Esto maneja automáticamente las dependencias y permite desinstalar el programa fácilmente desde el centro de software. 
¿Deseas ayuda para usar Makeresolvedeb o prefieres continuar con el método del instalador oficial?

https://www.danieltufvesson.com/makeresolvedeb

How it works

When you download Resolve or Resolve Studio for Linux you will get an installer made for the Rocky Linux / CentOS / RHEL system described in the Resolve configuration guide. That is the only officially supported configuration.

The native installer will install Resolve on your Debian based system but it will violate the Debian concept of fully tracked installations. The native installer forces software components into place and modifies parts of the OS in a way that is unbeknown to the Debian package manager. This practice will impede system reliability. MakeResolveDeb aims to solve that issue while including the Debian specific features required for a working Resolve system.

MakeResolveDeb is made for Debian and Debian based derivatives such as Ubuntu and Linux Mint.

MakeResolveDeb takes the official installer, unpacks it, and then reassembles it into a *.deb package that can be installed, and removed, using your favorite Debian package management tool.
Disclaimer

DaVinci Resolve and Fusion are products by Blackmagic Design Pty. Ltd. MakeResolveDeb and MakeFusionDeb are in no way endorsed by or connected to Blackmagic Design Pty. Ltd. I do not redistribute DaVinci Resolve or Fusion or provide pre-fabricated *.deb files for the products.

The information and tools are provided here at your own risk. I will not be liable for any losses or damages as a result of using information or tools from this web site.
MakeResolveDeb Multi

Up until now each version of MakeResolveDeb was tied to a specific version of DaVinci Resolve. This is no longer the case and MakeResolveDeb Multi is now following its own versioned releases. Starting from version 1.0 of MakeResolveDeb Multi there is no longer a need to match specific versions with DaVinci Resolve versions. Just make sure you use the latest MakeResolveDeb Multi and it will be able to handle all released and supported versions of DaVinci Resolve up until the time of release, starting at 15.2.2.
Instructions
Download DaVinci Resolve installer

Go to www.blackmagicdesign.com and download the official installer *.zip archive for DaVinci Resolve or DaVinci Resolve Studio for Linux and save it into a new empty directory.
Download MakeResolveDeb

Download the latest version of the MakeResolveDeb *.tar.gz archive from this website and put it in the same directory as the Resolve installer *.zip archive.
Unpack downloaded archives

From now on it's easiest to continue in the terminal. Open up a new terminal window and go to the directory where you downloaded the Resolve installer and MakeResolveDeb and unpack both archives.

For example:
cd ~/resolvedeb
unzip DaVinci_Resolve_Studio_19_Linux.zip
tar zxvf makeresolvedeb_1.7.2_multi.sh.tar.gz

You should now have the following files in the directory:
DaVinci_Resolve_Studio_19_Linux.run
DaVinci_Resolve_Studio_19_Linux.zip
Linux_Installation_Instructions.pdf
makeresolvedeb_1.7.2_multi.sh.tar.gz
makeresolvedeb_1.7.2_multi.sh
Run MakeResolveDeb

When you have unpacked the archives and have all the needed files in the directory it's time to assemble the new *.deb file using MakeResolveDeb. First install required packages.
sudo apt-get install fakeroot

For Resolve 15.x you also need xorriso:
sudo apt-get install xorriso

The current version of MakeResolveDeb works for both Resolve and Resolve Studio. MakeResolveDeb will detect the Resolve variant in the archive and generate the appropriate Debian package. Run MakeResolveDeb and give the DaVinci Resolve installer *.run archive file name as the only argument.

For example:
./makeresolvedeb_1.7.2_multi.sh DaVinci_Resolve_Studio_19_Linux.run

The conversion can take a few minutes depending on computer and storage performance. If there are errors during the process it will be displayed on the terminal. A successful conversion is indicated by the last line saying "[DONE]" and the reported number of errors 0.

NOTE: MakeResolveDeb will store temporary data in the working directory. Make sure you have enough space available on the filesystem used for the conversion (approximately 4x the size of the downloaded archive).
Installing the Debian package

A successful conversion generates a *.deb file that can be installed to your system. To install Resolve you can use dpkg.

For example:
sudo dpkg -i davinci-resolve-studio_19-mrd1.7.2_amd64.deb

or
sudo dpkg -i davinci-resolve_19-mrd1.7.2_amd64.deb
DaVinci USB devices

If you have any DaVinci USB devices connected during the install you need to disconnect and reconnect them after the install completes in order for them to work properly. This includes license dongle, Editor Keyboard, Speed Editor and Micro/Mini Panel.
Installing the appropriate GPU drivers

You will have to make sure that you have all the driver packages installed that are required by Resolve. The GPU choice and GPU driver setup varies depending on the system. Use the latest available driver for your system. For NVIDIA and Debian it will be something like this:
sudo apt-get install nvidia-driver nvidia-opencl-icd libcuda1 libglu1-mesa

For h.264 and h.265 export you also need the NVIDIA encode library:
sudo apt-get install libnvidia-encode1

Also make sure there is only one OpenCL ICD installed. This can be verified by looking in the directory /etc/OpenCL/vendors/. There should be one and only one *.icd file there. For NVIDIA it will usually be called nvidia.icd.

Even if you plan to run CUDA you need to have OpenCL installed.

The current Debian driver situation for AMD/Radeon is unpolished to put it mildly and it's not packaged properly for Debian. Installation method for AMD/Radeon will be left as an exercise to the reader. You will need the amdgpu-pro driver with OpenCL support.

Running Resolve using an Intel GPU under Linux is not supported.
Upgrading Resolve

When there are new updates to Resolve or MakeResolveDeb you only need to repeat the process above. There is no need to uninstall any previous Resolve versions when upgrading as long as both versions are using MakeResolveDeb. The Debian package will take care of the upgrade for you.

Switching between Resolve and Resolve Studio may require you to first uninstall the existing package and then install the new package. It is not possible to accidentally install both packages.

Settings and user data will be retained when upgrading, but always make sure you have proper backups of your projects before making any changes.
Uninstalling Resolve

To uninstall Resolve you should use the Debian package manager. Do not start deleting directories manually or use the official (un)installer.

Uninstall DaVinci Resolve:
sudo apt-get remove davinci-resolve

Uninstall DaVinci Resolve Studio:
sudo apt-get remove davinci-resolve-studio
Common problems
MakeResolveDeb reports missing function and stops

If the conversion process stops and gives an error message such as
Sorry. Need 'xyz' to continue.

Then you are missing a required system package to make the conversion. To fix this you just have to install the package mentioned in the error message and re-run the conversion.

For example:
sudo apt-get install xyz

(where xyz is the name of the missing package)
Resolve appears to start but only shows a white modal window

This often happens when the Onboarding feature does not work properly. Generally it is possible to force quit Resolve and next time it starts up correctly. Lately in Resolve 17 this has been caused by libssl issues. One work-around for this issue is to remove the Onboarding feature entirely from the resolve directory. This way Resolve will not find and execute the Onboarding process and will start normally.

Since MakeResovleDeb 1.5.1 there is an argument that can be provided that excludes the Onboarding feature from the Debian package. When running MakeResolveDeb just add the optional argument --skip-onboarding to create a Resolve Debian package without the Onboarding feature.
When Resolve doesn't start at all or exits immediately

    Run resolve from a prompt (/opt/resolve/bin/resolve) and check the output for error messages
    Make sure you have all required libraries installed. Run "ldd /opt/resolve/bin/resolve" and verify that there are no missing libraries (ldd should give no lines with "not found" in them)
    Log files provide a lot of useful information. Please check for clues. Location differs between Resolve versions.
    - Resolve 15 and below: /opt/resolve/logs/
    - Resolve 16 and up: ~/.local/share/DaVinciResolve/logs/
    Resolve is quite picky when it comes to GPU drivers and versions. Make sure you have working CUDA and OpenCL libraries installed.
    Segmentation fault on startup or abrupt exit before reaching the project window usually means missing GPU drivers, unsupported GPU driver version or unsupported GPU hardware. Check log files for clues.

Displaying a rolling Reslove log

Not all errors are visible in the graphical interface. Sometimes it's a good idea to look at the rolling log as Resolve is running to catch error messages. To do this you can open up a terminal window and run the command below.
tail -F  ~/.local/share/DaVinciResolve/logs/ResolveDebug.txt
This will continuously print the Resolve log messages to the terminal.

(For Resolve 15 and below the path would instead be /opt/resolve/logs/ResolveDebug.txt)
Resolve crashes when displaying certain parts of the UI

Some versions of Resolve will crash if the en_US.UTF locale is not present on the system. This can sometimes be the case for non-english OS installs. You can configure available locales when setting up the "locales" package.
sudo dpkg-reconfigure locale
Enable en_US.UTF from the menu that appears.
Can't download "Extras" in Resolve 20

Resolve 20 introduced the Extra downloads feature where Resolve can download components not included in the base installer. This feature is hard coded to make use of the RedHat/CentOS/Rocky path for CA certificate file in /etc/pki/tls/certs/ca-bundle.crt, but in the Debian world this file is stored at /etc/ssl/certs/ca-certificates.crt

To fix this you can add a symbolic link that will replicate the RedHat/CentOS/Rocky structure by running the two commands below which will first create the required directory structure and then the symbolic link for the CA file. It's important to create a link here instead of copying the file since this is a file that will be kept up to date by the OS packages.

    sudo mkdir -p /etc/pki/tls/certs/
    sudo ln -s /etc/ssl/certs/ca-certificates.crt /etc/pki/tls/certs/ca-bundle.crt

MakeResolveDeb intentionally does not include this fix since that would break the Debian concept of packages not touching files they are not responsible for, and could potentially have unpredictable consequences. The appropriate fix has to be incorporated into Resolve to look for this CA file in multiple locations.
Still having problems?

Check out the threads in the Blackmagic Design Forums using the link below or use my contact form at the bottom of the page.

DaVinci Resolve on Debian Linux (and makeresolvedeb)

DaVinci Resolve on Linux - Install issues
All MakeResolveDeb versions

1.8.3 - Released 2025-06-13
Add Apple Immersive support introduced in Resolve 20.1

1.8.2 - Released 2025-06-13
Fix for Resolve 20 Extras download dir.

1.8.1 - Released 2025-04-23
Store dpkg temp locally. Fix panel USB permissions.

1.8.0 - Released 2025-04-05
Added support for DaVinci Resolve 20.

1.7.3 - Released 2024-10-20
Fix for bundled libs that are known to cause problems.

1.7.2 - Released 2024-08-22
Fix Remote Monitor only available for Resolve Studio.

1.7.1 - Released 2024-04-14
Shell and mime fixes. Added renamed remote monitor shortcut for Resolve 19.

1.7.0 - Released 2024-04-12
Added support for DaVinci Resolve 19.

1.6.4 - Released 2022-12-05
Ensure permissions are correct after native installer extraction.

1.6.3 - Released 2022-11-25
Change installer extraction method for Resolve 17 & 18.

1.6.2 - Released 2022-11-12
Update dpkg-deb information message and bypass archive package dependency check.

1.6.1 - Released 2022-11-11
Fix for Resolve 18.1 installer not handling relative paths.

1.6.0 - Released 2022-04-19
Added support for Resolve 18 beta.

1.5.1 - Released 2021-07-01
Added support for Resolve 17.2.2 that no longer needs BlackmagicRawAPI fixes.

1.5.0 - Released 2021-04-14
Various small fixes. Improved argument handling and parsing. Added arguments for optionally excluding Onboarding and BRAW Player and Speed Test.

1.4.6 - Released 2021-03-30
Added missing Fairlight Panel API lib.

1.4.5 - Released 2021-02-10
Added root user check. Updated Debian package optional control fields. Made BMRAW lib symlinks relative. Improved directory creation error logging. Improve archive argument error handling.

1.4.4 - Released 2021-01-07
Added file permission enforcements for installer archive and deb control files. Improved logging of archive extraction process.

1.4.3 - Released 2020-12-17
Fixed udev rules and added postinst udev reload/trigger.

1.4.2 - Released 2020-12-15
Updated udev rules.

1.4.1 - Released 2020-12-07
Added support for the Editing Keyboard and the Speed Editor in Resolve 17.

1.4.0 - Released 2020-11-09
Added support for DaVinci Resolve 17.

1.3.1 - Released 2020-10-21
Fix for spelling error (not affecting generated deb package).

1.3 - Released 2020-10-12
Added support for Blackmagic raw player and speed test where applicable. Restore bash fixes reverted in 1.2 (they were not the problem). Added better reporting in case of missing files for desktop shortcuts.

1.2 - Released 2020-10-12
Rolled back some bash fixes since some systems produced incomplete Debian packages.

1.1 - Released 2020-10-12
Removed libpanelfix for Resolve 16 (please report hardware panel problems). Added libcudafix for Resolve 16.0 and 16.1. Ignoring missing techical docs for 16.0b versions. General bash improvements.

1.0 - Released 2020-10-11
First multi version. Includes most features up until this date.
BONUS: MakeFusionDeb

Just like MakeResolveDeb but for Fusion!

1.5.0 - Released 2025-05-24
Added support for Fusion 20 and store dpkg temp locally.

1.4.0 - Released 2024-08-29
Added support for Fusion 19.

1.3 - Released 2022-05-01
Added support for Fusion 18.

1.2 - Released 2021-11-30
Fixed Templates for Fusion 17.4.2

1.1 - Released 2020-11-12
Fixed printout for Fusion 17.x

1.0 - Released 2020-10-31
First multi version. Includes most features up until this date.

For legacy versions of MakeResolveDeb and MakeFusionDeb go here
