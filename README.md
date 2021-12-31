Fast Page Fault
=====

The Linux kernel with multple page allocations per fault (PPF). This can improve performance for applications that incur a large amount of page faults for contiguous memory locations.

# Compilation

This repository contains the entire source code for Linux v4.17.19 with the modified page fault handler. The kernel can be compiled using standard methods. Below is a summary:

* Install the required build dependecies:
    
    ```
    sudo apt install build-essential libncurses-dev bison flex libssl-dev libelf-dev
    ```

* Navigate to this repositories directory and create the default kernel configuration file:

    ```
    make ARCH=x86_64 defconfig
    ```

* Now begin compiling the kernel:

    ```
    make -j $(nproc)
    ```
    Note: If the compilation fails due to seLinux, it can be disabled by commenting seLinux in the `.config` file:
    ```
    #CONFIG_SECURITY_SELINUX=y
    ```

* Also compile kernel modules

    ```
    sudo make modules_install
    ```

# Usage

Control of PPF is through the `proc` filesystem in Linux. To modify the number of pages to allocate per fault one can write an unsigned integer to `\proc\[PID]\ppf`. The default value is 1. Additionally, the PPF value of a process is inherrited from its parent.

One can start a process with a custom PPF value of n by running:

```
echo n > /proc/[shell PID]/ppf
```

After this, any process started from that shell will inherit a PPF value of n.
