https://public.dhe.ibm.com/ibmdl/export/pub/software/server/ibm-ai/conda/#/
https://www.dell.com/support/kbdoc/en-in/000216077/how-to-install-nvidia-driver-in-rhel

I have followed the next instructions to install the driver:


This article discusses how to install a Nvidia driver in Red Hat Enterprise Linux.
Select the correct version of Red Hat Enterprise Linux, for example Red Hat Enterprise Linux8.
Select the correct CUDA version required with the CUDA toolkit that you are going to install, for example 12.2.
The download package is an RPM, for example nvidia-driver-local-repo-rhel8-550.54.15-1.0-1.ppc64le.rpm
Install the RPM. That creates a local repository.
[root@rhel87 ~]# ls
anaconda-ks.cfg  nvidia-driver-local-repo-rhel8-550.54.15-1.0-1.ppc64le.rpm
[root@rhel87 ~]# yum localinstall ./nvidia-driver-local-repo-rhel8-550.54.15-1.0-1.ppc64le.rpm
...output skipped...
[root@rhel87 ~]# yum repolist
Install Dynamic Kernel Module Support (DKMS). DKMS is not in Red Hat Enterprise Linux. It is available in Extra Packages for Enterprise Linux (EPEL) This hyperlink is taking you to a website outside of Dell Technologies..
[root@rhel87 ~]# yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
[root@rhel87 ~]# yum install dkms
Install compilation tools and kernel header. Ensure that the Red Hat subscription is attached.
[root@rhel87 ~]# yum groupinstall "Development Tools"
[root@rhel87 ~]# yum install kernel-devel-$(uname -r)
Install the cuda-driver
[root@rhel87 ~]# yum install cuda-driver
Confirm that the driver was installed successfully.
[root@rhel87 ~]# dkms status
nvidia/550.54.15, 4.18.0-513.5.1.el8_9.ppc64le, ppc64le: installed
Reboot the server after Nvidia driver is installed.


The output you provided indicates that your system recognizes the NVIDIA GPUs (multiple Tesla V100 SXM2) and the NVIDIA driver seems to be installed correctly. However, `modprobe` is still failing to load the `nvidia` module. Here are a couple more things to try:

1. **Check for Conflicting Modules:**  The error message "No such device" might suggest another kernel module is already managing the NVIDIA device. A common culprit is the Nouveau driver, the open-source alternative to the NVIDIA driver. You can check if it's loaded by running:

```
lsmod | grep nouveau
```

If Nouveau is loaded, you'll need to blacklist it to prevent it from interfering. You can achieve this by creating a file named `nouveau.conf` at `/etc/modprobe.d`  with the following content:

```
blacklist nouveau
```

Then, unload Nouveau (if loaded) and reload the modules:

```
sudo modprobe -r nouveau
sudo depmod -a
```

2. **Rebuild the DKMS Module (Optional):**  While `dkms status` shows the NVIDIA module as installed, rebuilding it might help address any potential issues during the initial build process. Run:

```
sudo dkms build nvidia/550.54.15 -k $(uname -r)
```

3. **Verify Permissions:** Ensure you have root privileges when running `modprobe nvidia`. Use `sudo` before the command.

After trying these steps, try loading the `nvidia` module again using:

```
sudo modprobe nvidia
```

If the issue persists, check the system logs for more details:

```
dmesg | grep nvidia
```

The logs might provide clues about what's preventing the module from loading. If none of these solutions work, consider searching online for solutions specific to Red Hat 8.9 on PowerPC architecture with NVIDIA drivers. There might be known compatibility issues or additional configuration steps required for your specific setup.


The information you provided confirms that the Nouveau driver is loaded and currently managing your NVIDIA GPUs. This explains why `modprobe` fails to load the `nvidia` module since the device is already claimed by another driver.

Here's how to solve the issue and load the NVIDIA driver:

1. **Blacklist Nouveau:**

   - Create a file named `nouveau.conf` at `/etc/modprobe.d` using a text editor (e.g., `nano /etc/modprobe.d/nouveau.conf`).
   - Add the line `blacklist nouveau` to the file.
   - Save and close the file.

2. **Unload Nouveau and Reload Modules:**

   - Unload the currently loaded Nouveau driver:
     ```bash
     sudo modprobe -r nouveau
     ```
   - Reload all kernel modules to ensure the changes take effect:
     ```bash
     sudo depmod -a
     ```

3. **Load the NVIDIA Module:**

   - Now that Nouveau is blacklisted, try loading the NVIDIA module again:
     ```bash
     sudo modprobe nvidia
     ```

4. **Verify Loading (Optional):**

   - Check if the NVIDIA module is loaded by running:
     ```bash
     lsmod | grep nvidia
     ```

   If the command outputs information about the `nvidia` module, it means it's loaded successfully.

**Additional Notes:**

- Make sure you have root privileges when running the `modprobe` and `depmod` commands. Use `sudo` before them.
- The `dmesg` output indicates the NVIDIA driver is unsigned, which might cause kernel taint warnings. You can usually ignore these warnings for non-critical systems. However, if it's a concern, refer to the NVIDIA documentation for signed driver options specific to your setup.

If after following these steps, `modprobe nvidia` still fails, check the system logs again with `dmesg | grep nvidia` for any new errors. You might need to consult resources specific to Red Hat 8.9 on PowerPC with NVIDIA drivers for further troubleshooting.



The output of `lsmod | grep nouveau` shows that the `nouveau` kernel module is loaded, which is an open-source NVIDIA driver. This module is conflicting with the proprietary NVIDIA driver (`nvidia`) that you're trying to load.

The output of `dmesg | grep nvidia` shows that the NVIDIA kernel module is being loaded, but it's being tainted due to the missing signature and/or required key. This is likely because the NVIDIA driver is not signed, which is a common issue on Linux systems.

To solve the issue, you can try the following steps:

1. **Unload the nouveau kernel module**: Since the `nouveau` module is conflicting with the proprietary NVIDIA driver, you need to unload it before loading the NVIDIA driver. Run the command:
```
sudo rmmod nouveau
```
2. **Unload any other conflicting kernel modules**: Check if there are any other kernel modules that might be conflicting with the NVIDIA driver. You can use the command:
```
lsmod | grep drm
```
This will show you any kernel modules related to the DRM (Direct Rendering Manager) subsystemh might be conflicting with the NVIDIA driver. Unload any conflicting modules using the `rmmod` command.
3. **Load the NVIDIA kernel module with the `--force` option**: Run the command:
```
sudo modprobe --force nvidia
```
This will force the loading of the NVIDIA kernel module, even if it's not signed.
4. **Verify that the NVIDIA kernel module is loaded**: Run the command:
```
lsmod | grep nvidia
```
This should show you that the NVIDIA kernel module is loaded.

If you're still facing issues, you can try:

* **Blacklisting the nouveau kernel module**: Add the following line to the `/etc/modprobe.d/blacklist.confe:
```
blacklist nouveau
```
This will prevent the `nouveau` kernel module from being loaded automatically on boot.
* **Rebuilding the NVIDIA kernel module**: Run the command:
```
sudo dkms build nvidia/550.54.15 -k $(uname -r)
```
Then, try loading the NVIDIA kernel module again using the `modprobe` command.

Remember to be cautious when loading unsigned kernel modules, as they can potentially compromise the stability and security of your system.