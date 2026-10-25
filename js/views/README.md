# Vocab 2

A vocabulary trainer that runs in your browser. It has no grammar, no course and no built-in words. You import your own word lists in any language pair and train them. It works offline and can be installed on a phone as an app (PWA).

> **Vibe coded.** This project was built with AI assistance (Claude Code) by prompting and iterating, not by writing every line by hand. It is programmed based on **LanguageLearningApp**, an earlier language learning project, and reuses its ideas: the levels, the answer checking and the overall look. Expect rough edges, and read the code before you trust it with anything important.

## Features

- Import your own words from text, CSV or JSON, for any language pair
- Practice in either direction, or mixed
- Four levels per word: New, Learning, Familiar, Mastered
- Answer checking with a "why it was wrong" explanation
- Stats and daily activity history
- Several profiles, no passwords, all data stays in your browser
- Backup and restore as a file
- Installable and works offline

## Run it

No build step and no dependencies. Serve the folder and open it:

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000`.

## How it works

- **Profiles**: names only, no passwords. Everything is saved in this browser under `vocab2.<profileId>.<key>`. Nothing is sent to a server.
- **Import** (Decks › Import): paste text or upload a file. The first side is in "your language" and the second side is in the language you're learning. **Swap the sides** flips a list that is written the other way round.
- **Stats** on import:
  - **Automatic**: new words start at 0, words already in the deck keep their stats, and stats saved in the file are used when they are newer.
  - **All to 0**: every imported word goes back to New, even words you already practised.
- **Stats to 0** on a deck resets every word in it. Daily activity history stays.
- **Levels**: New, then Learning (0 to 2 correct in a row), then Familiar (3 to 4), then Mastered (5). A wrong answer resets the streak.
- **Direction**: you can practise your language → target, target → your language, or mixed. Set it on Home, in the practice sheet or in Settings.
- **Download** on a deck gives you a JSON file with the words only, or with your stats. Import it again anywhere.

Your progress lives only in your browser. Clearing site data or switching phones loses it, so use the backup in Settings.

## Import formats

```json
[{"q": "pes", "a": "dog"}, {"q": "kočka", "a": ["cat", "kitty"]}]
{"lang": "en", "from": "cs", "title": "Animals", "words": [{"q": "pes", "a": "dog|hound"}]}
{"pes": "dog", "kočka": "cat"}
```

The importer also accepts `question/answer`, `front/back`, `term/definition` and `word/translation`. In text or CSV, put one pair per line, separated by `;` `,` `=` ` - ` or a tab. Separate extra answers with `|`.

An exported deck with stats carries a `stats` object per word (`streak`, `right`, `wrong`, `lastOk`, `last`, `due`). Bad values are dropped, so a broken file can only start words at 0.

## Optional: open from a dashboard

`index.html?profile=<id>&name=<name>&lang=<code>&home=<dashboard url>` signs straight into the profile with that id and creates it on the first visit. It also keeps the language selected even before it has words, and adds a **School** tab that links back to `home`. `home` must be on the same site; anything else is ignored. If you don't use a dashboard, ignore this.

## Files

| File | What it does |
|---|---|
| `js/data.js` | parsing imports, saving decks, stats from files, export |
| `js/srs.js` | levels, due dates, session picking |
| `js/check.js`, `js/explain.js` | answer checking and "why it was wrong" |
| `js/store.js`, `js/backup.js` | local storage, profiles, backup and restore |
| `js/embed.js` | dashboard URL parameters |
| `js/views/*.js` | one file per screen |
| `sw.js` | service worker for offline use |

Version is in `version.js`. Bump it on every push you want to see on your phone.

## Deploy on GitHub Pages

The repo is static files only, so Pages works as is. In the repo go to **Settings › Pages**, choose **Deploy from a branch**, pick `main` and `/ (root)`, and save. The site appears at `https://<your-username>.github.io/<repo-name>/`.

## License

Released under the [MIT License](LICENSE).
