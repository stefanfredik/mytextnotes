---
description: GNS 3 Errors and Troubleshoot
icon: circle-nodes
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

