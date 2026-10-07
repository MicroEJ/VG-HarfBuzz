# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [4.0.3] - 2026-10-07

### Changed

- Describe in the README how to get the HarfBuzz source and apply the Meson patch, for an integration from the GitHub repository.
- List in the README the boards this C Module is tested on and the MISRA deviations of its support files.

### Fixed

- Fix the MISRA deviations of `hb-alloc.c` to rules 10.4 and 18.4 that cppcheck reports, with no change of behavior.

## [4.0.2] - 2026-10-05

### Changed

- Align the README with the other MicroEJ C modules.
- Log the HarfBuzz heap through the log macros of the C Module MicroVG 8.0.3, without an empty line after each log.

### Fixed

- Fix the CPU fault raised when shaping complex-layout text on a target that requires aligned memory accesses.
- Fix the double free and the write through a null pointer that an exhausted HarfBuzz heap caused, and detect an overflowing `calloc` request.

## [4.0.1] - 2026-09-15

### Changed

- Enable the uncrustify and cppcheck checks on the MicroEJ source files.

## [4.0.0] - 2026-01-23

### Changed 

- Upgrade HarfBuzz from the version 10.0.1 (2024-09-24) to the version 12.3.0 (2025-12-27).
- Make the C module compatible with the new option configuration of MicroVG C module 8.0.0.

## [3.0.1] - 2025-06-25

### Changed

- Remove the `-ffreestanding` compilation option.

### Fixed

- Improve the installation procedure.

## [3.0.0] - 2024-10-18

### Changed

- Upgrade HarfBuzz from the version 4.2.1 (2022-04-24) to the version 10.0.1 (2024-09-24).
- Separate the original HarfBuzz files from MicroEJ's files.

### Migration notes

This C module is compatible with the MicroVG C module [8.0.1-9.0.0].

- Delete the directory `thirdparty/harfbuzz`.
- Remove `thirdparty/harfbuzz/inc` from your include path.
- Add `thirdparty/harfbuzz/src` to your include path.
- Remove `thirdparty/harfbuzz/lib/harfbuzz.a` and `thirdparty/harfbuzz/src/hb-alloc.c` from your build path.
- Build HarfBuzz and add the library and support files following the instructions in README.md.

## [2.0.0] - 2024-07-19

### Fixed

- Depend on the C Module MicroVG 8.0.0.
- Fix building when libc and libc++ have spaces in their path.

## [1.0.2] - 2022-09-09

### Fixed

- Fix license and copyrights.

## [1.0.1] - 2022-09-05

### Fixed

- Fix license file.

## [1.0.0] - 2022-09-02

### Fixed

- Fix thai script accents positioning issue https://github.com/harfbuzz/harfbuzz/issues/3703.

### Added

- C Module creation from Harfbuzz 4.2.1 version.

---
_Copyright 2022-2026 MicroEJ Corp. This file has been modified and/or created by MicroEJ Corp._

_ This is part of HarfBuzz, a text shaping library._

_Permission is hereby granted, without written agreement and without_
_license or royalty fees, to use, copy, modify, and distribute this_
_software and its documentation for any purpose, provided that the_
_above copyright notice and the following two paragraphs appear in_
_all copies of this software._

_IN NO EVENT SHALL THE COPYRIGHT HOLDER BE LIABLE TO ANY PARTY FOR_
_DIRECT, INDIRECT, SPECIAL, INCIDENTAL, OR CONSEQUENTIAL DAMAGES_
_ARISING OUT OF THE USE OF THIS SOFTWARE AND ITS DOCUMENTATION, EVEN_
_IF THE COPYRIGHT HOLDER HAS BEEN ADVISED OF THE POSSIBILITY OF SUCH_
_DAMAGE._

_THE COPYRIGHT HOLDER SPECIFICALLY DISCLAIMS ANY WARRANTIES, INCLUDING,_
_BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND_
_FITNESS FOR A PARTICULAR PURPOSE.  THE SOFTWARE PROVIDED HEREUNDER IS_
_ON AN "AS IS" BASIS, AND THE COPYRIGHT HOLDER HAS NO OBLIGATION TO_
_PROVIDE MAINTENANCE, SUPPORT, UPDATES, ENHANCEMENTS, OR MODIFICATIONS._\
_Build: 7E4D1F7C_
