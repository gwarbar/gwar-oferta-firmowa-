# ofertafirmowa.gwar.bar

Oferta dla firm Bar Gwar: wigilie, integracje, wynajem na wyłączność, z koktajlami jako główną pozycją.

**Osobne repo, poza landingiem.** `landing.gwar.bar` (repo trysti/gwar-landing) ma Content Blacklist (zero alkoholu) i reklamy Google Ads;
ta strona jest od niego niezależna, ma `noindex` i nie jest podpinana pod Google Ads.

- statyczny HTML, jeden plik `index.html`, bez buildu; animacje: GSAP 3.13 (ScrollTrigger, SplitText) w `vendor/`
  (darmowa licencja „Standard no-charge” GSAP, także do użytku komercyjnego)
- przejścia przy scrollu: lustro z sali rośnie do pełnego ekranu i delikatnie oddala się w kadr, roleta (pasy od środka) przed okazjami,
  finał „GWAR” z wideo w literach, w które się wlatuje, nalewanie sterowane scrollem (48 klatek w `media/pour/` na canvasie),
  stos kart koktajli (sticky + GSAP), płynna zmiana koloru tła między sekcjami, reveal'e w CSS `animation-timeline: view()`
  (fallback GSAP); `ScrollTrigger.config({ ignoreMobileResize: true })` przeciw skokom paska adresu w Safari
- `media/` – zdjęcia z sesji (Drive „photo”) w WebP 800/1600 px, `bar-loop.mp4` – 4 klipy z barmanem (Drive „video”)
- krój Jean-Luc i kolory marki jak na landingu; logotyp z `logo.svg` (maska CSS, także w finale)
- intro przy pierwszym wejściu: promienie G (`g-top`, wstawione inline) wystrzeliwują i odlatują do logo w nagłówku
  (flaga `gwar-intro` w localStorage, pomijane przy „ogranicz ruch”); szklanka w rogu jako pasek postępu scrolla;
  przyciski napełniają się od dołu po najechaniu
- `brand/` – znaki G z „Gwar-G.ai” (`g-left`, `g-top`, `g-right`, `g-bottom`) i kontur naklejki G z dłonią (`g-sticker.svg`,
  rysowany scrollem w różowej sekcji); G-rozbłysk (`g-top`) jako separator w pasku haseł

Podgląd lokalnie: `python3 -m http.server 8090` → http://localhost:8090/

## Do uzupełnienia (żółte znaczniki na stronie)

- karta koktajli: nazwy i składy z „Menu Firmowe GWAR” (Claude Design)
- pakiety: skład i ceny
- e-mail do ofert, `LEAD_EMAIL` w skrypcie na dole `index.html` (formularz składa gotową wiadomość do wysłania mailem; brak backendu)
- czas odpowiedzi, zaliczka, faktura VAT, catering, obsługa na eventach, tort/dekoracje, warsztaty koktajlowe
- link do polityki prywatności

## Publikacja

GitHub Pages: *Settings → Pages → Deploy from a branch*, `main` / `(root)`. Każdy push do `main` publikuje stronę
(plik `.nojekyll` wyłącza Jekylla). Domena: plik `CNAME` + rekord DNS `CNAME ofertafirmowa → gwarbar.github.io`.
Po wydaniu certyfikatu włączyć *Enforce HTTPS*.
