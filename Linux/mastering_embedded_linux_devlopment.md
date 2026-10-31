## 1- Starting Out

### Licenses
- Types of Licenses
    - Copy Left : GPL, if you use in your system then need to share the source code as system become the GPL
    - Permissive: BSD and MIT, you can do whatever you want except the terms and condition of license
> LGPL- Lesser GPL, to link the libs to proprietary system
> GPL3 and LGPL3 - components in any system allowed to update by end user of system

## 2- Learning About Toolchain
- toolchain
    - set of tools - compiler, linker and runtime libs

- it used for to compile the source code of following
    - bootloader
    - kernel
    - root file system

- GNU CC (GCC), Clang and LLVM
- GCC toolchain
    - Binutils: assembler, linker
    - Compiler: GCC C compiler
    - C Library: it provides the application interface for the OS application
    > along with these we need the linux kernel headers it consists the definition and constants need to compile the user space program which needed the linux kernel directly

- CPU architecture
    - EABI
        - hardfp
        - softfp

### GNU Prefix for toolchain
- CPU: big-endian arm (`armeb`)
- Vendor: Provider of toolchain- buildroot, or just unkown
- OS: linux
- User Space: name of the user space component - musl or gnu, `musleabi or gnueabihf`

- `gcc -dumpmachine`
 
### Chosing C Library
- glibc
- musl libc
- uclibc

### Building a toolchain using crosstool-NG
- crosstool-ng
