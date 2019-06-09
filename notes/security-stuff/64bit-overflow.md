---
layout: default
title: 64-bit overflow
parent: security stuff
#has_children: true
nav_order: 2
---

1. TOC
{:toc}


Registers:
- **RAX, RBX, RCX,RDX,RSI and RDI**
- **RIP, RBP, RSP**(*instruction pointer, base pointer, stack pointer*)
  - Pointers are 8-bytes wyde
- Maximum address size of **0x00007FFFFFFFFFFF**

The objective of a buffer overflow attack is to calculate the $RIP offset and change it by injecting a memory address pointing to our shellcode.


## ASLR
When develoing buffers overflows on modern systems, it is important to understand ASLR. 
- ASLR (Address Space Layout Randomizatin)
 - The ASLR is an exploit mitigation technique, which randomly moves certains areas of an executable program. 
 - introduced into linux kernel 2.6.12 in 2005
 - Linux ASLR can be configured though `/proc/sys/kernel/randomize_va_space`. The following values are supported: 
   - `0` - No randomization. Everything is static. :) 
   - `1` - Conservative randomization (mmap base, stack and VDSO)
   - `2` - Full Randomization (also enables heap randomization)
 ## Configure randomize_va_space
 ###  How to disable ASLR temporarily (system-wide)
 ```bash
 # method one
 echo 0 > /proc/sys/kernel/randomize_va_space
 # method two
 sysctl -w kernel.randomize_va_space=0
 ```  
 ### How to disable ASLR locally for specific program
 This involves manipulating personality flags with the `setarch` command. 
 ```bash
 # disable ASLR for specific program
 setarch 'uname -m' -R /tmp/mybinary

 # disable ASLR on a new shell in addition to all child processesstarted from said shell
 setarch 'uname -m' -R /bin/bash
 ```  
  - The `-R` option disables the randomization of the virtual address space by turning on ADDR_NO_RANDOMIZE. This allows programs to disable ASLR and run without any randomization
  - Keep in mind that compilers also have protection mechanisms. Use the following to disable stack protection during compilation:
  ```bash
  gcc -fno-stack-protector -z execstack -o myprogram myprogram.c
  ```
  - `'fno-stack-protector` compiles without stack canaries
  - `-z execstack` allows you to execute code on the stack

---

## Resources and More
{: .no_toc }
### Links
{: .no_toc }
[ASLR Linux - How Effective Is It?](https://securityetalii.es/2013/02/03/how-effective-is-aslr-on-linux-systems/)
#### Reference
{: .no_toc }
[ASLR - Wikipedia](https://en.wikipedia.org/wiki/Address_space_layout_randomization<Paste>)
[ASLR - randomize_va_space in the linux kernel](https://www.kernel.org/doc/Documentation/sysctl/kernel.txt)




