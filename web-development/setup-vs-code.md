# Setup VS Code

## Vue JS

#### 1. Install Library yang Dibutuhkan

Buka terminal di folder project Anda dan jalankan perintah ini:

Bash

```
npm install --save-dev eslint prettier eslint-config-prettier eslint-plugin-vue
```

* `eslint-config-prettier`: Sangat penting! Fungsinya untuk mematikan semua aturan ESLint yang bentrok dengan Prettier.
* `eslint-plugin-vue`: Aturan resmi untuk mengecek sintaks Vue.

#### 2. Konfigurasi ESLint (`.eslintrc.cjs` atau `.eslintrc.js`)

Buat atau edit file konfigurasi ESLint Anda. Pastikan prettier berada di urutan terakhir dalam array `extends`:

JavaScript

```
module.exports = {
  root: true,
  env: {
    node: true,
  },
  extends: [
    'plugin:vue/vue3-recommended', // Aturan rekomendasi Vue 3
    'eslint:recommended',          // Aturan dasar ESLint
    'prettier'                     // WAJIB TERAKHIR: Mematikan aturan ESLint yang bentrok dengan Prettier
  ],
  rules: {
    // Anda bisa menambahkan custom rule di sini
    'vue/multi-word-component-names': 'off', // Contoh: matikan peringatan nama komponen satu kata
  }
}
```

#### 3. Konfigurasi Prettier (`.prettierrc`)

Buat file `.prettierrc` untuk mengatur gaya visual yang Anda inginkan (termasuk solusi untuk masalah `CardDescription` Anda tadi):

JSON

```
{
  "semi": false,
  "singleQuote": true,
  "tabWidth": 2,
  "printWidth": 120,
  "bracketSameLine": true,
  "vueIndentScriptAndStyle": true
}
```

#### 4. Sinkronisasi dengan VS Code

Agar setiap kali Anda menekan Ctrl + S (Save) kode otomatis rapi, tambahkan pengaturan ini di `.vscode/settings.json` project Anda:

JSON

```
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
  },
  "[vue]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

***

#### Mengapa Setup Ini Bagus?

1. Tidak Ada Perang: `eslint-config-prettier` menjamin tidak ada situasi di mana ESLint menyuruh pakai tanda petik satu (`'`) tapi Prettier malah menggantinya ke petik dua (`"`).
2. Kualitas Kode: ESLint akan memberitahu Anda jika ada variabel yang tidak terpakai atau logic yang salah.
3. Visual Konsisten: Prettier akan menjaga agar indentasi dan panjang baris (termasuk tag `<CardDescription>` Anda) tetap rapi sesuai `printWidth`.

#### Tips untuk Masalah Sebelumnya

Dengan `printWidth: 120` di atas, teks di dalam `<CardDescription>` Anda kemungkinan besar akan langsung berada dalam satu baris secara otomatis karena batas karakternya sudah diperlebar.

Apakah Anda ingin saya bantu membuatkan script di `package.json` agar Anda bisa merapikan seluruh project sekaligus melalui terminal?
