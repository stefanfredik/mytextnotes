# 9 Router

9router sudah berjalan dengan baik (uptime 41 jam, restart 0). Berikut perintah lengkap beserta konteksnya: ──────

### 📋 Perintah Terminal — 9router via PM2

#### ▶️ Jalankan pertama kali (fresh start)

```
pm2 start /home/homenet/.nvm/versions/node/v24.14.1/lib/node_modules/9router/cli.js \
  --name "9router" \
  --interpreter /home/homenet/.nvm/versions/node/v24.14.1/bin/node \
  -- --tray --skip-update -n
```

#### 🔄 Jika 9router sudah ada di PM2 (restart ulang dengan config baru)

```
pm2 delete 9router && \
pm2 start /home/homenet/.nvm/versions/node/v24.14.1/lib/node_modules/9router/cli.js \
  --name "9router" \
  --interpreter /home/homenet/.nvm/versions/node/v24.14.1/bin/node \
  -- --tray --skip-update -n
```

#### 💾 Simpan agar auto-start setelah reboot

```
pm2 save
```

#### 🔍 Verifikasi status

```
pm2 show 9router
```

────── │ Status saat ini: 9router sudah online — uptime 41 jam, restarts = 0 ✅ │ Tidak perlu dijalankan ulang.
