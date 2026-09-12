<div align="center">

# OpenTerminal

**Workspace berbasis terminal untuk monitoring pasar keuangan — saham, crypto, opsi, makro — dibangun 100% pada data pasar publik gratis.**

Dark. Dense. Keyboard-driven. Tidak ada API key berwajib, hanya opsional untuk AI.

[![Stack](https://img.shields.io/badge/stack-Next.js%20%2B%20Express%20%2B%20TypeScript-orange)](#tech-stack)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](#license)
[![No API Key Required](https://img.shields.io/badge/data-tidak%20ada%20API%20key%20required-brightgreen)](#data-sources)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-ff69b4.svg)](#contributing)

<br/>

<img src="docs/screenshots/dashboard.png" alt="Dashboard OpenTerminal — chart live, quote panel, watchlist, berita, dan indeks makro" width="100%" />

</div>

<br/>

## � Mengapa OpenTerminal?

Terminal trading profesional berbiaya **$2.000+ per bulan**. Sebagian besar dashboard retail menutup fitur bagus di balik paywall atau menggunakan satu sumber data yang tidak andal.

OpenTerminal mendekati masalah ini dengan **menyambungkan beberapa endpoint data pasar publik gratis** — yang sama yang menjalankan situs keuangan besar — ke dalam satu dashboard berbasis widget yang cepat, keyboard-first, dan berjalan 100% di mesin Anda. Setiap endpoint memiliki rantai fallback otomatis, sehingga satu penghalang tidak pernah membuat aplikasi tidak bisa digunakan.

**Filsafat:** Tidak ada signup, tidak ada kartu kredit, tidak ada tier demo rate-limited. Clone, `npm install`, dan Anda punya terminal langsung dalam hitungan menit.

<br/>

## ✨ Fitur Utama

| Fitur | Deskripsi |
|---|---|
| 🖥️ **Workspace berbasis widget** | drag, resize, tambah, hapus panel (`react-grid-layout`); layout disimpan lokal dan otomatis dikembalikan pada reload |
| ⌘K **command palette global** | cari saham, ETF, crypto langsung dengan keyboard |
| 📈 **Charting profesional** | via `lightweight-charts` — candlestick, bar, line, area, volume, 8 timeframe (1D → MAX), SMA / EMA / VWAP / Bollinger Bands / RSI / MACD, hover-legend dengan detail OHLC, volume, nilai indikator |
| 💹 **Pane quote** | harga terakhir / bid / ask / OHLC, volume, market cap, P/E, EPS, dividend yield, 52-week range, beta, shares outstanding |
| 📰 **Feed berita** | dikelompokkan dan duplikasi RSS, filter per-simbol atau global |
| 🔍 **Screener pasar lengkap** | filter sektor, market cap, % change, volume di seluruh pasar saham US, sortable tiap kolom |
| 🗺️ **Heatmap sektor live** | treemap berukuran market cap, berwarna % perubahan harian, refresh tiap beberapa detik |
| ⛓️ **Options chain** | calls dan puts side-by-side dengan strike, bid/ask, volume, open interest, highlight ITM |
| 🪙 **Board kripto** | aset utama dengan sparkline 7-hari, BTC/ETH dominance, charting OHLCV lengkap |
| 🏦 **Dashboard makro** | yield curve Treasury AS, VIX, proxy indeks/commodity utama |
| 💼 **Tracker portofolio** | log transaksi beli/jual, rata-rata biaya, P&L realisasi & unrealisasi (SQLite) |
| 📅 **Kalender ekonomi** | event (Fed, ECB, CPI, NFP) dengan forecast, bacaan sebelumnya, hasil aktual; kalender earnings forecast vs actual EPS serta gerakan harga berikutnya |
| 🤖 **Asisten AI** (opsional) | ajukan pertanyaan tentang simbol yang dilihat, didukung Claude, konteks data terminal |
| ⚡ **Update hampir real-time** | quote dan indeks refresh tiap 1 detik dengan flash halus pada perubahan |
| ⌨️ **Shortcut keyboard** | `⌘K` search, `⌥1–9` add widget, dan shortcut lainnya di seluruh antarmuka |

<br/>

## 🚀 Cara Memulai (Quick Start)

Pilih metode yang sesuai dengan butuhan Anda:

### Metode 1: Lokal (Development) — untuk pengembangan dan uji coba

```bash
# 1. Clone repositori
git clone https://github.com/vanderstark/openterminal-id.git
cd openterminal

# 2. Install dependensi
npm install

# 3. Jalankan development server
npm run dev

# Buka http://localhost:3000
```

### Metode 2: Docker — untuk deployment cepat di mana saja

```bash
# Dari root proyek
docker compose up --build
```

Ports dipublikasikan pada `127.0.0.1` secara default. Untuk mengubah, lihat konfigurasi di bawah ini.

### Metode 3: Production — untuk deployment serius

```bash
# Build dan start di production mode
npm run build
npm start
# Atau gunakan Docker di production
docker compose -f docker-compose.prod.yml up --build
```

<br/>

## ⚙️ Konfigurasi Aplikasi

OpenTerminal dirancan sehingga **bisa digunakan tanpa konfigurasi apa-apa** untuk fitur dasar. Namun, beberapa fitur opsional memerlukan settingan.

### Environment Variables (.env)

File `.env` opsional di root proyek. Jika tidak dibuat, nilai default akan digunakan.

| Variable | Deskripsi | Default / Catatan |
|---|---|---|
| `API_HOST` | Host binding API server | `127.0.0.1` (hanya lokal) |
| `API_PORT` | Port API server | `4000` |
| `WEB_ORIGIN` | Origin yang diizinkan mengakses API | `http://localhost:3000` |
| `API_KEY` | Secret kunci bersama untuk endpoint portfolio & AI | Otomatis dibuat pada pertama kali dijalankan, disimpan ke `data/.api-key` |
| `API_KEY_EXPIRY` | Kadaluarsa API key (hari) | `0` = tidak kadaluarsa |
| `DATA_REFRESH_INTERVAL` | Interval refresh data (detik) | `1` (sekitar real-time) |
| `ENABLE_CRYPTO` | Aktifkan fitur crypto | `true` |
| `ENABLE_PORTFOLIO` | Aktifkan tracker portofolio | `true` |

### Cara Menyiapkan API Key (Opsional)

1. Jalankan aplikasi sekali tanpa `API_KEY` di `.env`.
2. Aplikasi akan **membuat API key secara otomatis** dan menyimpannya di `data/.api-key`.
3. Web app akan membaca file tersebut secara otomatis.
4. **Untuk production:** Salin `data/.api-key` ke direktori yang aman dan tetapkan `API_KEY` di `.env` dengan nilai yang sama.

### Data Sources (Semua Gratis, Tidak Ada API Key Wajib)

Semua endpoint data sudah disiapkan dengan **rantai fallback**. Jika provider pertama gagal, yang kedua akan otomatis digunakan.

| Fitur | Sumber Primer | Fallback | Catatan |
|---|---|---|---|
| Quote saham/ETF | Nasdaq public quote API | Yahoo Finance → Stooq | - |
| Fundamentals (P/E, EPS, beta) | TradingView scanner API | — | - |
| Historical candles | Nasdaq chart API | Yahoo Finance → Stooq | Data mungkin terkhat |
| Screener/Heatmap | TradingView scanner API (live US market) | — | - |
| Options chain | Nasdaq option-chain API | Yahoo Finance | - |
| News | Yahoo Finance RSS | Google News RSS | - |
| Crypto quotes & board | CoinGecko | Binance public API | - |
| Crypto candles | Binance public API (klines) | — | - |
| Macro (Treasury yields, VIX) | FRED (Federal Reserve) | — | - |
| Kalender ekonomi | Forex Factory public feed | — | - |
| Earnings calendar | TradingView scanner API | — | - |
| Earnings history | Nasdaq earnings-surprise API | — | - |

> ⚠️ **Penting:** Endpoint ini **bisa ketinggalan waktu** dan hanya untuk **personal/edukasi**. **Jangan gunakan untuk keputusan investasi nyata**.

<br/>

## 📖 Panduan Penggunaan Lengkap

Setelah aplikasi running di `http://localhost:3000`, berikut panduan penggunaan fitur-fiturnya:

### 1. Menambahkan Widget ke Workspace

1. Klik kanvas workspace (area kosong).
2. Akan muncul opsi panel di sisi kanan/atas.
3. Pilih widget yang diinginkan (Chart, Quote, Watchlist, dll).
4. Widget akan ditambahkan dan bisa diresize/drag sesuai keinginan.

### 2. Mencari Saham/Instrumen (Command Palette)

1. Tekan **`⌘K`** (atau `Ctrl+K` pada Windows/Linux).
2. Masukkan nama saham, simbol kripto, atau ETF.
3. Pilih hasil yang diinginkan dari daftar dropdown.
4. Aset tersebut akan dipindahkan ke panel aktif terpilih.

### 3. Menggunakan Charting

1. Pilih widget **Chart** dari workspace.
2. Gunakan toolbar atas chart untuk:
   - Mengubah timeframe (1D, 1W, 1Bulan, MAX, dll)
   - Menambahkan indikator (SMA, RSI, MACD, EMA, Bollinger Bands, VWAP)
   - Mengubah tipe chart (candlestick, bar, line, area)
   - Melihat legenda hover saat mengarah kursor ke candle

### 4. Melihat Quote Panel

1. Tambahkan widget **Quote** ke workspace.
2. Panel menampilkan:
   - Harga terakhir, bid, ask
   - Volume transaksi
   - Market cap
   - P/E Ratio, EPS
   - Dividend yield
   - Rentang 52 minggu
   - Beta, shares outstanding

### 5. Melacak Portofolio

1. Tambahkan widget **Portfolio**.
2. Klik tombol **"Add Transaction"** (atau ikon +).
3. Pilih **Buy** atau **Sell**.
4. Masukkan simbol, jumlah, harga, tanggal transaksi.
5. Portofolio akan menampilkan:
   - Rata-rata harga perbelian
   - P&L realisasi (sudah terjual)
   - P&L unrealisasi (masih dalam pos)
   - Grafik nilai portofolio seiring waktu

### 6. Kalender Ekonomi

1. Tambahkan widget **Economic Calendar**.
2. Pilih event yang ingin dilacak (Fed, CPI, NFP, dll).
2. Kolom akan menampilkan:
   - **Event**: nama kejadian (Fed rate decision, CPI release, dll)
   - **Waktu**: jadwal dalam WIB/EST
   - **Forecast**: perkiraan analis
   - **Previous**: bacaan sebelumnya
   - **Actual**: hasil aktual (saat tersedia)
   - **Surprise**: perbedaan forecast vs actual
3. Aktifkan notifikasi (jika tersedia) untuk event terpenting.

### 7. AI Assistant (Opsional)

1. Pastikan `API_KEY` sudah di-set di `.env` (atau biarkan otomatis).
2. Tambahkan widget **AI Widget** ke workspace.
3. Klik widget, lalu ketik pertanyaan seperti:
   - "Apa outlook analyst untuk AAPL?"
   - "Mengapa harga Bitcoin turun hari ini?"
   - "Bandingkan SMA 20 vs EMA 50 untuk TSLA"
4. AI akan merespons dengan konteks data terbar dari terminal.

### 8. Menyesuaikan Layout Workspace

1. Floating over widget untuk menampilkan handle drag.
2. Drag widget ke posisi baru.
3. Grab ujung widget untuk resize lebar/tinggi.
4. Klik tombol **"Save Layout"** (jika muncul) atau layout akan otomatis disimpan ke localStorage browser.
5. Pada reload berikutnya, layout akan muncul seperti sebelumnya.

<br/>

## 🐳 Docker Deployment

OpenTerminal sudah siap pakai dengan Docker Compose.

### Instalasi Cepat

```bash
# Clone repositori
git clone https://github.com/vanderstark/openterminal-id.git
cd openterminal

# Jalankan dengan Docker Compose
docker compose up --build
```

### Konfigurasi Lanjutan Docker

- **Ports**: Secara default, API ter-bind ke `127.0.0.1:4000` dan Web ke `localhost:3000`.
- **For exposing ke luar mesin**: Atur `API_HOST=0.0.0.0` di file `.env` dan pastikan `WEB_ORIGIN` mengarah ke domain/IP publik Anda.
- **Volume data**: Data portofolio persist di volume `terminal-data` (SQLite, WAL mode). Untuk backup, copy folder `data/` atau volume Docker.

### Docker Compose Environment Variables

Bisa di-set melalui file `.env` atau `docker-compose.yml`:

```yaml
# Contoh .env
API_HOST=0.0.0.0
API_PORT=4000
WEB_ORIGIN=https://domain-anda.com
API_KEY=rahasia-kuat-123
DATA_REFRESH_INTERVAL=2
```

<br/>

## 📁 Struktur Proyek

```
openterminal/
├── docker-compose.yml       # Orchestration Docker
├── docker-compose.prod.yml  # Konfigurasi production (opsional)
├── LICENSE                # Lisensi MIT
├── README.md              # Dokumentasi ini
├── package.json           # Dependensi root (menginisialisasi server & web)
├── package-lock.json      # Lock file dependensi
├── data/                  # Folder data SQLite & cache
│   └── .api-key           # API key otomatis (jika dibuat)
├── docs/                  # Dokumentasi tambahan (screenshot, panduan)
├── server/                # Backend Express + TypeScript API
│   └── src/
│       ├── providers/     # Data providers (nasdaq, yahoo, tradingview, binance, coingecko, fred, econcalendar, news)
│       ├── routes/        # API routes (market, portfolio, ai)
│       ├── cache.ts       # TTL cache dengan stale-while-revalidate fallback
│       ├── db.ts          # SQLite (better-sqlite3, WAL mode)
│       └── index.ts       # Entry point server
└── web/                   # Frontend Next.js 15 + React 19 + Tailwind 4
    ├── app/               # App Router Next.js 15
    │   ├── layout.tsx     # Layout utama
    │   └── page.tsx       # Homepage/dashboard
    ├── components/        # Komponen UI reusable
    │   ├── TopBar.tsx
    │   ├── Sidebar.tsx
    │   ├── Workspace.tsx
    │   └── CommandPalette.tsx
    ├── components/widgets/ # Widget visualization (25 komponen)
    │   ├── ChartWidget.tsx
    │   ├── QuoteWidget.tsx
    │   ├── WatchlistWidget.tsx
    │   ├── NewsWidget.tsx
    │   ├── ScreenerWidget.tsx
    │   ├── HeatmapWidget.tsx
    │   ├── CryptoWidget.tsx
    │   ├── OptionsWidget.tsx
    │   ├── PortfolioWidget.tsx
    │   ├── MacroWidget.tsx
    │   ├── CalendarWidget.tsx
    │   └── AiWidget.tsx
    ├── lib/               # Utilitas (API client, indikator teknikal)
    │   ├── api-key.ts
    │   ├── api.ts
    │   └── indicators.ts
    ├── store/             # Zustand store (layout workspace, persisted)
    │   └── terminal.ts
    ├── styles/            # Global styles, Tailwind config
    │   ├── globals.css
    │   └── tailwind.config.mjs
    └── package.json       # Dependensi frontend
```

<br/>

## 🛡️ Keamanan & Privasi

- **API bindings default:** `127.0.0.1` — hanya dapat diakses dari localhost.
- **Web origin check:** Hanya menerima request dari `http://localhost:3000` (atau `WEB_ORIGIN` yang di-set).
- **Portfolio & AI endpoints:** Menggunakan shared secret (`API_KEY`). Jika tidak di-set, sistem generate otomatis.
- **Untuk production:** Pastikan `API_HOST`, `WEB_ORIGIN`, dan `API_KEY` di-set dengan nilai yang kuat. Terminasi TLS di reverse proxy jika ter-expose ke luar.
- **Data source:** Semua data di-fetch langsung dari klien browser, tidak disimpan server kecuali data portofolio (SQLite lokal).
- **Privasi:** Tidak ada data pengiriman ke server pihak ketiga selama aplikasi dijalankan lokal.

<br/>

## 📜 Lisensi

[MIT](LICENSE)

⭐ **Star repo** jika bermanfaat — ini cara terbaik untuk mendukung proyek.

<br/>

## 🤝 Kontribusi

Pull request sangat diterima, terutama:

- ✅ Data providers baru atau lebih resilient (`server/src/providers/`).
- ✅ Widget baru (`web/components/widgets/`).
- ✅ Perbaikan bug dan polishing UI.

**Tips:** Buka issue terlebih dahulu untuk hal non-trivial agar kita bisa menyelaraskan pendekatan sebelum Anda menginvestasikan waktu.

<br/>

---

**Dibuat dengan ❤️ oleh satu pengembang untuk komunitas trader/ investor yang membutuhkan terminal profesional tanpa biaya bersiklus bulanan.**

---