# In Other Words

A free, no-signup web translator — type text, pick a language pair, and get a translation instantly. It's a single self-contained HTML file, so there's nothing to install and nothing to build.

## Features

- Translate between 30+ languages
- Swap source/target languages with one click
- Copy the translation to your clipboard
- Listen to the original text or the translation (browser text-to-speech)
- Automatic chunking for longer text (the free API caps each request at 500 bytes)
- No API key, no signup, no backend — works from a single static HTML file

## How it works

This project uses the [MyMemory Translation API](https://mymemory.translated.net/doc/spec.php), a free REST API that requires no key. The page sends a `GET` request per chunk of text:

```
https://api.mymemory.translated.net/get?q=<text>&langpair=<source>|<target>
```

Because MyMemory limits each request to 500 bytes of text, longer input is automatically split at word boundaries into chunks, translated one at a time, and stitched back together. Text-to-speech uses the browser's built-in `SpeechSynthesis` API, so voice availability depends on your browser and OS.

## Getting started

### Run it locally

Just open `in-other-words.html` in any browser — no install, no build step. If your browser blocks the request when opened this way (rare), run a quick local server instead:

```bash
python3 -m http.server
```

then visit `http://localhost:8000/in-other-words.html`.

### Deploy on GitHub Pages

1. Push this file to a GitHub repository.
2. Go to **Settings → Pages**.
3. Under "Build and deployment," set **Source** to **Deploy from a branch**, choose the `main` branch and the `/ (root)` folder, then **Save**.
4. GitHub publishes it at `https://<your-username>.github.io/<repo-name>/in-other-words.html` (or at the plain folder URL if you rename the file to `index.html`).

## Configuration

Near the top of the `<script>` tag in `in-other-words.html`:

```js
var CONTACT_EMAIL = ''; // optional
```

MyMemory's free tier:

| Usage | Daily limit |
|---|---|
| Anonymous (default) | 5,000 characters |
| With an email set in `CONTACT_EMAIL` | 50,000 characters |

No signup is required either way — adding an email is just a query parameter that raises the quota.

## Known limitations

- **No automatic language detection.** MyMemory doesn't support it, so both languages must be picked manually.
- **500-byte request cap**, worked around via chunking — very long pastes trigger multiple sequential API calls, which can be slower.
- Translation quality depends entirely on MyMemory's free translation memory / machine translation — for anything important, have a fluent speaker double-check it.

## Tech stack

Vanilla HTML, CSS, and JavaScript. No frameworks, no build tools, no dependencies.

## Credits

Translations powered by [MyMemory](https://mymemory.translated.net/).
