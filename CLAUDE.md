# CLAUDE.md

Oferta dla firm Bar Gwar (Mostowa 8, Kazimierz, Kraków): wigilie, integracje, wynajem na wyłączność, z koktajlami
jako główną pozycją → `ofertafirmowa.gwar.bar`. Szczegóły dla ludzi: `README.md`.

## Polecenia

```bash
python3 -m http.server 8090   # podgląd: http://localhost:8090/
/teksty                       # skill: Google Docs → Claude Doc → index.html (.claude/skills/teksty/SKILL.md)
```

Brak buildu, testów i lintera. Każdy push do `main` od razu publikuje stronę (GitHub Pages, `main` / root,
`.nojekyll`, `CNAME`), więc przed commitem sprawdź zmianę w przeglądarce.

## Struktura

| Gdzie | Co |
| --- | --- |
| `index.html` | cała strona: treść, CSS w `<style>` (tokeny w `:root`), skrypty na dole |
| `vendor/` | GSAP 3.13 + ScrollTrigger + SplitText (lokalne kopie, nie CDN) |
| `media/` | zdjęcia WebP w parach `-800` / `-1600`, `bar-loop.mp4` + poster, `pour/` – 48 klatek nalewania |
| `brand/` | znaki G (`g-top`, `g-left`, `g-right`, `g-bottom`) i `g-sticker.svg` |
| `fonts/jeanluc-bold.woff2`, `logo.svg` | krój nagłówków i logotyp |
| `copy/teksty.md` | ostatnio wdrożona wersja tekstów z Google Doca – baza dla `/teksty`, nie edytuj ręcznie |

## Twarde zasady

1. **Osobno od landingu.** Treści alkoholowe (koktajle, karta) są OK tylko tutaj. Nigdy nie przenoś niczego
   z tego repo do `trysti/gwar-landing` (Content Blacklist, Google Ads). Strona ma `noindex` – zostaw.
2. **Nie zmyślaj danych.** Ceny, skład pakietów, e-mail, zaliczka, VAT, catering itp. zostają jako
   `<span class="todo">…</span>` (żółty znacznik), dopóki właściciel ich nie poda. Lista otwartych pozycji:
   README → „Do uzupełnienia” – po wypełnieniu znacznika odhacz ją tam.
3. **Teksty pochodzą z Google Doca.** Zmiany treści idą przez `/teksty` (ze zgodą właściciela przed wdrożeniem).
   Ręczna zmiana tekstu w `index.html` rozjedzie się z dokumentem przy następnej synchronizacji.
4. **Formularz bez backendu.** `#lead` składa wiadomość do `mailto:` (`LEAD_EMAIL` w skrypcie na dole) – nie
   dodawaj zewnętrznych usług formularzy ani trackerów bez prośby właściciela.
5. **Ruch i dostępność.** Każda animacja musi mieć wersję dla `prefers-reduced-motion: reduce`; nowe animacje GSAP
   dodawaj w `mm.add('(prefers-reduced-motion: no-preference)', …)`. Kolory tylko z tokenów `:root`, kontrast AA
   (`--pink-text` do tekstu na ciemnym tle).
6. **Zdjęcia.** Nowe zdjęcie = dwie wersje WebP: `convert in.jpg -strip -resize 800x -quality 80 media/NAZWA-800.webp`
   i `-resize 1600x\>` → `-1600`; w HTML `srcset` z obiema, `alt` po polsku.

## Konwencje

- Tekst po polsku, `&nbsp;` po jednoliterowych spójnikach/przyimkach w akapitach; nagłówki pisane normalnie
  (wersaliki robi CSS).
- Komentarze w kodzie jak w otoczeniu; zmiany minimalne, w stylu istniejącego CSS/JS.
- Sprawdzenie zmiany: zrzuty Playwright (`PW=$(npm root -g)/playwright`) na 1440×900 i 390×844 – zero błędów
  konsoli, zero poziomego overflow, brak zepsutych obrazków.
- Commity: krótki tryb rozkazujący po angielsku, push na `main`.
