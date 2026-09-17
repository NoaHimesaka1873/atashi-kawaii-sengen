# atashi-kawaii-sengen

Source for [noa.codes](https://noa.codes) — Noa Himesaka's personal homepage.
One static page (plus error pages) about who she is, what she runs and what she
builds, written in the same design system as the rest of the
[YuruVerse](https://funami.tech): [YuruMirror](https://github.com/funamitech/mirror),
[funami.tech](https://github.com/funamitech/yuruverse) and
[mc.funami.tech](https://github.com/funamitech/YuruMC).

The repo is named after the song アタシ♡カワイイ♡宣言!!! ("The I'm Cute Proclamation"),
whose lyrics are at the bottom of this file, where they have always been and where
they are staying.

## Tech stack

- Tailwind CSS 4 (CSS-first config in `src/assets/css/input.css` — no `tailwind.config.js`)
- A tiny template builder (`build.js`) that expands shared partials into static pages
- Vanilla JS, self-hosted Inter variable font, inline SVG icon sprite — **no CDNs, no frameworks**
- Served by plain nginx (the vhost also 301s `/.well-known/{webfinger,host-meta,nodeinfo}`
  to social.noa.codes for the `@himesaka@noa.codes` fediverse handle; this repo only owns `/`)

The 2020–2022 Bootstrap Studio version (Bootstrap 4, jQuery, CDN icon fonts, Google
Fonts) is preserved on the `legacy` branch.

## Project structure

```
atashi-kawaii-sengen/
├── templates/            # HTML sources — EDIT THESE
│   ├── partials/         # head, nav, footer, icon sprite, error-page shell
│   ├── index.html
│   └── error/            # 403 / 404 / 50x (one include line each)
├── src/                  # deployable webroot — generated pages + static assets
│   └── assets/           # css (input.css + built tailwind.css), js, fonts, img
├── build.js              # expands templates/ -> src/
└── dev/server.js         # dev server: re-expands templates on every request
```

`src/*.html`, `src/error/*.html` and `src/assets/css/tailwind.css` are build artifacts
(committed so that `src/` can be deployed as-is). Edit `templates/` and `input.css`,
then rebuild.

## Development

```bash
npm install
npm run dev          # http://localhost:8080 — templates re-expand on every refresh
npm run build-css    # Tailwind in watch mode (run alongside `npm run dev`)
```

Append `?theme=dark` or `?theme=light` to any page to force a color scheme while
testing (this works on the production site too; it is handled by the inline script in
`partials/head.html` and is not persisted).

No Node? `python3 -m http.server 8080 --directory src` serves the built site just as well.

## Build & deploy

```bash
npm run build        # expand templates + minified CSS
```

Deploy the `src/` directory as the webroot. Wire the error pages up like the sister
sites do:

```nginx
error_page 403 /error/403.html;
error_page 404 /error/404.html;
error_page 500 502 503 504 /error/50x.html;
```

The root `CNAME` file belongs to the old GitHub Pages setup and only matters until
noa.codes is served from nginx.

## Editing the page

Everything is in `templates/index.html`, in four sections: hero, `#about`,
`#services` (what she runs), `#projects` (what she builds) and `#elsewhere`
(where to find her). Links and counts in there are facts — check them before
changing them:

1. `templates/partials/nav.html` — the "Stuff I run" dropdown + mobile menu
2. `templates/partials/footer.html` — "Around the web" and "Stuff I run" columns
3. `templates/partials/icons.html` — inline SVG sprite; add real paths, never an icon font

## Scripts

- `npm run dev` — dev server
- `npm run build` — templates + minified CSS (production)
- `npm run build-css` — CSS watch mode
- `npm run lint` / `npm run lint:fix` — ESLint

## License

MIT — see [LICENSE](LICENSE).

---

## アタシ♡カワイイ♡宣言!!!, The "I'm Cute" Proclamation

[Link to music](https://www.youtube.com/watch?v=YvHmHadBpi0)  
Go round and round  
the merry-go-around  
I’m the first in the world!  
So very very cute, adorable even  
Please, someone notice!  
  
La-la-la, Love me do!  
  
Like a princess  
My form like royalty  
should turn anyone’s head  
Please don’t be jealous  
I have killer smile, warning: you’ll love it  
This is an official proclamation  
Look, you’re now a captive too  
  
Oh, it’s a sin, my beauty  
No matter how much you look, you won’t be bored  
So look! The fastest wins!  
  
Go round and round  
the merry-go-around  
I’m the first in the world!  
So very very cute, adorable even  
Please, someone notice!  
  
Always, always this planet  
orbits around me  
So surely, no mistake  
If I get serious, I’ll make anyone fall for me an instant  
  
La-la-la, Love me do!  
  
Hey hey, everyone, look, look!  
Aren’t I cute?  
Speak up, praise me more  
and I’ll return a wink for you  
  
Oh, why is the reception so cold  
No matter, I won’t give up  
The happy end is right there  
  
Listen  
A little love fantasy  
I’m the main heroine  
So please hug me tight  
Prince on a white horse  
  
Finally, the long-awaited  
chance to wed me is here!  
A deluge of applications is a certainty  
If you look at my eyes, anyone, with one look, falls for me!  
  
Hey, everyone!  
Who is the cutest in the world?  
Of course, it’s me! (Yes, yes)  
What, I’m not?! Lies! That’s impossible!  
That can’t be true in any shape or form!  
Because I am the cutest in the world!  
  
I am, I am cute to this degree  
Even if you search the whole world, you won’t find one like me  
Why won’t anyone acknowledge it?  
At least if you noticed it…  
  
Go round and round  
the merry-go-around  
I’m the first in the world!  
So very very cute, adorable even  
Please, someone notice!  
  
Always, always this planet  
orbits around me  
So surely, no mistake  
If I get serious, I’ll make anyone fall for me an instant  
  
La-la-la, Love me do!  
