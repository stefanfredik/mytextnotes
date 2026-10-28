---
icon: user-pilot-hair-long
---

# Hermes Agent

## Panduan Lengkap Hermes Agent

> **Versi Dokumen:** Mei 2026 | Berdasarkan Hermes Agent v0.13.0 (The Tenacity Release)\
> **Dibuat oleh:** Nous Research | **Lisensi:** MIT\
> **GitHub:** https://github.com/NousResearch/hermes-agent

***

### Daftar Isi

1. [Pengenalan Hermes Agent](https://claude.ai/chat/ee460bdf-799d-41a1-b4c8-b0f5fb6a8a01#1-pengenalan-hermes-agent)
2. [Mengapa Hermes Populer?](https://claude.ai/chat/ee460bdf-799d-41a1-b4c8-b0f5fb6a8a01#2-mengapa-hermes-populer)
3. [Instalasi & Setup Awal](https://claude.ai/chat/ee460bdf-799d-41a1-b4c8-b0f5fb6a8a01#3-instalasi--setup-awal)
4. [Fitur-Fitur Utama Hermes Agent](https://claude.ai/chat/ee460bdf-799d-41a1-b4c8-b0f5fb6a8a01#4-fitur-fitur-utama-hermes-agent)
   * 4.1 [Self-Improving Skills (Loop Belajar Mandiri)](https://claude.ai/chat/ee460bdf-799d-41a1-b4c8-b0f5fb6a8a01#41-self-improving-skills-loop-belajar-mandiri)
   * 4.2 [Sistem Memori Berlapis](https://claude.ai/chat/ee460bdf-799d-41a1-b4c8-b0f5fb6a8a01#42-sistem-memori-berlapis)
   * 4.3 [Multi-Platform Messaging Gateway](https://claude.ai/chat/ee460bdf-799d-41a1-b4c8-b0f5fb6a8a01#43-multi-platform-messaging-gateway)
   * 4.4 [Sub-Agent & Kanban Multi-Agent](https://claude.ai/chat/ee460bdf-799d-41a1-b4c8-b0f5fb6a8a01#44-sub-agent--kanban-multi-agent)
   * 4.5 [Cron Scheduling (Otomasi Terjadwal)](https://claude.ai/chat/ee460bdf-799d-41a1-b4c8-b0f5fb6a8a01#45-cron-scheduling-otomasi-terjadwal)
   * 4.6 [MCP Support (Model Context Protocol)](https://claude.ai/chat/ee460bdf-799d-41a1-b4c8-b0f5fb6a8a01#46-mcp-support-model-context-protocol)
   * 4.7 [Terminal Backends (6 Mode Deployment)](https://claude.ai/chat/ee460bdf-799d-41a1-b4c8-b0f5fb6a8a01#47-terminal-backends-6-mode-deployment)
   * 4.8 [Model-Agnostic & Provider Routing](https://claude.ai/chat/ee460bdf-799d-41a1-b4c8-b0f5fb6a8a01#48-model-agnostic--provider-routing)
   * 4.9 [Web Control & Browser Tools](https://claude.ai/chat/ee460bdf-799d-41a1-b4c8-b0f5fb6a8a01#49-web-control--browser-tools)
   * 4.10 [Persistent /goal & Checkpoints v2](https://claude.ai/chat/ee460bdf-799d-41a1-b4c8-b0f5fb6a8a01#410-persistent-goal--checkpoints-v2)
   * 4.11 [Batch Processing & Research Mode](https://claude.ai/chat/ee460bdf-799d-41a1-b4c8-b0f5fb6a8a01#411-batch-processing--research-mode)
   * 4.12 [Web Dashboard](https://claude.ai/chat/ee460bdf-799d-41a1-b4c8-b0f5fb6a8a01#412-web-dashboard)
5. [Struktur Folder OpenClaw (\~/.openclaw)](https://claude.ai/chat/ee460bdf-799d-41a1-b4c8-b0f5fb6a8a01#5-struktur-folder-openclaw-openclaw)
6. [Struktur Folder Hermes (\~/.hermes)](https://claude.ai/chat/ee460bdf-799d-41a1-b4c8-b0f5fb6a8a01#6-struktur-folder-hermes-hermes)
7. [Konfigurasi config.yaml](https://claude.ai/chat/ee460bdf-799d-41a1-b4c8-b0f5fb6a8a01#7-konfigurasi-configyaml)
8. [Sistem Skills: SKILL.md](https://claude.ai/chat/ee460bdf-799d-41a1-b4c8-b0f5fb6a8a01#8-sistem-skills-skillmd)
9. [Perintah CLI Hermes (Cheat Sheet)](https://claude.ai/chat/ee460bdf-799d-41a1-b4c8-b0f5fb6a8a01#9-perintah-cli-hermes-cheat-sheet)
10. [Migrasi dari OpenClaw ke Hermes](https://claude.ai/chat/ee460bdf-799d-41a1-b4c8-b0f5fb6a8a01#10-migrasi-dari-openclaw-ke-hermes)
11. [Studi Kasus: ISP Network Monitoring Agent](https://claude.ai/chat/ee460bdf-799d-41a1-b4c8-b0f5fb6a8a01#11-studi-kasus-isp-network-monitoring-agent)
12. [Studi Kasus: Order Management WhatsApp Bot](https://claude.ai/chat/ee460bdf-799d-41a1-b4c8-b0f5fb6a8a01#12-studi-kasus-order-management-whatsapp-bot)
13. [Keamanan & Best Practices](https://claude.ai/chat/ee460bdf-799d-41a1-b4c8-b0f5fb6a8a01#13-keamanan--best-practices)
14. [Referensi & Sumber Daya](https://claude.ai/chat/ee460bdf-799d-41a1-b4c8-b0f5fb6a8a01#14-referensi--sumber-daya)

***

### 1. Pengenalan Hermes Agent

Hermes Agent adalah **autonomous AI agent open-source** yang dikembangkan oleh **Nous Research** — lab riset AI yang juga menciptakan model Hermes, Nomos, dan Psyche.

Dirilis pada **Februari 2026**, Hermes bukan sekadar chatbot atau coding copilot. Ia adalah **agen otonom** yang:

* Hidup di server kamu (bukan di laptop saja)
* Mengingat semua yang dipelajari lintas sesi
* **Membuat dan memperbaiki skill-nya sendiri** secara otomatis
* Menjadi semakin pintar semakin lama digunakan

> "It's not a coding copilot tethered to an IDE or a chatbot wrapper around a single API. It's an autonomous agent that lives on your machine and gets smarter every day."\
> — Hermes Agent Official Documentation

Dalam waktu **7 minggu** setelah rilis, Hermes mencapai **95.000+ GitHub Stars** — lebih cepat dari LangChain, AutoGPT, atau framework agent manapun. Per Mei 2026, Hermes adalah **AI agent paling banyak digunakan di dunia** berdasarkan data OpenRouter.

***

### 2. Mengapa Hermes Populer?

#### Perbandingan dengan Tools Lain

| Fitur                    | Hermes Agent   | OpenClaw       | Claude Code | Cursor     |
| ------------------------ | -------------- | -------------- | ----------- | ---------- |
| Self-improving skills    | ✅ Otomatis     | ⚠️ Manual      | ❌           | ❌          |
| Persistent memory        | ✅              | ✅              | ❌           | ❌          |
| Multi-platform messaging | ✅ 20+ platform | ✅ 24+ platform | ❌           | ❌          |
| Model agnostic           | ✅ 200+ model   | ✅              | ⚠️ Limited  | ⚠️ Limited |
| Self-hosted              | ✅              | ✅              | ❌           | ❌          |
| Cron scheduling          | ✅              | ⚠️ Terbatas    | ❌           | ❌          |
| Multi-agent Kanban       | ✅              | ❌              | ❌           | ❌          |
| MCP Client & Server      | ✅ Keduanya     | ⚠️ Client only | ✅ Client    | ⚠️         |
| Open Source (MIT)        | ✅              | ✅              | ✅           | ❌          |

#### Keunggulan Utama

1. **Learning Loop Tertutup** — Agent belajar dari pengalaman, bukan hanya menjalankan instruksi statis
2. **Zero Lock-in** — Ganti model kapan saja tanpa ubah satu baris kode pun
3. **Runs Where You Live** — Telegram, Discord, WhatsApp, Signal — satu gateway untuk semua
4. **Privacy-First** — Semua data di mesinmu, zero telemetri, tidak ada cloud lock-in
5. **Truly Autonomous** — Bisa jalan 24/7 di VPS $5 tanpa kamu harus buka laptop

***

### 3. Instalasi & Setup Awal

#### Persyaratan Sistem

* **OS:** Linux, macOS, atau WSL2 (Windows native = early beta)
* **RAM:** Minimum 2GB (rekomendasi 4GB+)
* **Disk:** \~500MB untuk instalasi dasar
* **Dependensi:** Diinstall otomatis (Python 3.11, Node.js, ripgrep, ffmpeg)

#### Instalasi One-Line

```bash
# Linux / macOS / WSL2
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash

# Windows (PowerShell - Early Beta)
irm https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.ps1 | iex

# Android (Termux) - sama dengan Linux, installer auto-detect
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

#### Setup Wizard

```bash
# Jalankan setup wizard interaktif
hermes setup

# Wizard akan:
# 1. Auto-detect ~/.openclaw jika ada (tawarkan migrasi)
# 2. Minta pilih model provider (Nous Portal, OpenRouter, OpenAI, dll)
# 3. Setup platform messaging (Telegram, Discord, dll)
# 4. Konfigurasi API keys
```

#### Mulai Menggunakan

```bash
# Start chat interaktif
hermes

# Chat dengan pertanyaan langsung
hermes chat -q "Apa IP public server ini?"

# Buka web dashboard lokal
hermes dashboard   # Akses di http://127.0.0.1:9119

# Cek status
hermes status
```

#### Update Hermes

```bash
hermes update   # Pull latest + reinstall dependencies
```

***

### 4. Fitur-Fitur Utama Hermes Agent

#### 4.1 Self-Improving Skills (Loop Belajar Mandiri)

Ini adalah **fitur paling revolusioner** Hermes yang membedakannya dari semua agent lain.

**Cara Kerja**

```
Kamu beri task → Hermes kerjakan → Setiap ~15 tool calls:
  → Agent berhenti sejenak
  → Refleksi: "Apa yang berhasil? Apa yang tidak?"
  → Tulis SKILL.md berisi pola yang berhasil
  → Skill tersimpan di ~/.hermes/skills/
  → Task serupa berikutnya: agent LOAD skill, bukan mulai dari nol
  → Skill DIPERBARUI setiap kali agent temukan cara lebih baik
```

**Contoh Nyata**

```
Minggu 1: Task "monitoring server" butuh 20 menit
Minggu 4: Task yang sama butuh 12 menit (skill sudah dioptimasi 2x)
Minggu 6: Task yang sama butuh 8 menit (skill dioptimasi total 4x)

Prompt tidak berubah. Output sama. Yang berubah: SKILL.md di baliknya.
```

**Autonomous Skill Maintenance**

Sejak v0.13.0, Hermes menjalankan **Curator** — proses background yang setiap 7 hari:

* Review semua skill yang ada
* Hapus skill yang sudah usang/tidak relevan
* Optimalkan skill yang sering digunakan
* Merge skill yang overlapping

**Kompatibilitas agentskills.io**

Skills Hermes mengikuti **open standard agentskills.io** — skill yang kamu buat di Hermes bisa dipakai di Claude Code, Cursor, Codex CLI, dan sebaliknya tanpa modifikasi.

***

#### 4.2 Sistem Memori Berlapis

Hermes menyimpan memori dalam **3 lapisan terpisah**, masing-masing dengan lifetime dan token cost berbeda:

**Layer 1: File Markdown (Persona & Fakta Permanen)**

```
~/.hermes/SOUL.md          → Persona/kepribadian agent
~/.hermes/memories/MEMORY.md → Fakta dan konteks yang harus selalu diingat
~/.hermes/memories/USER.md   → Profil pengguna (preferensi, environment, dll)
~/.hermes/memories/IDENTITY.md → Konfigurasi identitas
```

**SOUL.md — Contoh:**

```markdown
# Persona
Kamu adalah asisten IT Kopi. Kamu ringkas, teknis, dan tidak bertele-tele.
Kamu mengerti infrastruktur jaringan ISP, OLT/ONT, ACS, dan GenieACS.
Kamu SELALU konfirmasi sebelum menjalankan perintah yang destruktif.

# Bahasa
Gunakan Bahasa Indonesia kecuali diminta lain.
Istilah teknis boleh dalam Bahasa Inggris.
```

**MEMORY.md — Contoh:**

```markdown
# Environment
- Server OLT: ZTE C320 di 192.168.1.1
- ACS: GenieACS di http://192.168.10.5:7557
- Database: PostgreSQL 16 di localhost:5432
- Stack: Laravel 12, PHP 8.3

# Konvensi
- Deploy via: git pull + composer install + php artisan migrate
- Docker: user sudah di docker group, JANGAN pakai sudo
```

**Layer 2: Skills Directory**

```
~/.hermes/skills/
```

Setiap skill = folder berisi `SKILL.md`. Token cost: **nol per sesi** kecuali skill dipanggil.

**Layer 3: Session Database (SQLite)**

```
~/.hermes/state.db   → Semua pesan dari semua sesi tersimpan di sini
                       Full-Text Search (FTS5) untuk recall cross-session
                       Agent panggil tool session_search untuk cari konteks lama
```

**Memory Providers Eksternal (Opsional)**

Hermes mendukung plugin memory provider eksternal:

* **Honcho** — Dialectic user modeling
* **Mem0** — Semantic memory
* **Hindsight** — Conversation-aware memory
* **OpenViking, RetainDB, ByteRover, Supermemory** — Berbagai backend tambahan

***

#### 4.3 Multi-Platform Messaging Gateway

Hermes berjalan sebagai **single gateway process** yang terhubung ke semua platform secara bersamaan.

**Platform yang Didukung (20+ platform di v0.13.0)**

| Kategori         | Platform                                                  |
| ---------------- | --------------------------------------------------------- |
| **Chat Populer** | Telegram, WhatsApp, Signal, Discord, Slack                |
| **Enterprise**   | Microsoft Teams, Google Chat, Mattermost, Matrix          |
| **Asia**         | DingTalk, Feishu, WeCom, Weixin (WeChat), QQ Bot, Yuanbao |
| **Email & SMS**  | Email, SMS                                                |
| **Smart Home**   | Home Assistant, BlueBubbles                               |
| **CLI**          | Terminal langsung                                         |

**Cara Kerja Gateway**

```bash
# Semua platform jalan dari satu proses
hermes gateway start

# Kamu chat di Telegram → Hermes proses di VPS → Reply ke Telegram
# Kamu chat di Discord → Hermes proses di VPS (SAMA) → Reply ke Discord
# Satu agen, banyak pintu masuk
```

**Fitur Keamanan Gateway (v0.13.0)**

* **WhatsApp**: Otomatis reject stranger (strangers ditolak by default)
* **Discord**: Role-allowlists bersifat guild-scoped (fix CVSS 8.1 cross-guild DM bypass)
* **Secret Redaction**: ON by default — semua secret di-redact dari log

***

#### 4.4 Sub-Agent & Kanban Multi-Agent

**delegate\_task (Sub-Agent Sinkron)**

Untuk task pendek yang membutuhkan konteks terpisah:

```
Hermes Utama
    └── delegate_task("cek log error di /var/log/nginx")
         → Spawn sub-agent terisolasi
         → Sub-agent: konteks sendiri, terminal sendiri, toolset terbatas
         → Selesai → Kembalikan hasil ke parent
         → Sub-agent hilang (short-lived)
```

```bash
# Dari chat:
"Tolong cek error di 3 server sekaligus: web1, web2, db1"

# Hermes otomatis spawn 3 sub-agent paralel, masing-masing ke satu server
```

**Default:** 3 sub-agent concurrent (bisa dikonfigurasi)

**Kanban Multi-Agent (v0.13.0 — Durable Board)**

Untuk workflow kompleks jangka panjang:

```
┌─────────────────────────────────────────────────────┐
│                  HERMES KANBAN BOARD                 │
│                                                      │
│  READY          IN PROGRESS      DONE               │
│  ───────        ────────────     ────               │
│  t_abc123       t_def456 🔄      t_ghi789 ✅        │
│  "Research      "Write report"   "Send email"        │
│   competitor"   (assignee:       (completed)         │
│                  writer-agent)                       │
│  t_jkl012                                           │
│  "Analyze                                           │
│   logs"                                             │
└─────────────────────────────────────────────────────┘
```

**Fitur Kanban:**

* **Heartbeat**: Worker kirim sinyal setiap N detik; mati → task reclaimed
* **Zombie Detection**: Task yang worker-nya crash otomatis direset ke READY
* **Retry Budget**: Setiap task punya batas percobaan ulang
* **Hallucination Gate**: Filter output tidak masuk akal sebelum diteruskan
* **Dependency Links**: Task B tidak bisa jalan sebelum Task A selesai
* **Auto-subscribe**: Buat task dari Telegram → kamu dapat notif otomatis saat selesai

```bash
# CLI Kanban
hermes kanban create "Research kompetitor dan buat laporan" --assignee researcher
hermes kanban list
hermes kanban show t_abc123
hermes kanban tail t_abc123          # Live log
hermes kanban comment t_abc123 "Fokus pada fitur pricing"
hermes kanban complete t_abc123

# Dashboard web: http://127.0.0.1:9119 → Drag-drop board
```

***

#### 4.5 Cron Scheduling (Otomasi Terjadwal)

Hermes punya **built-in cron scheduler** yang berjalan unattended:

```bash
# Setup cron via chat natural language
"Setiap pagi jam 07:00, cek status semua ONT offline dan kirim laporan ke Telegram"

"Setiap Senin jam 08:00, buat ringkasan ticket yang masuk minggu lalu"

"Setiap hari jam 23:00, backup database dan simpan ke /backup/daily/"
```

**Dua Mode Cron (v0.13.0)**

**Mode 1: Full Agent Run**

```yaml
# Jalankan full agent loop — bisa browsing, menulis, eksekusi kode
cron:
  - schedule: "0 7 * * *"
    task: "Cek ONT offline dan kirim laporan ke Telegram"
    skill: network-monitoring   # Attach skill tertentu
    delivery: telegram          # Platform tujuan laporan
```

**Mode 2: no\_agent (Script-Only Watchdog)**

```yaml
# Zero LLM tokens — hanya jalankan script
# Output non-empty → dikirim ke platform
# Output kosong → diam (tidak ada notif)
cron:
  - schedule: "*/5 * * * *"
    no_agent: true
    script: "/home/kopi/scripts/check_olt_status.sh"
    delivery: telegram
```

Jobs disimpan di `~/.hermes/cron/jobs.json` dan **survive restart**.

***

#### 4.6 MCP Support (Model Context Protocol)

Hermes mendukung MCP sebagai **client DAN server**:

**Sebagai MCP Client (Terima Tool dari Server Eksternal)**

```bash
# Tambah MCP server
hermes mcp add github --command npx --args "-y,@modelcontextprotocol/server-github"
hermes mcp add filesystem --url https://mcp.example.com/sse

# List, test, remove
hermes mcp list
hermes mcp test github
hermes mcp remove github
```

```yaml
# config.yaml
mcp_servers:
  github:
    command: npx
    args: ["-y", "@modelcontextprotocol/server-github"]
    env:
      GITHUB_PERSONAL_ACCESS_TOKEN: "ghp_xxx"
  zabbix:
    url: "http://localhost:8080/mcp"
```

**Sebagai MCP Server (Expose ke Agent Lain)**

Hermes bisa **di-expose** ke Claude Code, Cursor, atau agent lain sebagai MCP server — memungkinkan mereka membaca messaging state dan konteks Hermes.

```bash
# Hermes expose dirinya sebagai MCP server
hermes mcp serve --port 8090

# Di Claude Code:
# Tambahkan Hermes sebagai MCP server → Claude Code bisa akses tool Hermes
```

***

#### 4.7 Terminal Backends (6 Mode Deployment)

Hermes memisahkan **di mana kamu bicara** dengan **di mana agent berjalan**:

```
┌──────────────────────────────────────────────────────────────────┐
│  KAMU BICARA          │  HERMES BERJALAN (Terminal Backend)      │
│  ────────────         │  ──────────────────────────────────────  │
│  Telegram di HP       │  Local: di laptop/server kamu           │
│  Discord di browser   │  Docker: container terisolasi           │
│  WhatsApp di HP       │  SSH: server remote                     │
│  CLI di terminal      │  Modal: serverless (hibernate saat idle) │
│                       │  Daytona: serverless persistence         │
│                       │  Singularity: HPC/research cluster      │
│                       │  Vercel Sandbox: edge deployment         │
└──────────────────────────────────────────────────────────────────┘
```

**Kasus Penggunaan:**

* Chat dari Telegram → Agent eksekusi di VPS $5 yang selalu nyala
* Chat dari HP → Agent monitor repo GitHub di cloud (kamu tidak perlu buka laptop)
* Consulting: Pin SSH backend di VPC client → semua data tetap di tempat client

***

#### 4.8 Model-Agnostic & Provider Routing

Hermes mendukung **200+ model** via berbagai provider:

| Provider           | Model Contoh                                    |
| ------------------ | ----------------------------------------------- |
| Nous Portal        | Hermes 4, Nomos                                 |
| OpenRouter         | DeepSeek V4, Qwen 3, Llama, Gemini, Claude, GPT |
| OpenAI             | GPT-4o, o3                                      |
| Anthropic          | Claude Opus, Sonnet                             |
| NVIDIA NIM         | Nemotron                                        |
| Hugging Face       | Model open-source apapun                        |
| Ollama / llama.cpp | Model lokal                                     |
| Custom Endpoint    | API endpoint sendiri                            |

```bash
# Ganti model secara interaktif (tanpa restart, tanpa ubah config)
hermes model

# Atau via slash command
/model deepseek/deepseek-v4-pro

# Atau via config
hermes config set model deepseek/deepseek-v4-pro
```

**Provider Routing Lanjutan**

```yaml
# config.yaml — Routing berdasarkan task
providers:
  primary: openrouter          # Provider utama
  fallback: openai             # Fallback jika primary error
  
routing:
  vision_tasks: google/gemini-2.0-flash    # Vision → Gemini
  compression: deepseek/deepseek-v4-flash  # Summarize → model murah
  heavy_reasoning: anthropic/claude-opus-4 # Task berat → Claude Opus

# Credential Pools — rotasi otomatis jika rate limit
credential_pools:
  openrouter:
    - "sk-or-key-1"
    - "sk-or-key-2"
    - "sk-or-key-3"
```

***

#### 4.9 Web Control & Browser Tools

Hermes memiliki kemampuan web lengkap bawaan:

| Tool          | Fungsi                                |
| ------------- | ------------------------------------- |
| `web_search`  | Cari informasi di internet            |
| `web_extract` | Ambil konten dari URL tertentu        |
| `browser`     | Browse web dengan screenshot + vision |
| `vision`      | Analisis gambar/screenshot            |
| `image_gen`   | Generate gambar                       |
| `tts`         | Text-to-speech                        |

```bash
# Contoh penggunaan dari chat:
"Cek harga bandwidth di website provider X dan bandingkan dengan Y"
"Screenshot halaman admin panel dan jelaskan apa yang kamu lihat"
"Buat gambar topologi jaringan ISP kami"
```

***

#### 4.10 Persistent /goal & Checkpoints v2

**/goal — Agen Terkunci pada Target**

```bash
/goal "Migrasi semua ONT dari firmware lama ke firmware terbaru tanpa downtime"

# Agent:
# - Set standing objective
# - Setiap turn: cek apakah goal sudah tercapai (via auxiliary judge model)
# - Jika belum → lanjut otomatis (max 20 turn default)
# - Goal state tersimpan di session DB → survive /resume

/goal status      # Cek progress
/goal pause       # Pause sementara
/goal resume      # Lanjutkan
/goal clear       # Hapus goal
```

**Checkpoints v2 — Rollback Aman**

```bash
# Agent auto-checkpoint setiap perubahan penting
# Jika ada yang salah:
/rollback                    # Rollback ke checkpoint sebelumnya
/rollback list               # Lihat semua checkpoint tersedia
/rollback to checkpoint_id   # Rollback ke checkpoint spesifik
```

**v0.13.0 tambahkan:**

* Real pruning (tidak ada orphan shadow repos)
* Disk guardrails (tidak makan storage berlebihan)
* Post-write linting otomatis (Python, JSON, YAML, TOML) — syntax error langsung terdeteksi

***

#### 4.11 Batch Processing & Research Mode

Hermes dirancang juga untuk **training data generation**:

```bash
# Generate ribuan trajectory tool-calling secara paralel
hermes batch run \
  --prompts prompts.jsonl \
  --workers 16 \
  --output trajectories/ \
  --format sharegpt

# Export untuk fine-tuning
hermes batch export --format atropos   # Integrasi RL dengan Atropos
```

**Gunanya:**

* Generate training data untuk fine-tune model lain
* Evaluasi performa agent pada ribuan task
* Reinforcement Learning via Atropos integration

***

#### 4.12 Web Dashboard

```bash
hermes dashboard
# Buka: http://127.0.0.1:9119
```

**Fitur Dashboard:**

* 💬 Chat interface berbasis web
* 📋 Kanban board drag-and-drop untuk multi-agent
* ⚙️ Manajemen API keys & konfigurasi
* 📊 Monitor sesi aktif
* 🔧 Kelola skills (install, edit, delete)
* 📅 Kelola cron jobs
* 📈 Visualisasi task history

> Catatan: Fitur browser-based dashboard chat pane membutuhkan WSL2 di Windows (menggunakan POSIX PTY)

***

### 5. Struktur Folder OpenClaw (\~/.openclaw)

OpenClaw adalah **predecessor** Hermes yang populer selama akhir 2025. Memahami strukturnya penting untuk migrasi dan perbandingan.

```
~/.openclaw/
├── openclaw.json              # Konfigurasi utama (agents, models, channels, tools)
├── .env                       # (opsional) environment variables, API keys
└── state/
    └── agents/
        └── {agent_id}/
            └── agent/
                └── auth-profiles.json  # OAuth tokens, API keys per agent

/etc/openclaw-tenants/         # (system-level, jika multi-user)
└── {username}.env             # Secrets level system (bot tokens, gateway tokens)

~/clawd/                       # Agent workspace (path bisa dikonfigurasi)
├── SOUL.md                    # Persona/kepribadian agent
├── MEMORY.md                  # Memori kerja agent
├── USER.md                    # Profil pengguna
├── IDENTITY.md                # Konfigurasi identitas
├── AGENTS.md                  # Setup multi-agent
├── TOOLS.md                   # Dokumentasi tools
├── HEARTBEAT.md               # Konfigurasi heartbeat
├── memory/                    # Log harian, catatan riset
│   ├── 2026-01-15.md
│   ├── 2026-01-16.md
│   └── research/
│       └── competitor-analysis.md
└── skills/                    # Agent skills (runbook yang ditulis manual)
    ├── network-check.md
    ├── deploy-server.md
    └── weekly-report.md
```

#### Penjelasan File OpenClaw

| File/Folder                            | Fungsi                                                                 |
| -------------------------------------- | ---------------------------------------------------------------------- |
| `openclaw.json`                        | Konfigurasi terpusat: model provider, channel messaging, tool settings |
| `.env`                                 | API keys dan variabel lingkungan sensitif                              |
| `state/agents/{id}/auth-profiles.json` | OAuth tokens dan credentials per agent                                 |
| `~/clawd/SOUL.md`                      | Mendefinisikan persona dan cara bicara agent                           |
| `~/clawd/MEMORY.md`                    | Fakta permanen yang selalu diingat agent tiap sesi                     |
| `~/clawd/USER.md`                      | Preferensi dan profil pengguna                                         |
| `~/clawd/IDENTITY.md`                  | Konfigurasi identitas agent (nama, role, dll)                          |
| `~/clawd/AGENTS.md`                    | Setup saat menjalankan lebih dari satu agent                           |
| `~/clawd/TOOLS.md`                     | Deskripsi tools yang tersedia untuk agent                              |
| `~/clawd/HEARTBEAT.md`                 | Konfigurasi untuk fitur heartbeat/keep-alive                           |
| `~/clawd/memory/`                      | Log percakapan harian, catatan riset terstruktur                       |
| `~/clawd/skills/`                      | Skills yang ditulis **manual** oleh user atau AI yang diprompt         |

#### Perbedaan Filosofi Skills: OpenClaw vs Hermes

```
OpenClaw Skills:
  → Ditulis oleh MANUSIA (atau AI yang diprompt manual)
  → Static runbook: sama di week 1 dan week 50
  → Disimpan di ~/clawd/skills/
  → Format: markdown bebas

Hermes Skills:
  → Dibuat OTOMATIS oleh agent dari pengalaman
  → Self-improving: update setiap kali ada pola lebih baik
  → Disimpan di ~/.hermes/skills/
  → Format: SKILL.md dengan YAML frontmatter wajib
  → Compatible dengan agentskills.io standard
```

***

### 6. Struktur Folder Hermes (\~/.hermes)

```
~/.hermes/                          # HERMES_HOME (bisa diubah dengan env var)
│
├── config.yaml                     # Konfigurasi utama (non-secret)
├── .env                            # API keys & secrets (JANGAN commit ke git!)
├── auth.json                       # OAuth tokens (format nested providers)
│
├── SOUL.md                         # Persona agent
│
├── memories/                       # Layer memori permanen
│   ├── MEMORY.md                   # Fakta dan konteks kerja
│   ├── USER.md                     # Profil dan preferensi pengguna
│   ├── IDENTITY.md                 # Konfigurasi identitas
│   └── openclaw_archive/           # (Jika migrasi) Arsip memori OpenClaw lama
│
├── skills/                         # Skills directory (PRIMARY, read-write)
│   ├── devops/                     # Kategori: DevOps
│   │   ├── deploy-laravel/
│   │   │   ├── SKILL.md           # File utama skill (WAJIB ada)
│   │   │   ├── references/        # Referensi panjang (docs, tabel)
│   │   │   ├── templates/         # Template output
│   │   │   └── scripts/           # Script helper deterministic
│   │   └── backup-check/
│   │       └── SKILL.md
│   ├── network/                    # Kategori: Network
│   │   ├── olt-monitoring/
│   │   │   └── SKILL.md
│   │   └── ont-provisioning/
│   │       ├── SKILL.md
│   │       └── scripts/
│   │           └── provision.py
│   ├── research/                   # Kategori: Research
│   │   └── competitor-analysis/
│   │       └── SKILL.md
│   └── openclaw-imports/           # Skills yang diimpor dari OpenClaw
│       └── weekly-report/
│           └── SKILL.md
│
├── cron/                           # Scheduled jobs
│   └── jobs.json                   # Definisi semua cron jobs (survive restart)
│
├── state.db                        # SQLite: semua sesi + FTS5 search index
│
├── sessions/                       # Cache sesi aktif
│   └── {session_id}/
│       ├── messages.json
│       └── checkpoint/             # Checkpoints v2
│           ├── checkpoint_001/
│           └── checkpoint_002/
│
├── kanban/                         # Multi-agent Kanban board state
│   └── kanban.db                   # SQLite board (tasks, runs, subscriptions)
│
├── gateway/                        # Gateway state per platform
│   ├── telegram/
│   │   └── state.json
│   ├── discord/
│   │   └── state.json
│   └── whatsapp/
│       └── state.json
│
├── logs/                           # Log files
│   ├── hermes.log
│   ├── gateway.log
│   └── cron.log
│
├── migration/                      # (Dibuat saat migrasi dari OpenClaw)
│   └── openclaw/
│       └── {timestamp}/
│           ├── report.txt          # Laporan hasil migrasi
│           └── archive/            # File yang perlu review manual
│
└── profiles/                       # Multiple profiles (optional)
    ├── work/
    │   ├── config.yaml
    │   ├── .env
    │   ├── SOUL.md
    │   └── skills/
    └── personal/
        ├── config.yaml
        └── skills/
```

#### Penjelasan Detail Setiap Komponen

**File Konfigurasi Utama**

| Path          | Fungsi                                                      |
| ------------- | ----------------------------------------------------------- |
| `config.yaml` | Non-secret: model, routing, gateway platform, cron settings |
| `.env`        | Secret: API keys, bot tokens — JANGAN taruh di config.yaml  |
| `auth.json`   | OAuth tokens format nested providers (auto-managed)         |
| `SOUL.md`     | Persona agent — dimuat penuh ke system prompt setiap sesi   |

**Direktori memories/**

| File          | Fungsi                                               | Token Cost         |
| ------------- | ---------------------------------------------------- | ------------------ |
| `MEMORY.md`   | Fakta kerja: server IPs, conventions, completed work | Dimuat setiap sesi |
| `USER.md`     | Profil user: preferensi, identitas, kebiasaan        | Dimuat setiap sesi |
| `IDENTITY.md` | Nama agent, role, scope                              | Dimuat setiap sesi |

**Direktori skills/**

Setiap skill adalah **folder** dengan struktur:

```
skill-name/
├── SKILL.md          # WAJIB — instruksi utama + YAML frontmatter
├── references/       # OPSIONAL — tabel panjang, docs vendor, OID list
├── templates/        # OPSIONAL — template output yang dihasilkan
├── scripts/          # OPSIONAL — script helper (Python, bash)
└── assets/           # OPSIONAL — file statis (gambar, data)
```

**state.db**

SQLite database berisi:

* Semua pesan dari semua sesi (lintas platform)
* FTS5 full-text search index
* Agent query via tool `session_search`

**kanban/kanban.db**

SQLite database berisi:

* Tasks, task\_runs, dependencies
* Worker heartbeats
* Subscriptions (notifikasi per platform)

***

### 7. Konfigurasi config.yaml

```yaml
# ~/.hermes/config.yaml
# Gunakan .env untuk secrets — config.yaml untuk non-secret settings

# === MODEL & PROVIDER ===
model: "openrouter/deepseek/deepseek-v4-pro"

providers:
  openrouter:
    api_key: "${OPENROUTER_API_KEY}"   # Dari .env
    
  openai:
    api_key: "${OPENAI_API_KEY}"

# Provider routing per task type
auxiliary:
  vision: "openrouter/google/gemini-2.0-flash"
  compression: "openrouter/deepseek/deepseek-v4-flash"
  session_search: "openrouter/deepseek/deepseek-v4-flash"

# Credential pools (rotasi otomatis jika rate limit)
credential_pools:
  openrouter:
    - "${OPENROUTER_KEY_1}"
    - "${OPENROUTER_KEY_2}"

# Fallback jika primary error
fallback:
  provider: openai
  model: "gpt-4o"

# === CONTEXT & COMPRESSION ===
compression:
  threshold: 0.7          # Kompres saat 70% context window terisi
  target_ratio: 0.5       # Target: kompres ke 50% ukuran asli
  protect_last_n: 10      # Jangan kompres 10 pesan terakhir

# === SKILLS ===
skills:
  # Direktori eksternal tambahan (read-only untuk discovery)
  external_dirs:
    - ~/.agents/team-skills
    - /home/shared/company-skills
  
  # Non-secret config untuk skills tertentu
  config:
    olt-monitoring.host: "192.168.1.1"
    olt-monitoring.community: "public"

# === GATEWAY (MESSAGING PLATFORMS) ===
gateway:
  telegram:
    token: "${TELEGRAM_BOT_TOKEN}"
    allowed_users: [123456789, 987654321]  # Telegram user IDs
    
  discord:
    token: "${DISCORD_BOT_TOKEN}"
    allowed_guilds: ["guild_id_1"]
    allowed_roles: ["admin", "netops"]
    
  whatsapp:
    # WhatsApp business API config
    reject_strangers: true   # Default v0.13.0

# === CRON ===
cron:
  timezone: "Asia/Makassar"  # WITA
  jobs:
    - id: morning-report
      schedule: "0 7 * * *"
      task: "Cek status ONT offline dan kirim laporan ke Telegram"
      skill: network-monitoring
      delivery: telegram
      
    - id: backup-check
      schedule: "0 23 * * *"
      no_agent: true
      script: "/home/kopi/scripts/backup-verify.sh"
      delivery: telegram

# === TERMINAL BACKEND ===
terminal:
  backend: local   # local | docker | ssh | modal | daytona | singularity
  
  # Jika backend = docker:
  docker:
    image: "hermesagent/sandbox:latest"
    mounts:
      - "/home/kopi/projects:/workspace"
      
  # Jika backend = ssh:
  ssh:
    host: "vps.example.com"
    user: "kopi"
    key: "~/.ssh/id_rsa"

  # Environment variables yang di-pass ke sandbox
  env_passthrough:
    - "PYTHONPATH"
    - "NODE_ENV"

# === MCP SERVERS ===
mcp_servers:
  github:
    command: npx
    args: ["-y", "@modelcontextprotocol/server-github"]
    env:
      GITHUB_PERSONAL_ACCESS_TOKEN: "${GITHUB_TOKEN}"

# === SUB-AGENTS ===
subagents:
  max_concurrent: 3    # Default concurrent sub-agents
  
# === KANBAN ===
kanban:
  enabled: true
  max_workers: 5
  heartbeat_interval: 30   # detik
  reclaim_after: 120       # detik tanpa heartbeat → zombie, reclaim

# === MEMORY ===
memory:
  # External memory providers (opsional)
  providers:
    - type: honcho
      api_key: "${HONCHO_API_KEY}"

# === SECURITY ===
security:
  redact_secrets: true    # Redact secrets dari log (default ON di v0.13.0)
  prompt_injection_scan: true  # Scan skill content untuk prompt injection
```

***

### 8. Sistem Skills: SKILL.md

#### Format SKILL.md

````markdown
---
# YAML Frontmatter — WAJIB
name: olt-monitoring
description: >
  Gunakan skill ini ketika user meminta monitoring OLT ZTE C320,
  cek status ONT offline, polling SNMP OLT, atau laporan kondisi GPON.
  Aktif juga untuk: cek port ONT, bandwidth utilization, alarm OLT.

# Metadata Hermes-specific (opsional)
metadata:
  hermes:
    tags: [network, olt, gpon, snmp, zte]
    related_skills: [ont-provisioning, snmp-polling]
    
    # Secret yang dibutuhkan (di-pass ke sandbox otomatis)
    required_environment_variables:
      - name: OLT_COMMUNITY
        description: SNMP community string untuk OLT
        
    # Setting non-secret (di config.yaml → skills.config)
    config:
      - key: olt-monitoring.host
        description: IP address OLT ZTE C320
        default: "192.168.1.1"
        prompt: "IP address OLT?"
---

# OLT Monitoring — ZTE C320

## Kapan Digunakan
Gunakan skill ini untuk semua task monitoring OLT ZTE C320 via SNMP.

## Prerequisite
- OLT IP: tersedia di config (olt-monitoring.host)
- SNMP community: tersedia di env OLT_COMMUNITY
- Tools tersedia: snmpwalk, python3, atau gunakan tool web_extract

## Prosedur

### 1. Cek ONT Offline
```bash
# Gunakan script di scripts/check-ont-offline.sh
bash ~/.hermes/skills/network/olt-monitoring/scripts/check-ont-offline.sh
````

#### 2. Cek Bandwidth Utilization

Ambil OID: .1.3.6.1.4.1.3902.1082.200.10.1.1.2

#### 3. Format Laporan

Laporan harus berisi:

* Timestamp (format: WITA)
* Total ONT: aktif / offline / total
* List ONT offline: serial number, port, terakhir online
* Rekomendasi action jika ada

### Output Expected

Kirim laporan via Telegram dengan format ringkas. Jika ada ONT offline > 5, tandai sebagai HIGH PRIORITY.

````

### Tips Penulisan SKILL.md

1. **Description wajib komprehensif** — ini yang digunakan agent untuk memilih skill yang tepat
2. **Frontload prosedur** — langkah pertama di awal file, detail di references/
3. **Gunakan references/** untuk tabel OID panjang, bukan di SKILL.md langsung
4. **Satu skill = satu domain** — jangan campur monitoring dan provisioning dalam satu skill
5. **Gunakan scripts/** untuk logic deterministic — bukan prompt ke LLM

### Manajemen Skills via CLI

```bash
# List semua skills
hermes skills list

# Install dari Skills Hub (agentskills.io)
hermes skills install github-pr-workflow
hermes skills install axolotl                        # Fine-tuning LLM
hermes skills install excalidraw                     # Diagram maker

# Install dari URL langsung
hermes skills install https://example.com/my-skill/SKILL.md

# Aktifkan skill dari chat
/olt-monitoring

# Review skill
hermes skills show olt-monitoring
hermes skills edit olt-monitoring
````

***

### 9. Perintah CLI Hermes (Cheat Sheet)

#### Perintah Dasar

```bash
# Start chat interaktif
hermes
hermes chat

# Chat dengan query langsung
hermes -q "Berapa uptime server ini?"
hermes chat -q "Cek disk usage semua mount point"

# Gunakan profile tertentu
hermes -p work chat
hermes -p work -q "Cek status deployment terbaru"

# Resume sesi terakhir
hermes resume
```

#### Manajemen Konfigurasi

```bash
hermes setup                              # Wizard setup interaktif
hermes config show                        # Tampilkan config saat ini
hermes config set model "openrouter/..."  # Set config tertentu
hermes config set providers.openrouter.api_key "sk-..."
hermes status                             # Cek status semua koneksi
hermes doctor                             # Diagnosa masalah
hermes doctor --fix                       # Auto-fix masalah umum
```

#### Manajemen Model

```bash
hermes model                    # Interactive model picker
/model                          # Ganti model dari dalam chat
hermes model list               # List semua model tersedia
```

#### Skills

```bash
hermes skills list
hermes skills install <name>
hermes skills install <url>
hermes skills show <name>
hermes skills edit <name>
hermes skills remove <name>
/skills                         # List dari dalam chat
/<skill-name>                   # Aktifkan skill dari dalam chat
```

#### Memory

```bash
hermes memory show              # Tampilkan MEMORY.md + USER.md
hermes memory edit              # Edit memory
hermes memory flush             # Force-flush memory dari sesi aktif ke disk
hermes memory search "query"    # Cari di semua sesi lama
/memory                         # Dari dalam chat
```

#### Gateway (Messaging)

```bash
hermes gateway start            # Start semua platform
hermes gateway stop
hermes gateway status
hermes gateway restart          # Restart gateway (tanpa restart agent)
systemctl --user restart hermes-gateway   # Via systemd
```

#### Cron Jobs

```bash
hermes cron list                # List semua jobs
hermes cron show <id>           # Detail job tertentu
hermes cron pause <id>          # Pause job
hermes cron resume <id>         # Resume job
hermes cron run <id>            # Jalankan sekarang (manual trigger)
hermes cron remove <id>         # Hapus job
/cron list                      # Dari dalam chat
```

#### Kanban

```bash
hermes kanban list
hermes kanban create "Task description" --assignee profile-name
hermes kanban show <task_id>
hermes kanban tail <task_id>    # Live log
hermes kanban comment <id> "Komentar"
hermes kanban complete <id>
hermes kanban block <id> "Alasan diblokir"
hermes kanban archive <id>
hermes kanban stats
hermes kanban daemon            # Start kanban daemon (long-lived)
```

#### MCP

```bash
hermes mcp list
hermes mcp add <name> --url <url>
hermes mcp add <name> --command <cmd> --args <args>
hermes mcp test <name>
hermes mcp remove <name>
hermes mcp serve --port 8090    # Hermes sebagai MCP server
```

#### OpenClaw Migration

```bash
hermes claw migrate                         # Migrasi interaktif penuh
hermes claw migrate --dry-run               # Preview tanpa eksekusi
hermes claw migrate --preset user-data      # Tanpa secrets
hermes claw migrate --overwrite             # Overwrite konflik
hermes claw migrate --source /custom/path   # Path custom
hermes claw cleanup                         # Rename leftover OpenClaw dirs
```

#### Slash Commands (dari dalam chat)

```
/model          - Ganti model
/memory         - Lihat/edit memori
/skills         - Kelola skills
/cron           - Kelola cron jobs
/kanban         - Multi-agent board
/goal <text>    - Set standing goal
/goal status    - Cek progress goal
/goal pause     - Pause goal
/goal resume    - Lanjutkan goal
/goal clear     - Hapus goal
/rollback       - Rollback ke checkpoint
/resume         - Resume sesi terinterupsi
/approve        - Approve action yang diminta konfirmasi
/restart        - Restart agent
/commands       - List semua slash commands
```

***

### 10. Migrasi dari OpenClaw ke Hermes

#### Pemetaan File

| OpenClaw                     | Hermes                                            |
| ---------------------------- | ------------------------------------------------- |
| `openclaw.json`              | `config.yaml`                                     |
| `~/clawd/SOUL.md`            | `~/.hermes/SOUL.md`                               |
| `~/clawd/MEMORY.md`          | `~/.hermes/memories/MEMORY.md`                    |
| `~/clawd/USER.md`            | `~/.hermes/memories/USER.md`                      |
| `~/clawd/skills/`            | `~/.hermes/skills/openclaw-imports/`              |
| `~/clawd/memory/`            | (session history — tidak migrasi, format berbeda) |
| `.env` / `openclaw.json.env` | `~/.hermes/.env`                                  |
| `auth-profiles.json`         | `~/.hermes/auth.json`                             |

#### Langkah Migrasi

```bash
# Step 1: Backup OpenClaw dulu!
tar -czf openclaw-backup-$(date +%Y%m%d).tar.gz ~/.openclaw ~/clawd
# Simpan ke tempat aman

# Step 2: Install Hermes (jika belum)
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash

# Step 3: Jalankan setup wizard — akan auto-detect ~/.openclaw
hermes setup
# Wizard akan tawarkan migrasi otomatis

# Atau migrasi manual:
# Preview dulu
hermes claw migrate --dry-run

# Migrasi tanpa secrets (aman untuk coba dulu)
hermes claw migrate --preset user-data

# Migrasi penuh
hermes claw migrate

# Step 4: Verifikasi
hermes status
hermes memory show        # Cek MEMORY.md berhasil migrasi
hermes skills list        # Cek skills berhasil diimpor

# Step 5: Test API keys
hermes config set providers.openrouter.api_key "sk-..."
hermes status

# Step 6: Restart gateway jika ada messaging
systemctl --user restart hermes-gateway

# Step 7 (Setelah seminggu, semua OK):
hermes claw cleanup       # Rename ~/.openclaw → ~/.openclaw.pre-migration
```

#### Catatan Penting Migrasi

* Session history OpenClaw **tidak bisa dimigrasi** (format SQLite berbeda)
* SOUL.md, MEMORY.md, USER.md, skills → **migrasi langsung** (format kompatibel)
* Skills OpenClaw masuk ke `~/.hermes/skills/openclaw-imports/` — perlu review manual
* Jika MEMORY.md terlalu besar → excess entries masuk ke overflow file di migration report

***

### 11. Studi Kasus: ISP Network Monitoring Agent

#### Latar Belakang

Kamu bekerja sebagai network engineer di ISP. Kamu perlu:

* Monitor status ONT offline setiap hari
* Alert otomatis ke Telegram jika ada lonjakan ONT offline
* Laporan harian kondisi OLT
* Cek bandwidth utilization

#### Struktur Project

```
~/.hermes/skills/
└── network/
    ├── olt-monitoring/
    │   ├── SKILL.md
    │   ├── references/
    │   │   └── zte-c320-oids.md        # Tabel OID lengkap ZTE C320
    │   └── scripts/
    │       ├── check-ont-offline.py    # Script cek ONT offline via SNMP
    │       └── bandwidth-report.py     # Script bandwidth utilization
    ├── ont-provisioning/
    │   ├── SKILL.md
    │   └── scripts/
    │       └── provision-ont.py        # Script provisioning ONT baru
    └── genieacs-query/
        └── SKILL.md                    # Query parameter via GenieACS API
```

#### File: `SOUL.md`

```markdown
# Persona: NetOps Assistant

Kamu adalah asisten network operations untuk ISP kami.
Kamu ahli dalam: GPON, OLT ZTE C320, GenieACS ACS, MikroTik, SNMP.
Bahasa: Indonesia (default), teknis dalam Bahasa Inggris boleh.

# Aturan Penting
- SELALU konfirmasi sebelum restart OLT atau ubah konfigurasi
- Jika ada > 10 ONT offline tiba-tiba, anggap sebagai INSIDEN dan eskalasi
- Format waktu: WIB/WITA/WIT sesuai zona OLT
- Laporan ringkas, langsung ke poin

# Environment
- OLT: ZTE C320 (multiple unit)
- ACS: GenieACS v1.2
- Monitoring: Zabbix + custom Hermes skills
```

#### File: `memories/MEMORY.md`

```markdown
# Infrastruktur OLT

## Unit OLT Aktif
| ID | IP | Lokasi | Kapasitas |
|----|----|---------| ---------|
| OLT-01 | 192.168.1.1 | DC Jakarta | 128 ONT |
| OLT-02 | 192.168.2.1 | DC Bandung | 64 ONT |

## GenieACS
- URL: http://192.168.10.5:7557
- API endpoint: /devices, /tasks, /faults

## Threshold Alert
- ONT offline: alert jika > 5 unit dalam 15 menit
- Bandwidth: alert jika utilisasi > 85%

## Konvensi
- SNMP community: "public" (read-only), "private" (read-write — RAHASIA, pakai env)
- Log incident di: /var/log/isp-incidents/
```

#### File: `skills/network/olt-monitoring/SKILL.md`

````markdown
---
name: olt-monitoring
description: >
  Gunakan skill ini untuk monitoring OLT ZTE C320: cek ONT offline,
  bandwidth utilization, status port GPON, alarm OLT, laporan harian.
  Aktif juga untuk: SNMP query OLT, cek uptime OLT, statistik traffic.
metadata:
  hermes:
    tags: [network, olt, gpon, snmp, zte, monitoring]
    related_skills: [ont-provisioning, genieacs-query]
    required_environment_variables:
      - name: SNMP_COMMUNITY_RO
        description: SNMP read-only community string
      - name: OLT_IPS
        description: "Comma-separated list OLT IPs, e.g.: 192.168.1.1,192.168.2.1"
    config:
      - key: olt-monitoring.default_olt
        description: IP OLT default jika tidak disebutkan
        default: "192.168.1.1"
---

# OLT Monitoring — ZTE C320

## Prosedur Cek ONT Offline

1. Jalankan script Python via terminal:
```bash
python3 ~/.hermes/skills/network/olt-monitoring/scripts/check-ont-offline.py \
  --olt-ip ${OLT_IP:-olt-monitoring.default_olt} \
  --community $SNMP_COMMUNITY_RO
````

2. Parse output: list ONT dengan status offline (operStatus != 1)
3. Format laporan:
   * Total ONT: {aktif}/{total}
   * ONT Offline ({count}):
     * SN: ZTEG12345678 | Port: gpon-0/1/3 | Terakhir online: 2026-05-18 06:30
   * Status: 🟢 NORMAL / 🟡 WARNING (>5 offline) / 🔴 CRITICAL (>10 offline)

### Prosedur Laporan Bandwidth

Gunakan script bandwidth-report.py, format output CSV, simpan ke: /var/log/isp-reports/bandwidth-{tanggal}.csv

### Referensi OID

Lihat references/zte-c320-oids.md untuk tabel lengkap OID ZTE C320.

````

### File: `skills/network/olt-monitoring/scripts/check-ont-offline.py`

```python
#!/usr/bin/env python3
"""
Script cek ONT offline via SNMP untuk OLT ZTE C320
Dipanggil oleh Hermes skill olt-monitoring
"""

import argparse
import sys
from datetime import datetime

try:
    from pysnmp.hlapi import *
except ImportError:
    print("ERROR: pysnmp tidak terinstall. Jalankan: pip install pysnmp")
    sys.exit(1)

# OID ZTE C320 untuk ONT status
OID_ONT_OPER_STATUS = "1.3.6.1.4.1.3902.1082.200.10.1.1.4"
OID_ONT_SERIAL_NUM  = "1.3.6.1.4.1.3902.1082.200.10.1.1.3"
OID_ONT_DESCRIPTION = "1.3.6.1.4.1.3902.1082.200.10.1.1.2"

def check_ont_offline(olt_ip: str, community: str = "public") -> dict:
    """Query ONT status via SNMP WALK"""
    online = []
    offline = []
    
    # SNMP Walk untuk oper status
    for (errorIndication, errorStatus, errorIndex, varBinds) in nextCmd(
        SnmpEngine(),
        CommunityData(community),
        UdpTransportTarget((olt_ip, 161), timeout=5, retries=2),
        ContextData(),
        ObjectType(ObjectIdentity(OID_ONT_OPER_STATUS)),
        lexicographicMode=False
    ):
        if errorIndication:
            print(f"ERROR SNMP: {errorIndication}")
            sys.exit(1)
            
        for varBind in varBinds:
            oid = str(varBind[0])
            status = int(varBind[1])  # 1=up, 2=down
            
            # Extract index dari OID (untuk mapping ke port)
            index = oid.split(".")[-2:]
            port_info = f"gpon-0/{index[0]}/{index[1]}"
            
            if status == 1:
                online.append(port_info)
            else:
                offline.append({
                    "port": port_info,
                    "status": "offline",
                    "checked_at": datetime.now().strftime("%Y-%m-%d %H:%M")
                })
    
    return {
        "olt_ip": olt_ip,
        "total": len(online) + len(offline),
        "online": len(online),
        "offline_count": len(offline),
        "offline_list": offline,
        "severity": (
            "CRITICAL" if len(offline) > 10 else
            "WARNING" if len(offline) > 5 else
            "NORMAL"
        )
    }

def main():
    parser = argparse.ArgumentParser(description="Cek ONT Offline ZTE C320")
    parser.add_argument("--olt-ip", required=True, help="IP address OLT")
    parser.add_argument("--community", default="public", help="SNMP community")
    args = parser.parse_args()
    
    result = check_ont_offline(args.olt_ip, args.community)
    
    # Output untuk Hermes
    icon = {"CRITICAL": "🔴", "WARNING": "🟡", "NORMAL": "🟢"}[result["severity"]]
    
    print(f"{icon} Status OLT {result['olt_ip']}: {result['severity']}")
    print(f"Total ONT: {result['online']}/{result['total']} online")
    
    if result["offline_list"]:
        print(f"\nONT Offline ({result['offline_count']}):")
        for ont in result["offline_list"]:
            print(f"  - Port: {ont['port']} | Cek: {ont['checked_at']}")
    
    # Exit code berbeda untuk alert
    sys.exit(2 if result["severity"] == "CRITICAL" else 
             1 if result["severity"] == "WARNING" else 0)

if __name__ == "__main__":
    main()
````

#### File: `cron/jobs.json` (bagian relevant)

```json
{
  "jobs": [
    {
      "id": "morning-ont-check",
      "schedule": "0 7 * * *",
      "task": "Jalankan cek ONT offline untuk semua OLT dan kirim laporan ke Telegram",
      "skill": "olt-monitoring",
      "delivery": "telegram",
      "enabled": true
    },
    {
      "id": "realtime-ont-watchdog",
      "schedule": "*/10 * * * *",
      "no_agent": true,
      "script": "/home/kopi/.hermes/skills/network/olt-monitoring/scripts/check-ont-offline.py --olt-ip 192.168.1.1 --community public",
      "delivery": "telegram",
      "only_on_output": true,
      "enabled": true
    },
    {
      "id": "bandwidth-report",
      "schedule": "0 23 * * *",
      "task": "Buat laporan bandwidth utilization harian dan simpan ke /var/log/isp-reports/",
      "skill": "olt-monitoring",
      "enabled": true
    }
  ]
}
```

#### Cara Menggunakan (dari Telegram)

```
Kamu: /olt-monitoring cek semua ONT offline sekarang

Hermes: 🔴 Status OLT 192.168.1.1: WARNING
        Total ONT: 118/128 online
        
        ONT Offline (10):
        - Port: gpon-0/1/5 | Cek: 2026-05-18 09:30
        - Port: gpon-0/1/6 | Cek: 2026-05-18 09:30
        - Port: gpon-0/2/3 | Cek: 2026-05-18 09:30
        ... (7 lainnya)
        
        Rekomendasi: 10 ONT offline di area yang sama (gpon-0/1 dan gpon-0/2).
        Kemungkinan: gangguan fiber upstream. Cek port splitter fisik.

Kamu: Bisa cek apakah ini karena fiber cut atau ONT individual?

Hermes: Saya akan query GenieACS untuk cek last seen dan error code...
        [Hermes jalankan genieacs-query skill secara otomatis]
        
        Hasil query GenieACS:
        - ONT gpon-0/1/5: Last seen 2026-05-18 08:45, Error: LOS (Loss of Signal)
        - ONT gpon-0/1/6: Last seen 2026-05-18 08:45, Error: LOS
        
        → Semua offline menunjukkan LOS pada waktu yang sama (08:45).
        → Kemungkinan besar: fiber cut upstream di splitter area ini.
        → Rekomendasi: dispatch teknisi ke lokasi splitter gpon-0/1 dan gpon-0/2.
```

***

### 12. Studi Kasus: Order Management WhatsApp Bot

#### Latar Belakang

Sistem manajemen order berbasis WhatsApp untuk bisnis kecil, terintegrasi dengan:

* Google Sheets (database order)
* Fonnte API (WhatsApp messaging)
* Notifikasi otomatis ke admin

#### Struktur Project

```
~/.hermes/skills/
└── business/
    ├── order-management/
    │   ├── SKILL.md
    │   └── scripts/
    │       ├── create-order.py
    │       ├── update-status.py
    │       └── daily-summary.py
    └── payment-verification/
        ├── SKILL.md
        └── scripts/
            └── verify-payment.py
```

#### `SOUL.md` untuk Bot Bisnis

```markdown
# Persona: Asisten Toko Online

Kamu adalah asisten admin toko online. Nama kamu Hana.
Kamu ramah, efisien, dan profesional.
Bahasa: Indonesia dengan gaya semi-formal.

# Tugas Utama
1. Proses order masuk dari WhatsApp
2. Update status order
3. Verifikasi pembayaran
4. Notifikasi ke pelanggan

# Aturan
- Selalu konfirmasi detail order sebelum disimpan
- Format harga: Rp 150.000 (bukan 150000)
- Status order: PENDING → PAID → PROCESSING → SHIPPED → DONE
- Jika ragu, tanya customer service (admin)
```

#### `skills/business/order-management/SKILL.md`

````markdown
---
name: order-management
description: >
  Gunakan skill ini untuk manajemen order: buat order baru, cek status,
  update status, daftar order hari ini, laporan penjualan.
  Aktif untuk: order masuk, cek pesanan, konfirmasi pembayaran.
metadata:
  hermes:
    tags: [bisnis, order, penjualan, whatsapp]
    related_skills: [payment-verification]
    required_environment_variables:
      - name: GOOGLE_SHEETS_ID
        description: ID Google Sheets untuk database order
      - name: FONNTE_API_KEY
        description: API key Fonnte untuk WhatsApp
    config:
      - key: order-management.admin_wa
        description: Nomor WhatsApp admin
        default: "628123456789"
---

# Order Management

## Buat Order Baru

1. Ambil informasi dari pesan customer:
   - Nama customer
   - Produk yang dipesan
   - Jumlah
   - Alamat pengiriman
   - Nomor WhatsApp

2. Hitung total:
   - Cek harga produk di sheets "Products"
   - Total = harga × jumlah + ongkir

3. Simpan ke Google Sheets:
```python
python3 ~/.hermes/skills/business/order-management/scripts/create-order.py \
  --name "{customer_name}" \
  --product "{product}" \
  --qty {qty} \
  --address "{address}" \
  --phone "{phone}"
````

4. Kirim konfirmasi ke customer via WhatsApp (Fonnte): Template konfirmasi ada di templates/order-confirmation.txt

### Update Status Order

```python
python3 ~/.hermes/skills/business/order-management/scripts/update-status.py \
  --order-id {order_id} \
  --status {new_status} \
  --note "{optional_note}"
```

Status valid: PENDING, PAID, PROCESSING, SHIPPED, DONE, CANCELLED

### Laporan Harian

Jalankan daily-summary.py dan kirim ke WhatsApp admin.

````

### Konfigurasi Cron untuk Order Management

```json
{
  "id": "daily-order-summary",
  "schedule": "0 20 * * *",
  "task": "Buat laporan ringkasan order hari ini dan kirim ke admin WhatsApp",
  "skill": "order-management",
  "delivery": "whatsapp",
  "enabled": true
}
````

#### Contoh Interaksi

```
[Customer WhatsApp → Hermes Bot]

Customer: Halo, mau order kaos polo size L warna hitam 2 pcs

Hana: Halo! Terima kasih sudah menghubungi kami 😊

      Saya konfirmasi pesanan Anda:
      📦 Produk: Kaos Polo Size L Hitam
      🔢 Jumlah: 2 pcs
      💰 Harga: Rp 150.000 × 2 = Rp 300.000
      🚚 Ongkir: Rp 25.000 (estimasi)
      💵 Total: Rp 325.000
      
      Boleh saya minta nama dan alamat pengiriman lengkap?

Customer: Nama: Budi Santoso
         Alamat: Jl. Melati No. 5, Denpasar Bali 80234

Hana: Terima kasih Pak Budi! Order Anda sudah dicatat 📝

      📋 Detail Order:
      No. Order: #ORD-2026-0518-001
      Nama: Budi Santoso
      Produk: Kaos Polo L Hitam × 2
      Total: Rp 325.000
      Alamat: Jl. Melati No. 5, Denpasar Bali 80234
      
      Silakan transfer ke:
      🏦 BCA: 1234567890 a.n. Toko Online Kami
      
      Setelah transfer, kirimkan bukti pembayaran ke sini ya!

[Hermes otomatis notif admin WhatsApp: "Order baru #ORD-2026-0518-001 dari Budi Santoso - Rp 325.000"]
```

***

### 13. Keamanan & Best Practices

#### Risiko yang Perlu Diketahui

**1. Memory/Skill Poisoning**

Ini adalah **risiko terbesar** Hermes:

```
Skenario:
1. Hermes browsing website berbahaya dalam task
2. Website mengandung prompt injection tersembunyi
3. Injection masuk ke skill yang dibuat Hermes
4. Skill tersimpan di disk sebagai file Markdown
5. Task berikutnya yang serupa → agent load skill "berbahaya" ini
6. Injection dieksekusi sebagai "trusted context"
```

**Mitigasi:**

```bash
# Review SEMUA skill baru yang dibuat agent
ls -lt ~/.hermes/skills/   # Lihat skill terbaru
cat ~/.hermes/skills/auto-generated/*/SKILL.md  # Baca isinya

# Aktifkan prompt injection scan (default ON di v0.13.0)
hermes config set security.prompt_injection_scan true

# Review skill sebelum digunakan di production
# Treat skill files seperti code: version control, review, audit
```

**2. VPS Credentials**

```
$5 VPS menjalankan Hermes dengan credentials KAMU:
- API keys ada di .env
- GitHub token ada di .env
- Database password ada di .env
```

**Mitigasi:**

* Gunakan **principle of least privilege** untuk semua credentials
* Pisahkan credentials: satu API key khusus untuk Hermes dengan scope terbatas
* Gunakan **Docker backend** untuk isolasi
* Enable `security.redact_secrets: true` (default ON)

**3. Gateway Security**

```bash
# WhatsApp: reject strangers (default ON di v0.13.0)
gateway:
  whatsapp:
    reject_strangers: true

# Telegram: whitelist user ID
gateway:
  telegram:
    allowed_users: [123456789]  # Hanya ID ini yang bisa akses

# Discord: guild-scoped role allowlist
gateway:
  discord:
    allowed_guilds: ["your_guild_id"]
    allowed_roles: ["admin"]
```

#### Best Practices

```markdown
## Untuk Production

1. **Gunakan Docker backend** untuk isolasi agent dari sistem host
2. **Pisahkan profiles**: profile "work" dan "personal" dengan .env berbeda
3. **Backup rutin**: `tar -czf hermes-backup-$(date +%Y%m%d).tar.gz ~/.hermes`
4. **Review skill baru** sebelum masuk ke cycle berikutnya
5. **Monitor log**: `tail -f ~/.hermes/logs/hermes.log`
6. **Update rutin**: `hermes update` untuk patch security

## Untuk Development

1. **Dry-run dulu**: test skill dengan query yang safe
2. **Gunakan no_agent cron** untuk watchdog sederhana (hemat token)
3. **Set context files**: drop `.hermes/` di project root untuk context otomatis
4. **Version control skills**: taruh ~/.hermes/skills/ di git private repo
5. **Gunakan credential pools** jika multi-user team
```

#### Environment Variables Penting

```bash
# ~/.hermes/.env (JANGAN commit ke git!)

# Model Providers
OPENROUTER_API_KEY=sk-or-...
OPENAI_API_KEY=sk-...
ANTHROPIC_API_KEY=sk-ant-...

# Messaging Platforms
TELEGRAM_BOT_TOKEN=123456:ABC...
DISCORD_BOT_TOKEN=...
WHATSAPP_TOKEN=...

# Network/Infrastructure (custom)
SNMP_COMMUNITY_RO=public
SNMP_COMMUNITY_RW=private123  # RAHASIA!
OLT_IPS=192.168.1.1,192.168.2.1
GENIEACS_URL=http://192.168.10.5:7557
GENIEACS_API_KEY=...

# Business Integration
GOOGLE_SHEETS_ID=1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74OgVE2upms
FONNTE_API_KEY=abc123...
```

***

### 14. Referensi & Sumber Daya

#### Dokumentasi Resmi

| Sumber            | URL                                          |
| ----------------- | -------------------------------------------- |
| Dokumentasi Resmi | https://hermes-agent.nousresearch.com/docs   |
| GitHub Repository | https://github.com/NousResearch/hermes-agent |
| Skills Hub        | https://agentskills.io                       |
| Landing Page      | https://hermes-agent.nousresearch.com        |

#### Dokumentasi Machine-Readable

```bash
# Untuk dimasukkan ke LLM context
curl https://hermes-agent.nousresearch.com/docs/llms.txt        # ~17KB, index semua halaman
curl https://hermes-agent.nousresearch.com/docs/llms-full.txt   # ~1.8MB, semua konten
```

#### File Penting untuk Dibaca

| Topik            | Path di Docs                       |
| ---------------- | ---------------------------------- |
| Instalasi        | /docs/getting-started/installation |
| Konfigurasi      | /docs/user-guide/configuration     |
| Skills System    | /docs/user-guide/features/skills   |
| Memory System    | /docs/user-guide/features/memory   |
| Kanban           | /docs/user-guide/features/kanban   |
| MCP Integration  | /docs/user-guide/features/mcp      |
| Gateway Setup    | /docs/user-guide/gateway           |
| Migrasi OpenClaw | /docs/guides/migrate-from-openclaw |
| CLI Reference    | /docs/reference/cli                |

#### Komunitas

* **Discord Nous Research**: Tempat diskusi resmi, desain RFC baru
* **GitHub Issues**: Bug reports dan feature requests
* **GitHub Discussions**: Pertanyaan umum dan sharing
* **agentskills.io**: Marketplace skills komunitas

#### Changelog Penting

| Versi   | Tanggal    | Fitur Utama                                                                               |
| ------- | ---------- | ----------------------------------------------------------------------------------------- |
| v0.13.0 | Mei 2026   | Kanban multi-agent, /goal, Checkpoints v2, Google Chat (platform ke-20), 8 security fixes |
| v0.12.x | April 2026 | Autonomous skill curator, provider routing                                                |
| v0.10.0 | Maret 2026 | Nous Portal integration, pluggable memory providers                                       |
| v0.1.0  | Feb 2026   | Initial release                                                                           |

***

_Panduan ini dibuat berdasarkan dokumentasi resmi Hermes Agent v0.13.0 (The Tenacity Release) dan berbagai sumber komunitas. Karena Hermes berkembang sangat cepat (\~100 commit per hari), selalu cek dokumentasi resmi untuk informasi terbaru._

_"The agent that grows with you" — Nous Research_
