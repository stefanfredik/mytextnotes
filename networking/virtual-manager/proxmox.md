# Proxmox

## Menghapus  semua VM yang nonaktif&#x20;

```bash
for vmid in $(qm list | awk '$2 != "VMID" && $3 == "stopped" {print $1}'); do
    echo "Menghapus VMID $vmid..."
    qm destroy $vmid --purge
done

```

#### Penjelasan Skrip:

1. **`qm list`**\
   Perintah ini menampilkan semua VM beserta statusnya.
2. **`awk '$2 != "VMID" && $3 == "stopped" {print $1}'`**
   * Memilih hanya baris yang statusnya **stopped**.
   * Kolom pertama (`$1`) adalah **VMID** yang akan dihapus.
   * Abaikan baris header (`VMID NAME STATUS ...`).
3. **`qm destroy $vmid --purge`**
   * Menghapus VM dengan ID tertentu.
   * Opsi `--purge` memastikan bahwa file konfigurasi dan disk juga dihapus secara permanen.
4. **`echo "Menghapus VMID $vmid..."`**
   * Memberikan informasi log agar Anda tahu VM mana yang sedang dihapus.
