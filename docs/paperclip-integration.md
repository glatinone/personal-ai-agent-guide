# Paperclip Integration: Orchestration CEO/Manager Pattern 🎯

Panduan setup dan dispatch task ke Paperclip untuk orchestration multi-agent dengan pattern CEO/Manager. Paperclip adalah sistem yang memungkinkan Hermes Agent mendelegasikan tugas ke worker-worker khusus secara hierarkis.

## Apa Itu Paperclip?

Paperclip adalah platform orchestration agent yang menjalankan multiple AI agents dalam satu ekosistem terkoordinasi. Dengan pattern CEO/Manager:

- **CEO Agent**: Menerima task strategis, membuat keputusan high-level, dan mendistribusikan work ke Manager
- **Manager Agent**: Mengelola eksekusi task spesifik, mengkoordinir worker teknis
- **Worker Agents**: Menjalankan task teknis seperti coding, riset, atau analisis data

Pattern ini mirip struktur organisasi perusahaan — efisien, scalable, dan mudah di-debug.

## Prasyarat

Sebelum mulai, pastikan Anda sudah:
- [Setup Hermes Agent](setup-guide.md) dan gateway aktif
- Akses ke VPS yang menjalankan Paperclip (atau setup lokal)
- Token API untuk Paperclip board

## Step 1: Setup Paperclip Board

Jika belum punya Paperclip board, deploy dulu:

```bash
# Clone repo Paperclip
git clone https://github.com/paperclip-org/paperclip.git
cd paperclip

# Deploy dengan Docker
docker-compose up -d
```

Paperclip akan running di `http://localhost:3100`.

## Step 2: Konfigurasi Koneksi Hermes-Paperclip

Tambahkan konfigurasi Paperclip ke Hermes config:

```bash
hermes config set paperclip.url "http://your-vps-ip:3100"
hermes config set paperclip.api_key "your-paperclip-token"
```

Verifikasi koneksi:

```bash
hermes paperclip status
```

Output yang diharapkan:
```
✅ Connected to Paperclip board
📊 Active agents: CEO, Manager, 3 Workers
```

## Step 3: Dispatch Task via CLI

### Pattern Dasar

Dispatch task ke CEO untuk delegasi otomatis:

```bash
/paperclip-task "Analisis kompetitor startup AI di Indonesia" --assignee CEO
```

CEO akan:
1. Breakdown task menjadi sub-tasks
2. Delegate ke Manager yang relevan
3. Manager assign ke Worker teknis
4. Aggregate results dan return summary

### Dispatch Langsung ke Manager

Untuk task yang sudah tahu jenisnya:

```bash
/paperclip-task "Buat landing page dengan React" --assignee Manager
```

Manager akan langsung handle tanpa melalui CEO.

## Step 4: Monitoring Progress

Cek status task yang sedang berjalan:

```bash
hermes paperclip tasks --status active
```

Lihat detail task tertentu:

```bash
hermes paperclip task <task-id>
```

## Step 5: Integrasi dengan Skills

Buat skill custom untuk workflow repetitif:

```markdown
---
name: competitor-analysis
description: Automated competitor research via Paperclip
---

# Competitor Analysis Skill

Workflow:
1. Accept company name from user
2. Dispatch to Paperclip CEO with context
3. Wait for completion
4. Format and present results

Usage: /competitor-analysis <company-name>
```

Install skill:

```bash
/skills install competitor-analysis
```

## Contoh Use Case

### 1. Riset Pasar Harian

```bash
/paperclip-task "Cari 5 startup AI terbaru di Southeast Asia, analisis funding dan traction" --assignee CEO
```

CEO akan delegate ke:
- Manager Research: Cari data startup
- Manager Analysis: Evaluasi traction
- Worker Writer: Compile report

### 2. Content Generation Pipeline

```bash
/paperclip-task "Buat 3 blog post tentang personal AI agent, optimasi SEO" --assignee Manager
```

Manager akan koordinir:
- Worker Research: Kumpulkan referensi
- Worker Writer: Draft konten
- Worker SEO: Optimasi keyword

### 3. Code Review Otomatis

```bash
/paperclip-task "Review PR #42 di repo github.com/user/project" --assignee Manager
```

## Troubleshooting

### Task Stuck di "Pending"

Cek log Paperclip:

```bash
docker logs paperclip-ceo --tail 50
```

Restart agent jika perlu:

```bash
hermes paperclip restart-agent CEO
```

### Koneksi Timeout

Verifikasi firewall VPS:

```bash
ufw allow 3100/tcp
```

Test konektivitas:

```bash
curl http://your-vps-ip:3100/api/health
```

### Agent Tidak Responsif

Reset agent state:

```bash
hermes paperclip reset-agent Manager
```

## Best Practices

1. **Gunakan CEO untuk task kompleks** yang butuh breakdown multi-step
2. **Direct ke Manager** untuk task yang sudah jelas scope-nya
3. **Monitor resource usage** — setiap agent consume memory
4. **Set timeout** untuk task yang mungkin lama: `--timeout 3600`
5. **Log semua dispatch** untuk audit trail

## Next Steps

Setelah integrasi berjalan, Anda bisa:
- Setup cron jobs untuk dispatch terjadwal
- Buat dashboard monitoring custom
- Integrate dengan notification system (Telegram/Discord)

Orchestration yang baik adalah kunci personal AI agent yang produktif. Selamat mencoba! 🚀