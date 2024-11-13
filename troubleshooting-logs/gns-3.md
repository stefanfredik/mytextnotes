---
icon: circle-nodes
description: GNS 3 Errors and Troubleshoot
---

# GNS 3

## KVM Error

#### Error

```bash
Could not access KVM kernel module: Permission denied
qemu-system-x86_64: failed to initialize kvm: Permission denied
```

How To Fix

```bash
usermod -a -G kvm yourUserName 
```

The error `qemu-system-x86_64: failed to initialize kvm: Permission denied` typically occurs when QEMU (which is used by GNS3) does not have the necessary permissions to access KVM (Kernel-based Virtual Machine) on your system. This is often due to a lack of proper privileges for the user running the GNS3/QEMU process.

Here are steps to resolve this issue:

1.  **Check if KVM is enabled**: Ensure that your system supports virtualization and KVM is enabled. You can check if KVM is enabled by running:

    ```bash
    kvm-ok
    ```

    If it says "KVM acceleration can be used", then KVM is supported.
2.  **Add your user to the `kvm` group**: GNS3 and QEMU need the appropriate privileges to access KVM. Add your user to the `kvm` group by running:

    ```bash
    sudo usermod -aG kvm $USER
    ```

    After adding your user to the `kvm` group, log out and log back in for the changes to take effect.
3.  **Check if `/dev/kvm` has the correct permissions**: Ensure that the `/dev/kvm` device has the correct permissions. You can check the permissions using:

    ```bash
    ls -l /dev/kvm
    ```

    The output should show that the `kvm` group has read/write permissions. If not, you can adjust the permissions by running:

    ```bash
    sudo chmod 666 /dev/kvm
    ```
4. **Check if virtualization is enabled in BIOS**: Some systems require virtualization to be explicitly enabled in the BIOS/UEFI. Check your system’s BIOS settings to ensure that Intel VT-x or AMD-V (depending on your processor) is enabled.
5. **Restart the GNS3 or your system**: After making these changes, restart GNS3 or your entire system to ensure that the permissions take effect.

These steps should allow GNS3 to access KVM and resolve the "Permission denied" error.
