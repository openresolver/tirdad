# Kernel patch

This branch is for a kernel patch to build Tirdad functionality right into the kernel at compile time and can be toggled with a sysctl called "kernel.ipv4.tcp_random.isn

I am working on moving my personal repo to this fork for more visibility. If you need a tested and working patch for older kernels, you can go to my personal repo called "tirdad-patch"

# This patch for kernel 7.* is in testing!!

I have not had time to test this patch in the new kernel or even verify I did the logic correctly - testing and feedback are welcome. I have a todo list which includes possibly moving the sysctl from the "sysctl_net_ipv4.c" file to lessen confusion about whether the patch toggles for IPv4 and IPv6 (it does toggle for BOTH). Do not use the current patch for long term use without removing the "pr_info" line - it is meant to print a 0 or 1 to dmesg to verify which if / else logic is being used. If you test the patch, open a terminal with `sudo dmesg -w` and generate some connections by opening a browser or running curl. I will be testing soon and updating

# tirdad
tirdad (pronounce /tērdäd/) is a kernel module to hot-patch the Linux kernel to
generate random Initial Sequence Numbers for TCP connections.

You can refer to this blog post to get familiar with the original issue:

https://bitguard.wordpress.com/?p=982

# Requirements
For the build process you will need to have the correct kernel header files
already installed on your system. These header files are usually available in
your apt repositories.

An example installation of the header files:
```
apt-get install linux-headers-`uname -r`
```
# Usage
 Compile by running:

`$make`

Run as root:

`#insmod module/tirdad.ko`

You can also disable the module with:

`#echo 0 | tee /sys/kernel/livepatch/tirdad/enabled`

`#rmmod module/tirdad.ko`

If you use the legacy version, you only need to run `rmmod` (the second
command).

After you disable it, kernel will continue to use its default algorithm to
generate initial sequence numbers.

For kernels 6.18.17 and newer, tirdad randomizes both ISN and initial TCP
timestamp offset (if enabled).
