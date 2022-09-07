# Whitepaper

Here is our whitepaper on cherifying Linux:
* [On CHERIfying Linux](https://github.com/cheri-linux/.github/files/9504062/cherilinux0609.pdf)

# Linux CHERI RISCV project

Capability Hardware Enhanced RISC Instructions (CHERI) is an extension to the Instruction Set Architecture aming to provide hardware assisted memory safety to improve memory protection of unsafe languages like C. CHERI has been developed by the researchers from Cambridge Univeristy. Researchers from Cambrdge university were focusing on FreeBSD like operating system.

Broad information about CHERI can be found here: 
* https://www.cl.cam.ac.uk/research/security/ctsrd/cheri
* https://github.com/CTSRD-CHERI

This port of Linux to the CHERI (RISC-V) was developed to validate the performance and security properties of CHERI also for Linux, which is the most used OS kernel today, especially in consumer and cloud. The CHERIfication of Linux, primarily involved two main endeavors: The first was to support user-space programs and daemons compiled with CHERI. In order to achieve this, programs needed to be loaded with the awareness that they were compiled with CHERI support --- requiring  necessary changes in the program loader, acting on changes in the ELF format. The changes were needed to manage the capability-formatting of environment variables for the program. Also, the scheduler and exception handler in the kernel needed to be made CHERI-aware, i.e. to know whether a user-space process is CHERIfied or not, since register stores and restore have to account for whether capability registers need to be saved and restored during scheduling. The second endeavor was to compile the kernel proper with CHERI memory protection, i.e. to let CHERI capabilities guard the memory allocations within the kernel. The current state of this part of the CHERIfication covers only the main kernel, its  memory management code, its bootstrap for RISC-V and selected drivers (filesystem, network) that have been used for validation in QEMU and on FPGAs. This part of the work mostly included fixes for pointer (capability) provenance, i.e. to modify casts from integers to pointers which in most architectures can be done, but in CHERI, the address must be accompanied with the range of the reference turning the pointer into a capability. A few instances where kernel code modified in this way actually turned out to reference memory addresses beyond the allocation (mostly different optimizations) where also corrected. 

This open-source repository contains our CHERI-modifications to a number of different existing projects around the Linux kernel and its run-time. The project is complete enough to run the Linux kernel with a small run-time on top of the emulated QEMU RISC-V CHERI emulator, and necessary scripting (buildroot) is included to showcase this. We hope the research, CHERI and Linux communities can leverage this work for further evolving CHERI towards the fully functional, deployed secure computing architecture it deserves to become.

This set of projects are dedicated to CHERI support Linux. The current focus was on RISCV architecture, but not limited to.

# Building and running

CHERI Linux port uses [buildroot](https://buildroot.org/) tool to generate Linux system images and CHERI RISCV QEMU emulator to run them.

How to build and run:

1. Clone CHERI linux buildroot

```
git clone https://github.com/cheri-linux/buildroot.git
cd buildroot
```

2. Configure build system
```
make O=riscv64cheri qemu_riscv64cheri_defconfig
cd riscv64cheri
```
It uses [qemu_riscv64cheri_defconfig](https://github.com/cheri-linux/buildroot/blob/riscv-cheri/configs/qemu_riscv64cheri_defconfig) configuration file.

3. Build the system
```
make -j12
```
Buildroot build system will build CHERI LLVM toolchain, QEMU, GDB, Linux kernel, BBL, MUSL libc, busybox, openssh and openssl.


4. Run QEMU
```
make run
```
It uses script [build/run128_riscv.sh](https://github.com/cheri-linux/buildroot/blob/riscv-cheri/build/run128_riscv.sh) to run QEMU. Every image needed to run QEMU can be seen from the script.

5. Login

After system is booted, normal loging prompt is displayed. Use 'root' username without password.

It is also possible login via SSH to the port 7777 (non-CHERI SSH) and 7778 (CHERI SSH).

```
ssh -p 7777 root@localhost
ssh -p 7778 root@localhost
```
