# Gathering files

## OpenCore

### Extraction

After downloading [OpenCorePkg (**pleeeeeeease grab the debug build**)](https://github.com/acidanthera/OpenCorePkg/releases), extract the files to a well-organised place (like a desktop, but not when it's filled with apps):

![](../../images/AMD/AMD-Gathering-Files/PutInDesktop.png)

:::info A release build (optimized) is a build type where the code is optimised in the compilation process speeding up execution and removing debugging information. In contrast, an unoptimised debug build contains more debugging information and logs, but is significantly slower than an optimised build. Use debug builds only when troubleshooting.:::

### Preparing the EFI

As you can see in the extracted folder, there are four folders, two of which are named **IA32** and **X64**, the former of which are for 32-bit firmwares, and the latter for 64-bit firmwares. Copy whichever you think your firmware supports to ***the root*** of your USB (Copy the EFI folder inside it, not IA32 or X64).
:::warning Please use X64 folder for X64 firmwares.:::

And then, move the `Sample.plist` from (the OpenCorePkg folder)/Docs to your USB -> EFI -> OC:

![](../../images/AMD/grabthesampledotplist.png)

And then rename it to config.plist:

![](../../images/AMD/copyandrenameconfig.png)

