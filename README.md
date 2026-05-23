<div align="center">

<img src="logo.png" alt="TruffleHog AutoScan Logo" width="180"/>

# 🔐 TruffleHog Secret Scans für dieses Repo

[![TruffleHog Scan](https://img.shields.io/github/actions/workflow/status/jbkunama1/hAI.TruffelHogAutoScan/trufflehog.yml?label=TruffleHog%20Scan&logo=github&logoColor=white)](https://github.com/jbkunama1/hAI.TruffelHogAutoScan/actions/workflows/trufflehog.yml)
![Security](https://img.shields.io/badge/security-secrets%20scan%20enabled-brightgreen?logo=trustpilot&logoColor=white)
![Schedule](https://img.shields.io/badge/schedule-daily%20at%2002%3A00%20UTC-blue?logo=clockify&logoColor=white)

<a href="https://www.buymeacoffee.com/highfish"><img src="https://cdn.buymeacoffee.com/buttons/default-orange.png" width="120" height="28" alt="Buy me a coffee"></a>

</div>

> Dieses Repository wird automatisch mit [TruffleHog](https://github.com/trufflesecurity/trufflehog) auf geleakte Secrets (API-Keys, Tokens, Passwörter etc.) gescannt. 🕵️‍♂️

---

## ✨ Was macht dieses Setup?

- Scannt das Repository **bei jedem Push** auf `main`/`master` auf Secrets.
- Führt zusätzlich **jeden Tag um 02:00 Uhr UTC** einen vollständigen Scan aus.
- Nutzt die offizielle **TruffleHog GitHub Action** aus dem [Marketplace](https://github.com/marketplace/actions/trufflehog-oss).

Damit helfen wir, versehentlich eingecheckte Zugangsdaten frühzeitig zu entdecken und zu entfernen.

---

## ⚙️ GitHub Actions Workflow (`.github/workflows/trufflehog.yml`)

```yaml
name: TruffleHog Secret Scan

on:
  schedule:
    - cron: '0 2 * * *'   # täglich um 02:00 Uhr UTC
  push:
    branches: [ main, master ]

jobs:
  trufflehog-scan:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4
        with:
          fetch-depth: 0    # gesamte Git-Historie scannen

      - name: Run TruffleHog OSS
        uses: trufflesecurity/trufflehog@main
        with:
          extra_args: --only-verified
```

> `--only-verified` reduziert False Positives – nur verifizierte Secrets werden gemeldet.

---

## 🕒 Cron-Zeitplan verstehen

```yaml
cron: '0 2 * * *'
```

| Feld | Wert | Bedeutung |
|------|------|-----------|
| Minute | `0` | zur vollen Stunde |
| Stunde | `2` | um 02:00 Uhr |
| Tag | `*` | jeden Tag |
| Monat | `*` | jeden Monat |
| Wochentag | `*` | jeden Wochentag |

⚠️ GitHub Actions nutzt **UTC-Zeit** → 02:00 UTC = 03:00 Uhr CEST (Deutschland).

---

## 🚀 Einrichtung – Schritt für Schritt

1. **Repository erstellen** – erledigt ✅
2. **Workflow-Datei liegt bereits unter** `.github/workflows/trufflehog.yml` ✅
3. **Badges** funktionieren automatisch nach dem ersten Workflow-Run.
4. **Ersten Run anstoßen**: Tab **Actions** → Workflow auswählen → **Run workflow**.

---

## 🧪 Scan manuell starten

1. Tab **Actions** öffnen
2. Workflow **"TruffleHog Secret Scan"** auswählen
3. Button **"Run workflow"** klicken

Praktisch vor einem Release oder nach dem Hinzufügen neuer Credentials.

---

## 📊 Ergebnisse lesen

- ✅ **Grüner Haken** = keine verifizierten Secrets gefunden
- ❌ **Rotes X** = Fund vorhanden → sofort rotieren!

Bei einem Fund:
1. Secret **sofort invalidieren** (Token/Key neu generieren).
2. Commit-Historie bereinigen (z.B. mit `git filter-repo`).
3. Erneut pushen und Scan wiederholen.

---

## 🧱 Best Practices

- 🚫 Keine API-Keys oder Tokens direkt im Code
- ✅ Secrets nur via GitHub **Repository Secrets** (`Settings → Secrets`)
- ✅ `.env`-Dateien immer in `.gitignore` eintragen
- ✅ Code-Reviews: bewusst auf versehentlich eingecheckte Credentials achten

---

## 🧩 Nützliche Links

- 🐷 [TruffleHog OSS auf GitHub](https://github.com/trufflesecurity/trufflehog)
- 🧩 [TruffleHog GitHub Action (Marketplace)](https://github.com/marketplace/actions/trufflehog-oss)
- 📘 [Blog: TruffleHog in GitHub Actions](https://trufflesecurity.com/blog/running-trufflehog-in-a-github-action)
- 🔐 [GitHub Actions Dokumentation](https://docs.github.com/en/actions)
