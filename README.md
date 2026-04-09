# ExamOS-2027 — Eindexamen Training Template

**Clean template** voor VWO/HAVO eindexamen training. Fork, deploy, vul de wizard in en je hebt een persoonlijk trainingssysteem in 5 minuten.

[![License](https://img.shields.io/badge/license-MIT-blue?style=flat-square)](LICENSE)
[![Built with Claude](https://img.shields.io/badge/built%20with-Claude%20Code-9d97ff?style=flat-square)](https://claude.com/claude-code)
[![Template](https://img.shields.io/badge/template-fork%20%26%20customize-6c63ff?style=flat-square)](#getting-started)

> Gewogen studieplanner · 7-stap setup wizard · AI nakijken · Foutenanalyse · Sterrengamification

Dit is de **template versie** van [ExamOS](https://github.com/ColdDesertLab/ExamOS). Het ships met demo data zodat je direct kunt rondklikken, en heeft een first-run wizard die de demo overschrijft met je eigen profiel.

---

## 🚀 Getting Started

### 1. Probeer de demo

Open de [live demo](https://examos-2027.vercel.app) of clone lokaal:

```bash
git clone https://github.com/ColdDesertLab/ExamOS-2027.git
cd ExamOS-2027
python3 -m http.server 8000
# open http://localhost:8000/ExamOS.html
```

**Demo login:** `demo` / `demo`

Na inloggen verschijnt de **setup wizard** automatisch. Je kunt deze overslaan om de demo data te verkennen, of meteen je eigen profiel invullen.

### 2. Self-host installer

```bash
curl -fsSL https://raw.githubusercontent.com/ColdDesertLab/ExamOS-2027/master/install.sh | bash
examos-2027
```

### 3. Add to Home Screen (PWA)

Werkt op iPad, iPhone, macOS en Android — open de URL in je browser en kies "Zet op beginscherm" / "Installeren". Zie [PWA Setup](#-pwa-setup) hieronder.

---

## 🧙 Setup Wizard

Bij eerste gebruik (of via **Instellingen → Wizard opnieuw doen**) verschijnt een 7-stap wizard:

1. **Welkom** — uitleg van het systeem
2. **Naam** — je voornaam (verschijnt in notificaties + dashboard)
3. **Vakken** — selecteer uit 19 VWO/HAVO vakken (Wiskunde A/B/C/D, Natuurkunde, Scheikunde, Biologie, Economie, M&O, Geschiedenis, Aardrijkskunde, Maatschappijleer, Filosofie, Kunst, Nederlands, Engels, Duits, Frans, Spaans, Latijn, Grieks)
4. **SE Cijfers** — vul je huidige schoolexamen gemiddelde per vak in
5. **Examendatums** — datum, tijd en duur per vak (uit je officiële rooster)
6. **Topics** — hoofdonderwerpen per vak (vooringevuld met defaults, inline bewerkbaar)
7. **Klaar** — samenvatting → klik "Plan genereren"

Bij voltooien:
- Demo data wordt gewist
- Je nieuwe vakken + grades worden opgeslagen
- Een gewogen studieplan wordt automatisch gegenereerd op basis van score deficit + tijdsdruk
- `state.wizardCompleted = true` zodat de wizard niet opnieuw verschijnt

---

## 📱 PWA Setup

### iPad / iPhone
1. Open in Safari → tap deel-knop → **"Zet op beginscherm"**
2. ExamOS verschijnt als native app, full-screen, eigen icoon

### macOS (Chrome / Edge / Arc)
1. Klik ⋮-menu → **"ExamOS installeren..."**
2. Verschijnt in Applications + Launchpad → sleep naar Dock

### macOS (Safari)
1. **Bestand** → **"Toevoegen aan Dock..."**

### Android
1. Chrome → ⋮ → **"App installeren"**

---

## ☁️ Cloud Sync (optioneel)

Deze template heeft cloud sync **uitgeschakeld** by default (alleen localStorage). Om cross-device sync te activeren:

1. Maak een gratis Supabase project aan op [supabase.com](https://supabase.com)
2. Maak een tabel `examos_state`:
   ```sql
   create table examos_state (
     id text primary key,
     state jsonb,
     updated_at timestamptz default now(),
     version int default 0
   );
   insert into examos_state (id) values ('default');
   alter table examos_state enable row level security;
   create policy "open" on examos_state for all using (true);
   ```
3. Vul in `ExamOS.html` (regel 2180):
   ```javascript
   const SUPABASE_URL = 'https://your-project.supabase.co';
   const SUPABASE_KEY = 'your-anon-key';
   ```
4. Deploy + log in op meerdere apparaten → sync werkt automatisch

---

## 🎯 Features

- **Setup wizard** (7 stappen) voor eerste gebruik
- **Gewogen budget planner** — zwakke vakken krijgen automatisch meer sessies
- **Examen-bewuste scheduling** — Pomodoro timer, verschillende dagtypes (zaterdag, zondag, examenweek, dag-na, etc.)
- **AI nakijken** (optioneel, met Claude/GPT) — upload examens → krijg automatische scoring + foutendetectie
- **Sterren & streaks** — gamification voor consistentie
- **Bonus activiteiten** na dag-klaar — extra sessie, fouten herhaling, theorie uitleg, flashcard quiz
- **Studie verdeling tabel** in Instellingen — interactieve aanpassing van gewicht per vak
- **Foutenlog** met F1-F8 codes en automatische actiepunten
- **Notities + Flashcards + Quiz generator**
- **PWA** — installeerbaar op iPad, iPhone, macOS, Android
- **Cross-device sync** via Supabase (optioneel)

## 📂 Bestanden

| Bestand | Functie |
|---------|---------|
| `ExamOS.html` | Hoofdbestand (~6500 regels, single-file app) |
| `index.html` | Kopie voor static hosting |
| `manifest.json` | PWA manifest |
| `install.sh` | Self-host installer |
| `README.md` | Dit bestand |

## 🛠️ Tech Stack

| Laag | Technologie |
|------|-------------|
| Frontend | Single HTML, vanilla JS, geen frameworks, geen build step |
| Styling | CSS custom properties, dark/light/auto theme, responsive |
| Data | localStorage + optionele Supabase sync |
| AI | Anthropic Claude API + OpenAI API (optioneel) |
| Hosting | Vercel / Netlify / GitHub Pages / self-host |
| PWA | Manifest + canvas-generated icons |

## 🆚 ExamOS 2026 vs 2027

| | **2026** | **2027** |
|---|---|---|
| Doelgroep | Specifiek voor Jussi (VWO 2026) | Iedereen — fork & customize |
| Vakken | Hardcoded (8 vakken) | Wizard-configured (19 presets) |
| Login | `Jussi / Jones2026!` | `demo / demo` |
| Examendatums | Hardcoded mei 2026 | Wizard-configured |
| Cloud sync | Geactiveerd (Supabase) | Uit (zelf in te stellen) |
| Wizard | Optioneel via Instellingen | Verplicht bij eerste gebruik |

[ExamOS 2026 →](https://github.com/ColdDesertLab/ExamOS)

## License

MIT — fork, customize, deploy. Geen attributie vereist.
