# Bonsai AI

## Setup

### Gambaran Arsitektur

```
VS Code
   ↓
Claude Code Extension
   ↓
Bonsai (trybons.ai)
   ↓
Model AI (Claude, GPT-5, Gemini, dll
```

#### Install Bonsai CLI

```bash
npm install -g @bonsai-ai/cli
```

Cek:

```bash
bonsai --version
```

***

#### Install Claude Code VS Code Extension

Buka **VS Code → Extensions → cari:**

```
Claude Code
```

Install extension resmi **Claude Code (by Anthropic)**.

## Buat Akun & Ambil API Key trybons.ai

1. Buka: [https://www.trybons.ai](https://www.trybons.ai/)
2. Login (GitHub / Google)
3. Masuk ke **Dashboard → API Keys**
4. **Generate API Key**
5. Simpan API key-nya

Contoh:

```
bonsai_xxxxxxxxxxxxx
```

***

## Login ke Bonsai dari Terminal

```bash
bonsai login
```

➡ Browser akan terbuka → login → authorize

***

## Konfigurasi VS Code ke Bonsai

Ada **2 metode**:

***

## METODE GLOBAL (Rekomendasi)

Berlaku untuk **semua project VS Code**

#### Windows

Buka:

```
C:\Users\USERNAME\.claude\settings.json
```

#### Linux / macOS

```
~/.claude/settings.json
```

Buat file jika belum ada.

Isi dengan:

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://go.trybons.ai",
    "ANTHROPIC_AUTH_TOKEN": "API_KEY_KAMU"
  }
}
```

Ganti:

```
API_KEY_KAMU
```

dengan API Key dari Bonsai.

***

## METODE PROJECT-ONLY

Hanya berlaku di 1 project.

Buat file:

```
.claude/settings.json
```

Isi sama:

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://go.trybons.ai",
    "ANTHROPIC_AUTH_TOKEN": "API_KEY_KAMU"
  }
}
```

***

## Restart VS Code

**WAJIB restart** supaya environment terbaca.

***

## Jalankan Claude Code via Bonsai

Di terminal:

```bash
bonsai start claude
```

Jika sukses, muncul:

```
Claude Code running with Bonsai backend
```

## Setup Optimal untuk Developer & NOC

Kalau melihat dari kebutuhan kamu (NOC + ISP + automation + dev tools), setup ideal:

```
VS Code
 + Claude Code
 + Bonsai
 + GitHub Copilot (opsional)
 + Terminal automation (bash + python)
```

##

### Integrasi VS Code

* Install ekstensi Claude Code forr VS Code
* Edit Setting Json pada extensi tersebut&#x20;
* Isi konfigurasi dengan konfigurasi berikut :&#x20;

```json
{
  "[php]": {
    "editor.defaultFormatter": "bmewburn.vscode-intelephense-client"
  },
  "[jsonc]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "remote.autoForwardPortsSource": "hybrid",

  "claude-code.environmentVariables": [
    {
      "name": "ANTHROPIC_BASE_URL",
      "value": "https://go.trybons.ai"
    },
    {
      "name": "ANTHROPIC_AUTH_TOKEN",
      "value": "API_KEY_KAMU"
    }
  ]
}
```

🔁 Ganti:

```
API_KEY_KAMU
```

dengan API key dari dashboard trybons.ai.

## 💡 Tips Pro (Multi-model Switching)

Kalau mau **switch model secara dinamis**, Bonsai mendukung:

```bash
bonsai models list
bonsai models set gpt-5
```

***

## 🚀 Rekomendasi Workflow untuk Kamu

Melihat background kamu (NOC + networking + automation):

```
VS Code
 + Claude Code
 + Bonsai
 + Python automation
 + Bash scripting
```

Ini sangat powerful untuk:

* Automation monitoring
* Konfigurasi router otomatis
* Generate script Mikrotik
* Parsing log & alerting



Berikut **panduan praktis & step-by-step cara menggunakan AgentRouter**, dari **daftar → ambil API key → pakai di VS Code (Claude Code)**. Saya buat **ringkas, runtut, dan mudah diikuti** 👍

***

## 🚀 Cara Menggunakan AgentRouter (Lengkap & Praktis)

### Alur Kerja

```
VS Code → Claude Code → AgentRouter → Claude / GPT / Gemini
```

***

## 1️⃣ Daftar & Ambil API Key

1. Buka: [**https://agentrouter.ai**](https://agentrouter.ai/)
2. Daftar (Google / GitHub)
3. Masuk ke **Dashboard**
4. Buka menu: **API Keys**
5. Klik: **Generate API Key**
6. Simpan API key

Biasanya kamu akan dapat:

> 🎁 **Free credits (sering ± $100 – $200)**

***

## 2️⃣ Ambil Base URL API

Biasanya:

```
https://api.agentrouter.ai/v1
```

(Cek di dashboard → API Documentation)

***

## 3️⃣ Setting di VS Code (Claude Code Extension)

Buka VS Code → `settings.json`

Tambahkan:

```json
"claude-code.environmentVariables": [
  {
    "name": "ANTHROPIC_BASE_URL",
    "value": "https://api.agentrouter.ai/v1"
  },
  {
    "name": "ANTHROPIC_AUTH_TOKEN",
    "value": "API_KEY_AGENTROUTER"
  }
]
```

Ganti:

```
API_KEY_AGENTROUTER
```

dengan API key milikmu.

***

## 4️⃣ Restart VS Code

Tutup → buka lagi VS Code\
(**wajib supaya setting terbaca**)

***

## 5️⃣ Tes Claude Code

1. Tekan `Ctrl + Shift + P`
2. Ketik: `Claude: Ask`
3.  Prompt:

    ```
    Buatkan script bash untuk backup otomatis server Linux.
    ```

Jika keluar jawaban → 🎉 **berhasil**

***

## 6️⃣ Pilih Model di Dashboard AgentRouter

Di dashboard AgentRouter:

Biasanya ada menu:

```
Routing → Default Model
```

Pilih:

* Claude 3.5 Sonnet (paling seimbang)
* Claude Opus (kualitas tertinggi)
* GPT-5 (reasoning kuat)
* Gemini Pro

***

## 🔥 Advanced (Opsional)

#### Auto-Fallback Routing

Misalnya:

```
Claude → GPT → Gemini
```

Kalau Claude down → otomatis pindah GPT.

***

## ⚠ Troubleshooting

#### ❌ Error 401

➡ API key salah

***

#### ❌ Tidak connect

➡ Cek:

* Base URL benar
* VS Code sudah restart

***

## 🎯 Rekomendasi Model untuk Kamu

Melihat background kamu (NOC + automation + scripting):

| Use Case           | Model             |
| ------------------ | ----------------- |
| Scripting          | Claude 3.5 Sonnet |
| Network automation | GPT-5             |
| Log analysis       | Claude Opus       |
| General            | Sonnet            |

***

## 🚀 Setup Ideal Buat Kamu

```
VS Code
 + Claude Code
 + AgentRouter
 + Bash + Python automation
```

👉 Cocok untuk:

* Auto konfigurasi MikroTik
* Monitoring ISP
* Parsing log
* Auto remediation

***

Kalau mau, saya bisa:

* Buatkan **template AI automation untuk NOC**
* Setup **workflow AI + monitoring jaringan**
* Rancang **arsitektur AI gateway internal ISP**

Tinggal bilang 😄

