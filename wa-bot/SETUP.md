# EastXperience WA Bot — Setup Guide
## Deploy dalam 20 menit, jalan 24/7 gratis

---

## STEP 1 — Siapkan Fonnte Token

1. Login ke **fonnte.com**
2. Hubungkan WhatsApp Business kamu (+62811342473) ke Fonnte
   - Scan QR Code dari dashboard Fonnte
   - Pastikan WhatsApp Business kamu online
3. Ke **Settings → API Token** → Copy tokennya
4. Simpan token ini, kita pakai di Step 3

---

## STEP 2 — Siapkan Anthropic API Key

1. Buka **console.anthropic.com**
2. Login / daftar (gratis, ada free credits untuk mulai)
3. Ke **API Keys → Create Key**
4. Copy key-nya (mulai dengan `sk-ant-...`)

---

## STEP 3 — Deploy ke Railway (GRATIS)

### Option A: Deploy via GitHub (recommended)

1. **Push bot ke GitHub:**
   ```bash
   cd eastxperience-wa-bot
   git init
   git add .
   git commit -m "EastXperience WA Bot"
   git remote add origin https://github.com/eastxperience/wa-bot.git
   git push -u origin main
   ```

2. **Buka railway.app** → Login dengan GitHub

3. **New Project → Deploy from GitHub repo** → pilih `wa-bot`

4. **Add Environment Variables** (di tab Variables):
   ```
   FONNTE_TOKEN = [token dari Step 1]
   ANTHROPIC_API_KEY = [key dari Step 2]
   BROADCAST_SECRET = eastxperience_broadcast_2026
   ```

5. Railway otomatis deploy. Tunggu 2-3 menit.

6. **Copy URL deployment** (misal: `https://wa-bot-production-xxxx.up.railway.app`)

### Option B: Deploy ke Render (alternatif gratis)

1. Buka **render.com** → New Web Service
2. Connect GitHub repo
3. Build Command: `npm install`
4. Start Command: `node server.js`
5. Tambahkan Environment Variables sama seperti di atas

---

## STEP 4 — Set Webhook di Fonnte

1. Login ke **fonnte.com**
2. Ke **Settings → Webhook**
3. Isi Webhook URL:
   ```
   https://[url-railway-kamu]/webhook
   ```
4. Save

---

## STEP 5 — Test Bot

Kirim pesan ke WA +62811342473 dari nomor lain:
```
Halo
```
Bot harus balas dalam <5 detik dengan greeting menu.

Test lebih lanjut:
```
1           → Waerebo pitch
waerebo     → Waerebo pitch
harga       → Claude jawab dengan pricing
booking     → Booking flow
```

---

## STEP 6 — Monitor

Railway/Render punya built-in logs. Kamu bisa lihat semua percakapan masuk.

URL health check:
```
https://[url-railway-kamu]/
```
Harus return: `{"status": "✅ EastXperience WA Bot online", ...}`

---

## BROADCAST MANUAL

Untuk kirim blast ke contact list:

```bash
curl -X POST https://[url]/broadcast \
  -H "Content-Type: application/json" \
  -d '{
    "secret": "eastxperience_broadcast_2026",
    "targets": ["628123456789", "628198765432"],
    "message": "Halo! Ada slot Waerebo tersisa minggu ini..."
  }'
```

---

## ESTIMASI BIAYA

| Service | Cost |
|---------|------|
| Railway (free tier) | GRATIS (500 jam/bulan) |
| Fonnte | ~Rp 50.000/bulan |
| Anthropic Claude Haiku | ~$0.25 per 1.000 pesan |
| **Total ~100 pesan/hari** | **~Rp 100.000–150.000/bulan** |

ROI: 1 booking Waerebo = IDR 1.850.000 → bot bayar dirinya sendiri dari 1 booking pertama.

---

## TROUBLESHOOTING

**Bot tidak reply:**
- Cek Fonnte webhook URL sudah benar
- Cek Railway/Render logs untuk error
- Pastikan WA Business terhubung dan online di Fonnte

**Bot reply tapi lambat (>10 detik):**
- Normal untuk pesan pertama (Railway cold start)
- Upgrade ke Railway paid ($5/bulan) untuk always-on

**Bot jawab tidak akurat:**
- Edit SYSTEM_PROMPT di server.js
- Tambahkan rules atau pricing yang berubah
- Re-deploy
