# Xiaomi MiMo Auto Register

Bot otomatis untuk mendaftar akun di [Xiaomi MiMo API Open Platform](https://platform.xiaomimimo.com) menggunakan Playwright + temp mail.

## Fitur

- **Auto register** — isi form, pilih region, handle captcha (manual)
- **Temp email** — generate email sementara + auto-extract OTP verifikasi
- **Terms & agreements** — auto-checklist + confirm
- **Cookie consent** — auto-accept di tiap halaman
- **API key extraction** — buat API key otomatis + simpan ke `test.txt`
- **2captcha ready** — tinggal isi API key, ganti `captchaMode: '2captcha'`

## Prasyarat

- Node.js >= 18
- Chromium (auto-install via Playwright)

## Instalasi

```bash
npm install
npx playwright install chromium
```

## Cara pakai

```bash
npm run register
```

### Konfigurasi

Edit bagian `CONFIG` di `register.js`:

```js
const CONFIG = {
  password: 'PortoAuto2025!',    // password akun
  region: 'Indonesia',           // region (auto-detect dari URL)
  apiKeyName: 'auto-xxx',        // prefix nama API key
  outputFile: 'test.txt',        // file output API key
  captchaMode: 'manual',         // 'manual' | '2captcha'
  captchaApiKey: '',             // isi kalau pakai 2captcha
};
```

## Flow (11 steps)

| Step | Keterangan |
|------|-----------|
| 1 | Launch browser Chromium |
| 2 | Generate temp email |
| 3 | Buka halaman register Xiaomi + accept cookies |
| 4 | Region auto-detected |
| 5 | Isi email, password, confirm, checklist setuju |
| 6 | Submit form + **captcha manual** (auto-detect solved) |
| 7 | Tunggu OTP email → auto-extract → auto-fill |
| 8 | Terms & agreements (checklist + confirm) |
| 9 | Redirect ke platform console + accept cookies |
| 10 | Navigasi API Keys → Create API Key |
| 11 | Extract API key → simpan ke `test.txt` |

## Output

Format di `test.txt` (append, tidak overwrite):

```
# Xiaomi MiMo API Key - Generated 2026-06-19T...
Email: user_xxx@moymoy.me
Password: PortoAuto2025!
API Key Name: auto-xxx
API Key: sk-xxxxxxxxxxxxxxxxx
```

## File

| File | Deskripsi |
|------|-----------|
| `register.js` | Main bot (Playwright) |
| `tempmail.js` | Temp email + OTP extractor (Node) |
| `tempmail.py` | Temp email + OTP extractor (Python) |
| `test.txt` | Output API key (gitignored) |
| `*.png` | Screenshot debug (gitignored) |

## Screenshot

Script otomatis menyimpan screenshot di tiap step untuk debugging:
- `before_submit.png` — form sebelum submit
- `api_keys_page.png` — halaman API keys
- `api_key_created.png` — setelah API key dibuat
- `error.png` — jika terjadi error

## Catatan

- Captcha harus diselesaikan manual (browser muncul visible)
- Kalau selector tidak match, cek screenshot dan sesuaikan selector di `register.js`
- Supabase anon key di `tempmail.js` bersifat public
