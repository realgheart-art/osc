# OSC JPN Kedah — Panduan Pemasangan

Portal sehenti (One Stop Centre) dashboard sektor JPN Kedah.
Stack biasa Cikgu: **Google Apps Script + Google Sheets** (backend) · **GitHub Pages** (frontend) · **PWA** penuh.

## Fail dalam pakej ini
| Fail | Guna | Letak di |
|---|---|---|
| `Code.gs` | Otak sistem (auth, pengumuman, direktori, log) | Editor Apps Script |
| `index.html` | Antara muka (logo sudah terbenam) | Repo GitHub Pages |
| `manifest.json` | Tetapan PWA | Repo GitHub Pages |
| `sw.js` | Service worker (offline & pasang) | Repo GitHub Pages |
| `icon-192.png`, `icon-512.png`, `icon-maskable-512.png` | Ikon PWA | Repo GitHub Pages |

---

## LANGKAH A — Backend (Google Apps Script + Sheets)

1. Buka Google Sheet **baharu** (namakan cth: `OSC JPN Kedah — Pangkalan Data`).
2. Menu **Extensions › Apps Script**. Padam kod contoh, **tampal seluruh `Code.gs`**.
3. Di bahagian atas `Code.gs`, tukar dua baris ini kepada emel rasmi Cikgu:
   ```js
   const ADMIN_EMAIL = 'emel-rasmi-cikgu';   // Admin pertama
   const ADMIN_NAMA  = 'Nama Cikgu';
   ```
4. Pilih fungsi **`setup`** pada bar atas › klik **Run**. Benarkan kebenaran (authorize) bila diminta.
   → Ini akan cipta 4 helaian: **Pengguna, Pengumuman, Direktori, Log**, dan daftar Cikgu sebagai Admin.
5. Klik **Deploy › New deployment › (⚙️) Web app**:
   - **Execute as:** Me
   - **Who has access:** Anyone
   - **Deploy** › salin **URL** yang berakhir dengan `/exec`.

> Setiap kali `Code.gs` dikemas kini: **Deploy › Manage deployments › ✏️ Edit › Version: New version › Deploy**. URL kekal sama.

---

## LANGKAH B — Frontend (GitHub Pages)

1. Buka `index.html`, cari baris berhampiran atas skrip:
   ```js
   const API_URL = '__API_URL__';   // <-- TAMPAL URL /exec DI SINI
   ```
   Gantikan `__API_URL__` dengan URL `/exec` dari Langkah A.
2. Muat naik ke repo GitHub: `index.html`, `manifest.json`, `sw.js`, dan **ketiga-tiga ikon** (di root repo yang sama).
3. **Settings › Pages › Deploy from branch › main / root**. Tunggu URL `https://username.github.io/nama-repo/`.
4. Buka di telefon → menu pelayar → **Add to Home Screen** untuk pasang sebagai app (ikon OSC muncul).

---

## LANGKAH C — Guna & isi kandungan

Log masuk sebagai Admin (emel Cikgu → set kata laluan kali pertama). Kemudian **menu (👤) › Panel Admin**:

- **Pengumuman** — tambah/sunting; tanda *Penting* untuk pin ke atas.
- **Direktori** — untuk setiap sektor, isi Nama dashboard + URL, pilih **Mod paparan**:
  - `Buka tab baharu` — untuk pautan luar (Looker Studio, Power BI, dll) yang menyekat iframe. **Pilihan selamat & disyorkan.**
  - `Iframe` — hanya untuk pautan yang membenarkan embed (cth GitHub Pages Cikgu sendiri).
  - Ubah **Status** dari `Akan Datang` → `Aktif` bila dashboard sedia.
- **Pengguna** — daftar emel warga JPN (inilah "pagar"). Mereka set kata laluan sendiri semasa log masuk pertama.
- **Log** — jejak log masuk & perubahan.

---

## Struktur Sheets (rujukan)

**Pengguna:** `Emel · Nama · Peranan · PasswordHash · Salt · Status · TarikhDaftar · LoginTerakhir · ResetCode · ResetExpiry`
**Pengumuman:** `ID · Tarikh · Tajuk · Isi · Penting · Status`
**Direktori:** `Sektor · NamaDashboard · URL · Kategori · Ikon · ModPaparan · Susunan · Status`
**Log:** `Masa · Emel · Tindakan · Butiran`

> Boleh sunting terus dalam Sheets, tapi lebih selamat guna Panel Admin supaya format kekal betul.

---

## Nota keselamatan (penting)

- Kata laluan disimpan **ter-hash (SHA-256 + garam)** — bukan teks biasa. Sesuai untuk kawalan akses dalaman jabatan.
- Ini memadai untuk OSC dalaman, **bukan** untuk data ultra-sensitif. **Jangan** simpan no. KP penuh / data gaji dalam Sheet yang sama.
- "Lupa kata laluan" menghantar kod 6-digit ke emel rasmi (sah 30 minit). Pastikan emel dalam senarai betul-betul boleh terima mesej.
- Reset akaun Admin tersekat? Buka Sheet **Pengguna**, kosongkan sel `PasswordHash` & `Salt` untuk baris Cikgu — log masuk sekali lagi akan minta set kata laluan baharu.

---
*Dibina mengikut identiti navy-emas JPN Kedah · Fraunces + Plus Jakarta Sans.*
