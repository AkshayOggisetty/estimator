# Estimate & Invoice Extractor

A single-file, browser-only web app that reads an **estimation PDF** and an **invoice PDF** with the Google Gemini API and hands everything back in copyable form — per-document tables, an estimate-vs-invoice comparison, and one-click copy as text, CSV, or JSON.

## Features

- 🔑 Paste your own **Google Gemini API key** (free tier — no billing needed)
- 📐🧾 Drag & drop an estimation and/or invoice PDF
- 🤖 Gemini reads the PDFs natively (tables, line items, totals, parties, dates)
- 📊 Automatic **estimate vs invoice** comparison when both are provided
- 📋 Copy as formatted text, CSV (line items), or raw JSON
- 🌑 Sleek dark UI, no build step, no backend

## Usage

1. Open `index.html` in any modern browser (or host it anywhere static).
2. Get a free API key at **[aistudio.google.com/apikey](https://aistudio.google.com/apikey)** and paste it in.
3. Drop in your PDFs and click **Read documents**.

Everything runs client-side — your key and files are sent directly to Google's API and never touch any other server.

## Model

Defaults to **`gemini-2.5-flash`** (best free choice for document extraction). Flash-Lite and Pro are also selectable in the UI.
