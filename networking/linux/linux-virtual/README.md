# Virtual

Disable KVM Module

```bash
sudo rmmod kvm_amd kvm
```

Enable Virtual Box Module

```bash
 sudo modprobe vboxdrv

```

## Proxmox

#### Hitung Jumlah VM di Proxmox

```bash
#!/bin/bash

vm_count=$(qm list | awk 'NR>1' | wc -l)
container_count=$(pct list | awk 'NR>1' | wc -l)
total=$((vm_count + container_count))
ros=$(qm list | awk 'NR>1 && $2 ~ /^Homenet-/ {count++} END {print count+0}')

# Menampilkan hasil
echo "Jumlah VM: $vm_count"
echo "Jumlah Container LXC: $container_count"
echo "Total VM + Container: $total"
echo "Jumlah ROS :$ros"
```

#### Menghitung Jumlah Resource

```bash
#!/bin/bash

# Fungsi untuk mengonversi byte ke GB dengan 2 angka desimal
to_gb() {
    local bytes=$1
    awk -v b="$bytes" 'BEGIN { printf "%.2f", b/1024/1024/1024 }'
}

# Ambil informasi node (hanya satu node, jika cluster sesuaikan)
node=$(pvecm nodes | grep 'Local' | awk '{print $4}' | sed 's/(//;s/)//')
if [ -z "$node" ]; then
    node=$(hostname)
fi

# Dapatkan informasi CPU
cpu_cores=$(grep -c ^processor /proc/cpuinfo)
cpu_free=$(top -b -n1 | grep "Cpu(s)" | awk '{print $8}' | cut -d. -f1)
cpu_used_percent=$(echo "scale=2; 100 - $cpu_free" | bc)

# Dapatkan informasi RAM
mem_total=$(grep MemTotal /proc/meminfo | awk '{print $2}')
mem_free=$(grep MemFree /proc/meminfo | awk '{print $2}')
mem_buffers=$(grep Buffers /proc/meminfo | awk '{print $2}')
mem_cache=$(grep "^Cached:" /proc/meminfo | awk '{print $2}')
mem_available=$((mem_free + mem_buffers + mem_cache))

# Dapatkan informasi disk
disk_total=$(df -B1 --output=size /var/lib/vz | tail -n1)
disk_used=$(df -B1 --output=used /var/lib/vz | tail -n1)
disk_free=$((disk_total - disk_used))

# Tampilkan hasil
echo "📊 Resource Usage Proxmox Host: $node"
echo "========================================="
echo "CPU:"
echo "  🔹 Total Cores       : $cpu_cores"
echo "  🟢 Free CPU (%)      : $cpu_free%"
echo "  🔴 Used CPU (%)      : $cpu_used_percent%"
echo ""
echo "Memory (RAM):"
echo "  🔹 Total RAM         : $(to_gb $mem_total) GB"
echo "  🟢 Free RAM          : $(to_gb $mem_available) GB"
echo ""
echo "Disk (/var/lib/vz):"
echo "  🔹 Total Disk        : $(to_gb $disk_total) GB"
echo "  🟢 Free Disk         : $(to_gb $disk_free) GB"
echo "========================================="
```

### Script Bulk Migrasi VM&#x20;

Migrasi VM dengan diawali nama Homenet

```bash
#!/bin/bash

# Tujuan node
TARGET_NODE="node10"
JUMLAH_VM=10

# Ambil daftar VM ID dengan prefix "Homenet-", maksimal 5
VM_LIST=$(qm list | awk '$2 ~ /^Homenet-/ {print $1}' | head -n $JUMLAH_VM)

# Array untuk menyimpan hasil migrasi sukses
declare -a SUCCESS_VM=()

echo "🚀 Memulai migrasi maksimal 5 VM ke $TARGET_NODE..."
echo

for VMID in $VM_LIST; do
    # Ambil nama VM
    VMNAME=$(qm config $VMID | grep ^name: | awk '{print $2}')
    
    echo "🔄 Migrasi VM ID: $VMID (Nama: $VMNAME) ke $TARGET_NODE..."
    
    # Eksekusi migrasi
    if qm migrate $VMID $TARGET_NODE --online --with-local-disks; then
        echo "✅ Sukses migrasi: $VMID ($VMNAME)"
        SUCCESS_VM+=("$VMID $VMNAME")
    else
        echo "❌ Gagal migrasi: $VMID ($VMNAME)"
    fi

    echo -e "\n Menunggu 5 detik...\n"
    sleep 5
done

# Menampilkan ringkasan
echo -e "\n==============================="
echo "Ringkasan Migrasi Sukses"
echo "==============================="
for VM in "${SUCCESS_VM[@]}"; do
    echo "- $VM"
done
echo "==============================="
echo "Semua proses migrasi selesai."

```
