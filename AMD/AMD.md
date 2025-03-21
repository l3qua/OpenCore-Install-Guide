# AMD Subguide

:::warning This page is made with AMD-based machines/CPUs in mind, and is based off the ChefsKiss guide. Please move [here](../installer-guide/opencore-efi.md) if you're looking to make an EFI for Intel-based computers/CPUs.:::
:::info `AMD Bulldozer (15h)` and newer CPUs are supported, using the AMD Vanilla patches! By the way, AMD Laptop CPUs are the same as AMD Desktop CPUs.:::
# Requirements
## CPU Requirements
### SSE requirements
* `SSE3` is required for **all Intel versions** of macOS
* `SSSE3` is required for **all 64-bit versions** of macOS
* `SSE4` is required for `macOS 10.12 (Sierra)` up to (not including) `macOS 10.14 (Mojave)`
* `SSE4.2` is required for `macOS 10.14`+. Avoid SSE4.1-only CPUs if possible. There is a chance that might work with some hacks, see [Gathering Files > Kexts](./amd-gathering-files.md#kexts) (oh no I haven't done this yet plz don't click)
