# AutoLaris Payment Gateway — API Docs

Dokumentasi tidak resmi untuk fitur **Create Payment (Payment Gateway)** pada **AutoLaris H2H API** — pembuatan tagihan pembayaran melalui **Virtual Account**, **QRIS**, dan **E-Wallet DANA**, beserta alur callback notifikasi pembayaran.

> Disusun dari [Postman Documenter AutoLaris H2H API](https://documenter.getpostman.com/view/25938923/2sB2iwFuwz) untuk memudahkan integrasi partner. Bukan dokumentasi resmi AutoLaris.

## Isi

📄 **[AutoLaris-Payment-Gateway-API.md](./AutoLaris-Payment-Gateway-API.md)** — dokumentasi lengkap:

- Base URL & environment (production / development)
- Autentikasi Bearer Token (API Key) + whitelist IP
- Endpoint `POST /api/h2h/create_payment` — request, response, tabel field
- Daftar 8 channel pembayaran
- Alur & contoh handler callback (Node.js / Express & PHP / Laravel)
- Penanganan error + pencegahan double-charge
- Checklist go-live & daftar pertanyaan terbuka untuk tim AutoLaris

## Ringkas

| | |
|---|---|
| **Base URL** | `https://api-h2h.autolaris.com` |
| **Endpoint** | `POST /api/h2h/create_payment` |
| **Auth** | `Authorization: Bearer <API_KEY>` |
| **Content-Type** | `application/json` |

### Channel pembayaran

| Kode | Keterangan |
|---|---|
| `QRIS` | QRIS |
| `VABCA` | BCA Virtual Account |
| `VAMANDIRI` | Mandiri Virtual Account |
| `VABNI` | BNI Virtual Account |
| `VABRI` | BRI Virtual Account |
| `VABSI` | BSI Virtual Account |
| `VAPERMATA` | Permata Virtual Account |
| `DANA` | E-Wallet DANA |

### Contoh request

```bash
curl -X POST "https://api-h2h.autolaris.com/api/h2h/create_payment" \
  -H "Authorization: Bearer <API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "reff_id": "ORD-001",
    "channel_code": "VAMANDIRI",
    "customer_id": "31857118",
    "customer_name": "Budi Santoso",
    "customer_phone": "081234567890",
    "customer_email": "customer@example.com",
    "expired": "20270422094000",
    "amount": "25000",
    "callback_url": "https://your-domain.com/autolaris/callback"
  }'
```

### Contoh response

```json
{
  "rc": "00",
  "ket": "Sukses",
  "data": {
    "trx_id": "671647",
    "virtual_account": "8779611150001393",
    "qr": "",
    "payment_code": "",
    "url": "",
    "amount": 25000,
    "admin": 3000,
    "total": 28000
  }
}
```

> Tagihkan `total` (sudah termasuk `admin`) ke pelanggan. Cek `rc == "00"` sebelum memproses `data`.

## Sumber resmi

- Dashboard seller / request API Key: https://seller.autolaris.com
- Daftar akun: https://seller.autolaris.com/daftar

## Lisensi & disclaimer

Dokumentasi komunitas untuk tujuan integrasi. Seluruh merek dagang milik AutoLaris. Konfirmasikan detail final (format payload callback, zona waktu `expired`, signature) ke tim AutoLaris sebelum produksi.
