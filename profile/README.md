# Linux CHERI RISCV project

Capability Hardware Enhanced RISC Instructions (CHERI) is an extension to the Instruction Set Architecture aming to provide hardware assisted memory safety to unsafe languages like C.

This set of projects are dedicated to CHERI support Linux. The current focus was on RISCV architecture, but not limited to.

More information about CHERI can be found here: 
* https://www.cl.cam.ac.uk/research/security/ctsrd/cheri
* https://github.com/CTSRD-CHERI

How to build and run:

1. git clone https://github.com/cheri-linux/buildroot.git
2. cd buildroot
3. make O=riscv64cheri qemu_riscv64cheri_defconfig
4. cd riscv64cheri
5. make -j12
6. make run
