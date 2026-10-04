# Base44 Dev Environment

## Project
Static HTML site — a French/Korean language learning app ("Enrichir son expression (1)"). Pages are standalone HTML files using Tailwind CSS (CDN), Phosphor icons (CDN), and Tone.js (CDN). No build step, no backend, no package manager.

## Running the app
```
docker compose -f docker-compose.base44.yml up -d
```
Serves all HTML files via nginx on port 3000. An `index.html` lists all lesson pages.

## Why nginx runs as root
The repo root directory has mode 700. nginx's default worker user (`nginx`, UID 101) cannot traverse it, causing 403. The custom `nginx.base44.conf` sets `user root;` so workers can read the bind-mounted files.

## Pages
- `index.html` — navigation page listing all lessons
- `1.html`, `1-1.html` — Chapitre 1, Leçon 1 (and variant)
- `2.html` — Chapitre 1, Vocabulaire
- `5.html` — Chapitre 2, Leçon 5
- `6.html` — Chapitre 2, Vocabulaire
- `9.html` — Chapitre 3, Leçon 9
- `10.html` — Chapitre 3, Vocabulaire

## Verification
- `curl -s -o /dev/null -w "%{http_code}" http://localhost:3000/` → 200
- `curl -s -o /dev/null -w "%{http_code}" http://localhost:3000/1.html` → 200

## No secrets required
The app is fully static with no external credentials needed.
