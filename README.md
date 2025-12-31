# FreeBSD Unofficial Port: gcc15-ada

> **Note**
> This port is unofficial and maintained separately from the FreeBSD Ports Collection.
> It is based on modifications to `/usr/ports/lang/gcc15` to enable Ada support.

By default, the official `lang/gcc15` port does not include Ada (GNAT). This port modifies the build to enable Ada and package the GNAT toolchain alongside GCC.

## Features

- GCC 15 with Ada frontend (GNAT)
- Includes all GNAT utilities
- Installs Ada runtime libraries
- Packaged as `gcc15-ada` for easy installation and distribution

## Requirements

- FreeBSD 14.3 or later
- A bootstrap Ada compiler `gcc14-ada` https://github.com/hodong-kim/gcc14-ada

## Build

Clone this repository into your FreeBSD ports tree:

```sh
cd /usr/ports/lang
git clone https://github.com/hodong-kim/gcc15-ada.git
cd gcc15-ada
```

Build and install:

```sh
# Switch to root
su

# 1. Install the bootstrap compiler
pkg add ./Downloads/gcc14-ada-14.x.x.pkg

# 2. Set environment variables for the bootstrap session
export CC=gcc14
export CXX=g++14
export CPP=cpp14
export PATH=/usr/local/bin:$PATH

# 3. Create temporary symbolic links for bootstrapping
ln -sf /usr/local/bin/gcc14        /usr/local/bin/gcc
ln -sf /usr/local/bin/gnat14       /usr/local/bin/gnat
ln -sf /usr/local/bin/gnatmake14   /usr/local/bin/gnatmake
ln -sf /usr/local/bin/gnatbind14   /usr/local/bin/gnatbind
ln -sf /usr/local/bin/gnatlink14   /usr/local/bin/gnatlink
ln -sf /usr/local/bin/gnatls14     /usr/local/bin/gnatls
ln -sf /usr/local/bin/gnatprep14   /usr/local/bin/gnatprep
ln -sf /usr/local/bin/gnatchop14   /usr/local/bin/gnatchop
ln -sf /usr/local/bin/gnatclean14  /usr/local/bin/gnatclean
ln -sf /usr/local/bin/gnatkr14     /usr/local/bin/gnatkr
ln -sf /usr/local/bin/gnatname14   /usr/local/bin/gnatname

# 4. Start the build and packaging process
make package
make reinstall
```

Alternatively, install the generated package manually:

```sh
sudo pkg add ./work/pkg/gcc15-ada-15.x.x.pkg
```

Remove `gcc14-ada` if previously installed:

```
sudo pkg remove gcc14-ada
```

## Verification

### Update Symbolic Links

After installation, you must update the symbolic links to point to the newly installed GCC 15 (GNAT 15) tools.

```
# Ensure you are still in the root session
export CC=gcc15
export CXX=g++15
export CPP=cpp15

ln -sf /usr/local/bin/gcc15        /usr/local/bin/gcc
ln -sf /usr/local/bin/gnat15       /usr/local/bin/gnat
ln -sf /usr/local/bin/gnatmake15   /usr/local/bin/gnatmake
ln -sf /usr/local/bin/gnatbind15   /usr/local/bin/gnatbind
ln -sf /usr/local/bin/gnatlink15   /usr/local/bin/gnatlink
ln -sf /usr/local/bin/gnatls15     /usr/local/bin/gnatls
ln -sf /usr/local/bin/gnatprep15   /usr/local/bin/gnatprep
ln -sf /usr/local/bin/gnatchop15   /usr/local/bin/gnatchop
ln -sf /usr/local/bin/gnatclean15  /usr/local/bin/gnatclean
ln -sf /usr/local/bin/gnatkr15     /usr/local/bin/gnatkr
ln -sf /usr/local/bin/gnatname15   /usr/local/bin/gnatname

# Exit root session
exit
```

After installation, confirm Ada support:

```sh
pkg info gcc15-ada
gnat15 --version
gcc15 -v
```

You should see `GNAT 15.x.x` and `--enable-languages=c,c++,objc,fortran,ada,jit` in the GCC configuration output.
