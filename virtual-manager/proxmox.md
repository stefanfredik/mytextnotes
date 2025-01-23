# Proxmox

## Menghapus  semua VM yang nonaktif&#x20;

```bash
for vmid in $(qm list | awk '$2 != "VMID" && $3 == "stopped" {print $1}'); do
    echo "Menghapus VMID $vmid..."
    qm destroy $vmid --purge
done

```
