# Setup Guide: Instalasi Hermes Agent dari Nol 🛠️

Panduan lengkap menginstall **Hermes Agent** dari VPS kosong sampai gateway messaging aktif. Cocok untuk developer Indonesia yang ingin punya personal AI agent pribadi di infrastruktur sendiri.

## Prasyarat

Sebelum mulai, pastikan Anda punya:
- VPS Linux (Ubuntu 22.04/24.04 recommended) dengan minimal 2GB RAM
- Akses SSH ke VPS
- Akun Telegram/Discord/Slack untuk testing gateway
- Koneksi internet stabil

## Step 1: Persiapan VPS

Login ke VPS Anda via SSH:

```bash
ssh root@your-vps-ip
```

Update sistem dan install dependency dasar:

```bash
apt update && apt upgrade -y
apt install -y curl git python3 python3-pip nodejs npm
```

## Step 2: Install Hermes Agent

Jalankan installer resmi Hermes:

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
```

Installer akan otomatis:
- Download dan setup Python environment
- Install Node.js dependencies
- Setup binary Hermes di PATH
- Konfigurasi directory struktur di `~/.hermes`

Setelah instalasi selesai, reload shell:

```bash
source ~/.bashrc
```

Verifikasi instalasi:

```bash
hermes --version
```

## Step 3: Konfigurasi Awal

Jalankan setup wizard untuk konfigurasi interaktif:

```bash
hermes setup
```

Wizard akan memandu Anda melalui:
1. **Pilihan Model LLM** - Pilih provider (OpenAI, Anthropic, OpenRouter, dll)
2. **API Keys** - Masukkan API key untuk model dan tools
3. **Config Dasar** - Setting preferensi agent

Atau gunakan Nous Portal untuk setup cepat tanpa ribet kumpul API key:

```bash
hermes setup --portal
```

## Step 4: Testing CLI

Coba chat dengan Hermes via CLI:

```bash
hermes
```

Ketik pertanyaan sederhana seperti "Halo, siapa kamu?" untuk memastikan agent responsif.

Keluar dari CLI dengan `Ctrl+C` atau ketik `/exit`.

## Step 5: Setup Gateway Messaging

Ini bagian paling seru — membuat Hermes bisa diakses via Telegram, Discord, atau Slack!

### Opsi A: Telegram Gateway

1. Buat bot baru via @BotFather di Telegram
2. Copy token bot yang diberikan
3. Jalankan:

```bash
hermes gateway setup telegram
```

4. Paste token bot saat diminta
5. Start gateway:

```bash
hermes gateway start
```

### Opsi B: Discord Gateway

1. Buat aplikasi Discord di Developer Portal
2. Buat bot dan copy token
3. Invite bot ke server Anda
4. Jalankan:

```bash
hermes gateway setup discord
```

5. Paste token dan configure permissions

## Step 6: Verifikasi Gateway

Kirim pesan ke bot Anda di platform yang dipilih. Hermes harus merespons dalam beberapa detik.

Cek status gateway:

```bash
hermes gateway status
```

## Step 7: Setup Cron Jobs (Opsional)

Untuk tugas terjadwal seperti backup harian atau laporan mingguan:

```bash
hermes cron add "daily-report" --schedule "0 8 * * *" --command "Generate laporan harian"
```

## Troubleshooting

### Gateway tidak start

Cek log error:

```bash
hermes gateway logs
```

Pastikan port tidak diblokir firewall:

```bash
ufw allow 8080  # jika menggunakan custom port
```

### API Key error

Verifikasi config:

```bash
hermes config get providers
```

Update key jika perlu:

```bash
hermes config set providers.openai.api_key "sk-your-key"
```

### Memory usage tinggi

Restart agent:

```bash
hermes restart
```

## Next Steps

Setelah gateway aktif, Anda bisa:
- Integrasi dengan [Paperclip](paperclip-integration.md) untuk orchestration multi-agent
- Buat custom skills untuk workflow spesifik
- Setup multi-profile untuk use case berbeda

Selamat! Personal AI agent Anda sudah siap bekerja 24/7. 🎉