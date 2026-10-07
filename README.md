# Overview

This C Module integrates HarfBuzz, the text-shaping library, in a VEE Port that uses the MicroVG Abstraction Layer.
The MicroVG Abstraction Layer uses it for the complex text layout of the vector fonts, described in [LLVG_FONT: Vector Font](https://docs.microej.com/en/latest/VEEPortingGuide/appendix/llapi.html#llvg-font-vector-font).
See [C Module: HarfBuzz](https://docs.microej.com/en/latest/VEEPortingGuide/vgCco.html#section-vg-c-module-harfbuzz) in the MicroEJ documentation.

This C Module is tied to the MicroEJ VG Pack: the versions it is compatible with are listed in the [compatibility](https://docs.microej.com/en/latest/VEEPortingGuide/vgReleaseNotes.html#c-modules-compatibility-version) tables of the documentation.

# Usage

1. Add the sources of this C Module to the BSP of the VEE Port, in either of these two ways:

   - From the MicroEJ repository: install the `.cco` archive of the version the VEE Port needs, as the [installation guide](https://docs.microej.com/en/latest/VEEPortingGuide/appendix/cmodules.html) describes.
     The archive holds the HarfBuzz source, with the patch of this C Module applied, in `bsp/thirdparty/harfbuzz`, and the support files in `bsp/harfbuzz_support`.
   - From GitHub: add the [GitHub repository](https://github.com/MicroEJ/VG-HarfBuzz) to the BSP of the VEE Port as a git submodule, at the tag of the version the VEE Port needs.
     The repository holds the support files only, in `<folder>/src/main/c/harfbuzz_support`, where `<folder>` is the folder of the submodule.
     Get the HarfBuzz source at the tag `12.3.0` in `<folder>/src/main/c/thirdparty/harfbuzz`, the folder the support files expect, then apply the patch of this C Module to it:

     ```sh
     git submodule add https://github.com/MicroEJ/VG-HarfBuzz.git bsp/vee/port/vg/VG-HarfBuzz
     git -C bsp/vee/port/vg/VG-HarfBuzz checkout <version>
     git clone --branch 12.3.0 https://github.com/harfbuzz/harfbuzz.git bsp/vee/port/vg/VG-HarfBuzz/src/main/c/thirdparty/harfbuzz
     git -C bsp/vee/port/vg/VG-HarfBuzz/src/main/c/thirdparty/harfbuzz apply ../../harfbuzz_support/lib/meson.patch
     ```

     A copy of these sources in the VEE Port repository works as well.

   In the next steps, `<c>` is the folder that holds `harfbuzz_support` and `thirdparty`: `bsp` for the archive, and `<folder>/src/main/c` for the GitHub repository.

2. Build HarfBuzz, as [Building HarfBuzz](#building-harfbuzz) describes.
3. Add the support files, as [Adding support files](#adding-support-files) describes.

## Building HarfBuzz

To integrate HarfBuzz into your project, it must first be built.
The directory `<c>/thirdparty/harfbuzz` contains the source code of HarfBuzz with the build files `meson.build` and `meson_options.txt` patched for compatibility with build scripts provided by MicroEJ.
The applied patch can be consulted in the file `<c>/harfbuzz_support/lib/meson.patch`.

This module provides build scripts to build the library.
As HarfBuzz depends on the Meson build system, you must first [install Meson and its dependencies](https://mesonbuild.com/Getting-meson.html).
Make sure the `meson` command is available in your shell.

You will also need FreeType to be available to your BSP.
You will typically do so by adding the FreeType C Module to your VEE Port, from the [Developer Repository](https://forge.microej.com/ui/repos/tree/General/microej-developer-repository-release/com/microej/clibrary/thirdparty/freetype) or from its [GitHub repository](https://github.com/MicroEJ/VG-FreeType).
The build scripts search the header files of FreeType in the directory `<c>/thirdparty/freetype/include`, sibling of the directory `<c>/thirdparty/harfbuzz`, which is where the `.cco` archive of the FreeType C Module installs them.

If your FreeType headers are located at a different path, as they are when FreeType comes from its GitHub repository, set `FREETYPE_DIR` in the `build.*` scripts to the FreeType folder, the one that holds `include`, or remove the option `-Dfreetype_includes` altogether to let Meson find the headers from the system locations (typically if FreeType is installed on an OS with a package manager).

Then, in a terminal, go to `<c>/harfbuzz_support/lib` and run either `build.bat` or `build.sh` depending on the shell used.
The file `libharfbuzz.a` will be created in `<c>/harfbuzz_support/lib`; you can then link your project against this file.

Before building HarfBuzz, you may want to edit `<c>/harfbuzz_support/lib/arm-armv5.txt` or the `build.*` scripts to better suit your needs.

### Building HarfBuzz for IAR

IAR cannot build HarfBuzz.
Instead, you should use the GNU toolchain (`arm-none-eabi-gcc`) to build it.

In addition to `libharfbuzz.a`, the file `_popcountsi2.o` will be added in `<c>/harfbuzz_support/lib`.
This file contains the compiled code extracted from the GCC library needed to run HarfBuzz.
You should link your project against it as well as against `libharfbuzz.a`.

An IAR project file `lib_harfbuzz.ewp` is provided for convenience.
This project merely launches `build.bat` when built.

## Adding support files

By default, HarfBuzz uses `malloc` to allocate memory.
This feature has been disabled in favor of a custom allocator.
You must compile `<c>/harfbuzz_support/src/hb-alloc.c` in your project to use this allocator.

If you wish to use the default `malloc` allocator instead, you must build HarfBuzz without the preprocessor macro `HB_CUSTOM_MALLOC` (see the `build.*` scripts).
If you do so, you must not compile `hb-alloc.c` in your project.

# Requirements

Building HarfBuzz requires:

* [Meson](https://mesonbuild.com/)
* [The GNU ARM toolchain](https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads)

# Validation

This C Module is tested with the MicroVG Abstraction Layer on the following boards:

- MIMXRT595-EVK.
- STM32U5G9J-DK2.
- Linux.

# MISRA Compliance

The support files, in `harfbuzz_support`, of this C Module are MISRA-compliant (MISRA C:2012) with some noted exceptions.
The HarfBuzz sources are not checked.
They have been verified with Cppcheck v2.19.
Here is the list of deviations from the MISRA standard:

| Deviation  | Category  | Justification |
|:----------:|:---------:|:------------- |
| Rule 8.4   | Required  | The compatible declarations are HarfBuzz's functions, which `hb-alloc.c` overrides. |
| Rule 11.5  | Advisory  | A `void *` is converted to the object type it is known to point to, for code genericity. |
| Rule 11.6  | Required  | The address of the heap goes through `void *` to become the integer that `BESTFIT_ALLOCATOR_initialize` takes. |
| Rule 17.3  | Mandatory | The implicitly declared functions are the UI Pack's and VG Pack's functions. |

# Dependencies

- The MicroVG Abstraction Layer.
- The FreeType C Module, whose headers the build of HarfBuzz reads.

# Changelog

The changes of this C Module are listed in the [public changelog of the MicroEJ VG Pack](https://docs.microej.com/en/latest/VEEPortingGuide/vgChangeLog.html).

# Source

- ``thirdparty/harfbuzz``: the HarfBuzz sources, with the patched build files, in the `.cco` archive only.
- ``harfbuzz_support/lib``: the build scripts, the Meson patch and the IAR project; the build creates the library there.
- ``harfbuzz_support/src``: the custom allocator ``hb-alloc.c``.

# Restrictions

None.

---
_Copyright 2022-2026 MicroEJ Corp. This file has been modified and/or created by MicroEJ Corp._

_ This is part of HarfBuzz, a text shaping library._

_Permission is hereby granted, without written agreement and without_\
_license or royalty fees, to use, copy, modify, and distribute this_\
_software and its documentation for any purpose, provided that the_\
_above copyright notice and the following two paragraphs appear in_\
_all copies of this software._

_IN NO EVENT SHALL THE COPYRIGHT HOLDER BE LIABLE TO ANY PARTY FOR_\
_DIRECT, INDIRECT, SPECIAL, INCIDENTAL, OR CONSEQUENTIAL DAMAGES_\
_ARISING OUT OF THE USE OF THIS SOFTWARE AND ITS DOCUMENTATION, EVEN_\
_IF THE COPYRIGHT HOLDER HAS BEEN ADVISED OF THE POSSIBILITY OF SUCH_\
_DAMAGE._

_THE COPYRIGHT HOLDER SPECIFICALLY DISCLAIMS ANY WARRANTIES, INCLUDING,_\
_BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND_\
_FITNESS FOR A PARTICULAR PURPOSE.  THE SOFTWARE PROVIDED HEREUNDER IS_\
_ON AN "AS IS" BASIS, AND THE COPYRIGHT HOLDER HAS NO OBLIGATION TO_\
_PROVIDE MAINTENANCE, SUPPORT, UPDATES, ENHANCEMENTS, OR MODIFICATIONS._\
_Build: 7E4D1F7C_
