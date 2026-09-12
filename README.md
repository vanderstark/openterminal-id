<div align="center">

# OpenTerminal

**Workspace berbasis terminal untuk monitoring pasar keuangan — saham, crypto, opsi, makro — dibangun 100% pada data pasar publik gratis.**

Dark. Dense. Keyboard-driven. Tidak ada API key berbayar, tidak ada langganan.

[![Stack](https://img.shields.io/badge/stack-Next.js%20%2B%20Express%20%2B%20TypeScript-orange)](#tech-stack)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](#license)
[![No API Key Required](https://img.shields.io/badge/data-tidak%20ada%20API%20key%20required-brightgreen)](#data-sources)

<br/>

<img src="docs/screenshots/dashboard.png" alt="Dashboard OpenTerminal — chart live, pane quote, watchlist, berita, dan indeks makro" width="100%" />

</div>

<br/>

## Mengapa OpenTerminal?

Terminal trading profesional berbiaya **$2.000+ per bulan**. Sebagian besar dashboard retail menutup fitur bagus di balik paywall atau menggunakan satu sumber data yang tidak andal.

OpenTerminal mendekati masalah ini dengan **menyambungkan beberapa endpoint data pasar publik gratis** — yang sama yang menjalankan situs keuangan besar — ke dalam satu dashboard berbasis widget yang cepat, keyboard-first, dan berjalan 100% di mesin Anda. Setiap endpoint memiliki rantai fallback otomatis, sehingga satu penghalang tidak pernah membuat aplikasi tidak bisa digunakan.

Tidak ada signup. Tidak ada kartu kredit. Tidak ada tier demo rate-limited. Clone, `npm install`, dan Anda punya terminal langsung dalam hitungan menit.

<br/>

## ✨ Fitur Utama

- 🖥️ **Workspace berbasis widget** — drag, resize, tambah, dan hapus panel (`react-grid-layout`); layout disimpan lokal dan otomatis recovery
- ⌘K **command palette global** — cari saham, ETF, dan crypto langsung oleh keyboard
- 📈 **Charting profesional** (via [`lightweight-charts`](https://github.com/tradingview/lightweight-charts)) — candlestick, bar, line, area, volume, 8 timeframe (1D → MAX), SMA / EMA / VWAP / Bollinger Bands / RSI / MACD, tiap indikator memiliki hover-legend dengan detail OHLC, volume, nilai tiap indikator
- 💹 **Pane quote** — harga terakhir / bid / ask / OHLC, volume, market cap, P/E, EPS, dividend yield, 52-week range, beta, shares outstanding
- 📰 **Feed berita** — dikelompokkan dan duplikasi RSS, filter per-simbol atau global
- 🔍 **Screener pasar lengkap** — filter sektor, market cap, % change, volume di seluruh pasar saham AS, dapat di-sort tiap kolom
- 🗺️ **Heatmap sektor live** — treemap berukuran market cap, berwarna % perubahan harian, refresh tiap beberapa detik
- ⛓️ **Options chain** — calls dan puts side-by-side dengan strike, bid/ask, volume, open interest, dan highlight ITM
- 🪙 **Board kripto** — aset utama dengan sparkline 7-hari, BTC/ETH dominance, dan charting lengkap OHLCV
- 🏦 **Dashboard makro** — yield curve Treasury AS, VIX, dan proxy indeks/commodity utama
- 💼 **Tracker portofolio** — log transaksi beli/jual, rata-rata biaya, P&L realisasi & unrealisasi (tersimpan di SQLite)
- 📅 **Kalender ekonomi** — event (Fed, ECB, CPI, NFP) dengan forecast, bacaan sebelumnya, dan hasil aktual; kalender earnings dengan forecast vs. actual EPS serta gerakan harga berikutnya
- 🤖 **Asisten AI** (opsional) — ajukan pertanyaan tentang simbol yang sedang dilihat, didukung oleh Claude, dengan konteks data terminal saat ini
- ⚡ **Update hampir real-time** — quote dan indeks refresh tiap 1 detik dengan flash halus pada perubahan

<br/>

## 🚀 Cara Memulai (Quick Start)

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

<br/>

## 📦 Stack Teknis

| Lapisan | Stack |
|---|---|
| Frontend | Next.js 15 · React 19 · TypeScript · Tailwind CSS 4 · Zustand · TanStack Query |
| Charts | `lightweight-charts` (candles/indicators) · D3 (heatmap treemap) · Recharts (yield curve) |
| Backend | Node.js · Express · TypeScript |
| Database | SQLite (`better-sqlite3`, WAL mode) |
| AI | Anthropic Claude (opsional) |

<br/>

## 📂 Data Sumber Gratis Tanpa API Key

Setiap endpoint memiliki **rantai fallback**. Jika satu provider gagal, otomatis pakai yang lain.

| Data | Sumber Primer | Fallback |
|---|---|---|
| Quotes (stocks/ETFs) | Nasdaq public quote API | Yahoo Finance → Stooq |
| Fundamentals (P/E, EPS, beta, div yield) | TradingView scanner API | — |
| Historical candles | Nasdaq chart API | Yahoo Finance → Stooq |
| Symbol search | TradingView symbol search | Yahoo Finance |
| Screener / Heatmap pasar | TradingView scanner API (live seluruh US market) | — |
| Options chain | Nasdaq option-chain API | Yahoo Finance |
| Berita | Yahoo Finance RSS | Google News RSS |
| Quote & Board kripto | CoinGecko | Binance public API |
| Candle kripto | Binance public API (klines) | — |
| Makro (Treasury yields, VIX) | FRED (Federal Reserve) | — |
| Kalender ekonomi | Forex Factory public feed | — |
| Kalender earnings | TradingView scanner API | — |
| Earnings history | Nasdaq earnings-surprise API | — |

> ⚠️ Endpoint publik ini **bisa terkhat**, gunakan untuk keperluan pribadi/edukasi, **jangan untuk keputusan investasi nyata**.

<br/>

## 📁 Struktur Proyek

```
├── server/                  # Express + TypeScript API
│   └── src/
│       ├── providers/       # nasdaq, tradingview, yahoo, stooq, fred, binance, coingecko, news
│       ├── routes/          # market, portfolio, ai
│       ├── cache.ts         # TTL cache dengan stale-while-revalidate fallback
│       └── db.ts            # SQLite (better-sqlite3, WAL)
└── web/                      # Next.js 15 + React 19 + Tailwind 4
    ├── components/           # TopBar, Sidebar, Workspace, CommandPalette
    ├── components/widgets/   # Chart, Quote, Watchlist, News, Screener, Heatmap, Crypto, Options, Macro, Portfolio, Calendar, AI
    ├── lib/                  # API client, technical indicators
    └── store/                # Zustand store (workspace layout, persisted)
```

<br/>

## 🐳 Docker

```bash
docker compose up --build
```

Data portofolio persists di volume `terminal-data` (SQLite, WAL mode). Ports dipublikasikan hanya pada `127.0.0.1` secara default.

<br/>

## 📜 Lisensi

[MIT](LICENSE)

⭐ Star repo jika bermanfaat — ini cara terbaik untuk mendukung proyek.
</div>