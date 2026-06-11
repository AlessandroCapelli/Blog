# Blog di Alessandro Capelli

Blog statico, zero JavaScript, zero temi esterni. Markdown puro, dark mode automatica, feed RSS su `/feed.xml`.

**Live:** https://alessandrocapelli.github.io/Blog/

## Struttura

```
_config.yml
_layouts/
  default.html
  post.html
_posts/
assets/
  css/style.css
  images/
index.html
```

## Deploy su GitHub Pages

Il repo è già configurato (`_config.yml`): `url: https://alessandrocapelli.github.io`, `baseurl: Blog`.

1. Push sul branch `main`.
2. Su GitHub: **Settings → Pages → Source: Deploy from a branch → main / (root)**.
3. GitHub compila Jekyll da solo, nessuna build locale necessaria.

## Scrivere un nuovo post

Crea un file in `_posts/` chiamato `AAAA-MM-GG-titolo-breve.md`:

```markdown
---
layout: post
title: "Il titolo del post"
date: 2026-06-12
---

Il contenuto in Markdown puro. Le immagini vanno in assets/images/ e si
inseriscono così:

![Descrizione]({{ "/assets/images/nome-file.svg" | relative_url }})
```

Commit, push, online. Tutte le pagine usano lo stesso layout `post.html`.
