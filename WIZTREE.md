# libsmb2 as used by WizTreeNix and WizTreeMac

This is a fork of [sahlberg/libsmb2](https://github.com/sahlberg/libsmb2) (LGPL-2.1, see `LICENCE-LGPL-2.1.txt` and
`COPYING`). The `wiztree` branch is upstream commit `84705fd70d2a31f70ff19d13b9aeb61ec706ca12` plus one change, marked
`WizTree patch` in the source:

- `struct smb2_stat_64` (`include/smb2/libsmb2.h`) gains `uint64_t smb2_alloc_size`, the allocated size on disk.
- `smb2_readdir` (`lib/libsmb2.c`) fills it from the directory listing's AllocationSize, which the stock library discards.

WizTree shows allocated sizes as well as file sizes, so its SMB scans need that value. Nothing else is changed.

WizTreeNix and WizTreeMac load this library as a separate shared library (not linked into the program), so a modified
build of it can be dropped in place of the one they ship. To build it: `cmake -S . -B build -DBUILD_SHARED_LIBS=ON
-DENABLE_GSSAPI=OFF -DENABLE_LIBKRB5=OFF && cmake --build build`.
