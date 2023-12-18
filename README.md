# CMPE-283assignment
December 17, 2023

# Project Scope
In this project, we will create a Linux Kerne module to query specific MSRs and report the features discovered for each. 

# Team Members
The team consisted of Puja Kumari(017460157) and Tonja Jean.

** Detailed Document with all the steps followed is included in the GitHub repo, filename - CMPE283Assignment_1.docx. **

**Dynamic Teamwork**
The first assignment did present several unexpected issues so the original divide of tasks did overlap as we had to resolve issues ranging from finding the best GCP VM Service, building the VM with the needed storage, and creating the kernel. Overall the project was divided according to need and we paired on building and resolving issues presented. The general divide of the project was as follows:

The first assignment did present several unexpected issues, so the original divide of tasks did overlap as we had to resolve issues ranging from finding the best GCP VM Service, building the VM with the needed storage, and creating the kernel. Overall, the project was divided according to need and we paired on building and resolving issues presented. The general divide of the project was as follows:

**Tonja Jean**

Researched system options from various platforms and hardware.

Perform a collaborative review of the requirements to determine the resources needed to complete.

Created an instance from the Google Cloud Platform and performed system requirements for memory, CPU, and nested capabilities.

Installed Linux build with needed source code, libraries, and components.

Performed system updates including the .config boot file.

Modified cmpe283-1.c  file and performed the build for the following:

  IA32_VMX_PINBASED_CTLS 0x481
  
o	IA32_VMX_PROCBASED_CTLS 0x482

o	IA32_VMX_PROCBASED_CTLS2 0x48B

Performed test and debug collaboration.
Collected and compiled documentation.
Performed final code refactoring and compilation.

**Puja Kumari**

Researched system options from various platforms and hardware.

Perform a collaborative review of the requirements to determine the resources needed to complete.

Created an instance from the Google Cloud Platform and performed system requirements for memory, CPU, and nested capabilities.

Installed Linux build with needed source code, libraries, and components.

Modified cmpe283-1.c file and performed the build for the following:


o	IA32_VMX_EXIT_CTLS 0x483

o	IA32_VMX_ENTRY_CTLS 0x484

o	IA32_VMX_PROCBASED_CTLS3 0x492

Performed test and debug collaboration.

Performed final code refactoring and compilation.

Collected and compiled documentation.





# Create the Environment
The environment requires an Intel processor that supports the creation of a nested image. The following is the environment used in this implementation.

Google Cloud Platform VM Server
Machine Type: n1-standard-8
CPU Platform: Intel Haswell
Architecture: x86/64
Boot Disk:  debian-11-bullseye-v20231115  200G

![image](https://github.com/tlavette/cmpe283assignment-1/assets/33330609/68010c75-bac7-43e5-9d76-816c8ce2a76c)


The user will need to load libraries and configurations needed to establish basic functionality of Linux environment starting with an apt-update.

# Built the Kernel from the cpuid.c
* Ensure the following files are located on the linux directory:  
cpuid.c
Makefile

* Updated the config from 5.10.0-26-cloud-amd64 to 6.7.0-rc3+

  From the linux directory, boot the latest config new kernel by copying the newest kernel configuration to the .config
cp /boot/6.7.0-rc3+ .config
make oldconfig
make prepare

![image](https://github.com/tlavette/cmpe283assignment-1/assets/33330609/545f88f2-a1e5-47a6-9d89-40417a353c95)





# Build, test, and debug the kernel modules for each MSR

The following are the MSRs queried.

* IA32_VMX_PINBASED_CTLS      0x481

* IA32_VMX_PROCBASED_CTLS     0x482

* IA32_VMX_PROCBASED_CTLS2    0x48B

* IA32_VMX_EXIT_CTLS          0x483

* IA32_VMX_ENTRY_CTLS         0x484

* IA32_VMX_PROCBASED_CTLS3    0x492


For each the cmpe283-1.c file had to be modified with the MSR and the Index



# Build

* Issued the following from the Home directory which resulted in the query running for the specified MSR Model read from the cpuid.c file.
    - make
    - sudo insmod cmpe283-1.ko
    - lsmod | grep cmpe283

![image](https://github.com/tlavette/cmpe283assignment-1/assets/33330609/003ce4df-8a33-4812-95e1-89f2652b1484)


![image](https://github.com/tlavette/cmpe283assignment-1/assets/33330609/d5a1a331-d3a5-47f0-b5ec-9e130e61a6c6)

The above steps were repeated for each MSR Model (modify cmpe283-1.c) and save the original MSR files renamed respectively.

# Debugging
The project does come with some challenges.  The following are just a few of issues encountered and the solution utilized.


**Issue #1:**

make[3]: *** No rule to make target 'debian/certs/debian-uefi-certs.pem', needed by 'certs/x509_certificate_list'.  Stop.
make[3]: *** Waiting for unfinished jobs....
  CC      certs/system_keyring.o
  CC      arch/x86/coco/core.o
![image](https://github.com/tlavette/cmpe283assignment-1/assets/33330609/34086c7c-cd0c-4ed9-93b6-057be7dc2e56)


Solution: modified the .config file
CONFIG_MODULE_SIG_KEY=""
CONFIG_MODULE_SIG_KEY_TYPE_RSA=y
CONFIG_SYSTEM_TRUSTED_KEYRING=y
CONFIG_SYSTEM_TRUSTED_KEYS="debian/certs/debian-uefi-certs.pem" < ---------------------commented out and reran
![image](https://github.com/tlavette/cmpe283assignment-1/assets/33330609/cee4d3e8-bd79-496d-acfd-9a2c964170ad)


**Issue #2:** 

As mentioned, upon building the environment, there may be libraries or tools missing. As a result, it is up to the user to identify and resolve the issues prior to being able to build the kernel.

  /bin/sh: 1: lz4c: not found
make[3]: *** [arch/x86/boot/compressed/Makefile:148: arch/x86/boot/compressed/vmlinux.bin.lz4] Error 127
make[3]: *** Deleting file 'arch/x86/boot/compressed/vmlinux.bin.lz4'
make[3]: *** Waiting for unfinished jobs....
![image](https://github.com/tlavette/cmpe283assignment-1/assets/33330609/ac1826d7-4f1d-4009-bd73-9ad34879919c)

Solution: Performed a sudo apt-get install liblz4-tool



