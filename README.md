# Malayalam Box Office — North America

A private dashboard tracking North American box office performance for Malayalam films. Built as a single-page HTML application backed by Supabase.

## What it does

- Tracks 20 films from Neru (Dec 2023) through Karuppu (May 2026)
- Metro-level gross, theater count, avg per theater, top theater
- Geographic bubble map (Plotly)
- Circuit breakdown with estimated distributor share
- Opening weekend vs. total gross legs comparison
- US / Canada market filter
- USD ↔ INR toggle

## Stack

- Plain HTML + CSS + vanilla JS — no framework, no build step
- [Plotly.js](https://plotly.com/javascript/) for the map
- [Supabase](https://supabase.com) for data (PostgreSQL + RLS)
- Deployed on [Vercel](https://vercel.com)

## Project structure

```
/
├── index.html        # Entire application
├── vercel.json       # Vercel routing + content-type headers
└── README.md
```

## Database schema

Four tables in Supabase:

| Table | Description |
|---|---|
| `films` | Film metadata — id, label, badge, release date |
| `metros` | Metro-level box office — gross, theaters, top theater, coordinates |
| `theaters` | Theater-level drill-down — opening weekend, total gross |
| `circuits` | Circuit breakdown — locations, gross, settlement % by US/CA |

Row Level Security is enabled on all tables with public read access.

## Local development

No build step required. Open `index.html` directly in a browser, or use any static server:

```bash
npx serve .
```

The Supabase anon key is embedded directly in `index.html` — it is safe to expose (RLS enforces access at the database level).

## Adding a new film

1. Insert a row into the `films` table
2. Insert metro rows into `metros`
3. Insert theater rows into `theaters`
4. Insert circuit rows into `circuits`
5. Add the film to the `<select>` dropdown in `index.html`

No code changes required beyond the dropdown entry.

## Access

The dashboard is protected by a client-side password gate. This provides casual friction — it is not a substitute for server-side auth. Data is readable by anyone with the anon key and knowledge of the table names.
