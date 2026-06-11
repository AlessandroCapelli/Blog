# Blog di Alessandro Capelli

Blog statico, zero JavaScript, zero temi esterni. Markdown puro, dark mode automatica, feed RSS su `/feed.xml`.

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

1. Crea un repo su GitHub. Due opzioni:
    - `TUOUSERNAME.github.io` → blog su `https://TUOUSERNAME.github.io`
    - qualsiasi altro nome (es. `blog`) → blog su `https://TUOUSERNAME.github.io/blog`
2. In `_config.yml` imposta `url` e `baseurl`:
    - repo `TUOUSERNAME.github.io`: `url: https://TUOUSERNAME.github.io`, `baseurl: ""`
    - repo `blog`: `url: https://TUOUSERNAME.github.io`, `baseurl: /blog`
3. Push di tutti i file sul branch `main`:
    ```bash
    git init && git add . && git commit -m "primo post"
    git branch -M main
    git remote add origin git@github.com:TUOUSERNAME/NOME-REPO.git
    git push -u origin main
    ```
4. Su GitHub: **Settings → Pages → Source: Deploy from a branch → main / (root)**.
5. GitHub compila Jekyll da solo, nessuna build locale necessaria.

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
