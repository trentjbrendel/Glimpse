# Glimpse Image Editor Windows Installer
These bespoke Windows installers are built on the [WiX Toolset](https://wixtoolset.org/).

The instructions provided assume you have already installed [MSYS2](https://www.msys2.org/) on your system, and then the pre-requisites for Glimpse Image Editor inside that build environment. Full instructions are provided in `HACKING.md`.

## Limitations
We have provided the 32-bit libintl.dll file in this folder for compatibility with third-party plugins. Our understanding is that it is licensed under the GNU LGPL, so we are permitted to include it with this source code for convenience.

We do not support Python plug-ins or filters on Windows. Initially this was due to a bug with MSYS2 and gaps in the upstream build docs, but now that [Python 2 is end of life](https://www.python.org/doc/sunset-python-2/) we think the performance and security benefits of *not* including it are worthwhile. We stopped including Python 2 components and dependencies in our Windows installers entirely from 0.2.0 onwards.

These manifests were made for [WiX Toolset 3.11.2](https://github.com/wixtoolset/wix3/releases/tag/wix3112rtm). We have tried to avoid relying on default values where possible, but your mileage may vary with newer versions.

Finally, if you redistribute your MSI file(s) they may trigger [Windows SmartScreen](https://support.microsoft.com/en-us/help/17443/windows-internet-explorer-smartscreen-faq) and anti-virus programs on other peoples' computers. That is because your MSI file [has not been signed](https://wixtoolset.org/documentation/manual/v3/overview/insignia.html). You can theoretically self-sign for development and testing purposes, but you need a [CA validated certificate](https://docs.microsoft.com/en-us/windows-hardware/drivers/dashboard/get-a-code-signing-certificate) for production use.

## How to package Glimpse Image Editor for Windows x86
These instructions will package the code as a 32-bit Windows MSI installer. To build the code first, follow the directions provided in the `HACKING.md` file.

1. Install the WiX toolset and ensure the `%WIX%` environment variable has been set.
2. Create a folder in this directory called `InputFiles32`
3. Copy the contents of your 32-bit build folder to `InputFiles32`
4. Copy the `AUTHORS` file to `InputFiles32\AUTHORS.TXT`
5. Copy the `COPYING` file to `InputFiles32\COPYING.TXT`
6. Copy the `LICENSE` file to `InputFiles32\LICENSE.TXT`
7. Copy the `intl.dll` file in this directory to `InputFiles32\bin\`
8. Copy `build-aux\windows\fileicon.ico` to `InputFiles32\bin`
9. Copy the contents of `C:\msys64\mingw32` to `InputFiles32`
10. Navigate to the directory this file is in using a command prompt window and run the following commands:

```bat
"%WIX%bin"\candle.exe glimpse-0-2-0-i686.wxs -arch x86
"%WIX%bin"\light.exe glimpse-0-2-0-i686.wixobj -ext WixUIExtension
```
The component versions in `glimpse-0-2-0-i686.wxs` may differ from your own, so you will need to update those appropriately. 

You will be pleased to know that we do not include the whole of MinGW inside the MSI file! The WiX build tools just pick out the dependencies we have determined that Glimpse Image Editor needs to function through a combination of experimentation and guesswork.

## How to package Glimpse Image Editor for Windows x64
These instructions will package the code as a 64-bit Windows MSI installer. To build the code first, follow the directions provided in the `HACKING.md` file.

If you want 32-bit plugin support, you also need a working 32-bit build environment in MSYS2. If you don't, you scan skip that step but you will need to remove the relevant XML tags from `glimpse-x64.wxs`.

1. Install the WiX toolset and ensure the `%WIX%` environment variable has been set.
2. Create a folder in this directory called `InputFiles64`
3. Copy the contents of your 64-bit build folder to `InputFiles64`
4. Copy the `AUTHORS` file to `InputFiles64\AUTHORS.TXT`
5. Copy the `COPYING` file to `InputFiles64\COPYING.TXT`
6. Copy the `LICENSE` file to `InputFiles64\LICENSE.TXT`
7. Copy the contents of `C:\msys64\mingw64` to `InputFiles64`
8. Create a new folder at `InputFiles64\32` and copy the contents of `C:\msys64\mingw32` into it
9. Copy the `intl.dll` file in this directory to `InputFiles64\32\bin\`
10. Copy `build-aux\windows\fileicon.ico` to `InputFiles64\bin`
11. Download the ZIP version of G'MIC from https://gmic.eu/download.shtml, extract it, then copy the `gmic_gimp_qt` folder to `InputFiles64\lib\glimpse\2.0\plug-ins\`
12. Navigate to the directory this file is in using a command prompt window and run the following commands:

```bat
"%WIX%bin"\candle.exe glimpse-0-2-0-x64.wxs -arch x64
"%WIX%bin"\light.exe glimpse-0-2-0-x64.wixobj -ext WixUIExtension
```
The component versions in `glimpse-0-2-0-x64.wxs` may differ from your own, so you will need to update those appropriately. 

You will be pleased to know that we do not include the whole of MinGW inside the MSI file! The WiX build tools just pick out the dependencies we have determined that Glimpse Image Editor (64-bit) needs to function through a combination of experimentation and guesswork.

## Testing your Glimpse Image Editor installer
From a command prompt in the same directory as this file, you can test your MSI installer with logging enabled. The first line works for most cases, but you may want to use the second line if you need more verbose output:

```bat
msiexec /i glimpse-0-2-0-i686.msi /l* glimpse-0-2-0-i686.log
msiexec /i glimpse-0-2-0-i686.msi /l*v glimpse-0-2-0-i686.log
```

To uninstall Glimpse Image Editor without having to go through the "Add/Remove Programs" control panel area:

```bat
msiexec /x glimpse-0-2-0-i686.msi
```

These commands also work with the other two installers.
