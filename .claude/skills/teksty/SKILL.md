---
name: teksty
description: Pobiera teksty strony z Google Docs „Gwar dla firm – teksty strony”, kopiuje je do Claude Doc o tej samej nazwie pokazuje listę planowanych zmian i po zgodzie właściciela wdraża je na ofertafirmowa.gwar.bar (index.html), łącznie z poleceniami dopisanymi w dokumencie. Użyj, gdy właściciel pisze /teksty, „zsynchronizuj teksty”, „zerknij na zmiany w docs” itp.
---

# /teksty: Google Docs → Claude Doc → strona

Właściciel poprawia teksty w Google Docs. Ten skill przenosi je do Claude Doc (kopia robocza) i na stronę, a polecenia dopisane w dokumencie wykonuje.

## Źródła

| Co | Gdzie |
|---|---|
| Google Doc (źródło prawdy) | fileId `1o4DwZJBE9fDqM0w9hWRAGmB2_eLsamzCrBj1PeA2V2c` |
| Claude Doc (kopia) | container `{"kind":"project","id":"e4190f0f-c1bf-43a2-8555-f22d3562d3b2"}`, https://claude.ai/code/artifact/e4190f0f-c1bf-43a2-8555-f22d3562d3b2 |
| Ostatnio wdrożona wersja | `copy/teksty.md` w tym repo (tekst Google Doca po poprzednim przebiegu) |
| Strona | `index.html` w tym repo, gałąź `main` → GitHub Pages → ofertafirmowa.gwar.bar |
| Podgląd | Artifact https://claude.ai/artifact/MCJXB2f2W3rwRrpEhXHHQB |

## Kroki

1. **Pobierz Google Doc**: `mcp__Google_Drive__read_file_content` z `includeComments: true`. Zapisz `fileContent` do `copy/teksty.new.md`.
2. **Znajdź zmiany**: `diff -u copy/teksty.md copy/teksty.new.md`. Brak różnic i brak komentarzy → powiedz to w jednym zdaniu i zakończ.
3. **Podziel zmiany** na:
   - **teksty**: zmieniona treść w tabelach/listach → odpowiadający tekst w `index.html` (grep po starej wersji);
   - **polecenia**: zdania-instrukcje dopisane do dokumentu lub komentarze (np. „Zamień zdjęcia…”, „usuń…”) → zadania do wykonania;
   - **[DO UZUPEŁNIENIA] wypełnione**: zamień `<span class="todo">…</span>` na podany tekst; odhacz pozycję w liście „Do uzupełnienia”.
4. **Potwierdź z właścicielem, zanim cokolwiek zmienisz.** Pokaż po polsku listę planowanych zmian, każdą w jednej linii: sekcja, „było → będzie”, a dla poleceń co konkretnie zrobisz (np. które zdjęcie na którą kartę). Niejasności podaj z Twoją propozycją. Zapytaj (AskUserQuestion: „Wykonaj wszystko” / „Wykonaj z poprawkami” / „Nie teraz”) i **czekaj na odpowiedź**. Nie edytuj plików, Claude Doc, nie commituj i nie publikuj przed zgodą. Wykonaj tylko to, co zatwierdził; przy „Nie teraz” usuń `copy/teksty.new.md` i zakończ.
5. **Wdróż na stronę**: zmiany w `index.html` minimalne, w stylu otoczenia (`&nbsp;` po jednoliterowych spójnikach w akapitach, nagłówki pisane normalnie – wersaliki robi CSS). Rzeczy niejasne lub sprzeczne ze stroną (np. tabela w docu nie pasuje już do układu kart) → zrób najbardziej sensowną wersję i nazwij decyzję w podsumowaniu.
   - Teksty o alkoholu są OK tylko tutaj. Nigdy nie przenoś niczego do repo `trysti/gwar-landing` (landing nie może mieć treści alkoholowych).
   - Zdjęcia z Dysku: pobieranie przez MCP działa tylko do ok. 5,5 MB. Większe → poproś o wrzucenie pliku na czat (w osobnej wiadomości, nie w trakcie pracy). Konwersja: `convert in.jpg -strip -resize 800x -quality 80 media/NAZWA-800.webp` i wersja `-1600` (`-resize 1600x\>`).
6. **Sprawdź**: serwer `python3 -m http.server 8091` w katalogu repo, zrzuty Playwright (`PW=$(npm root -g)/playwright`) zmienionych sekcji na 1440×900 i 390×844; zero błędów konsoli, zero poziomego overflow, brak zepsutych obrazków. Obejrzyj zrzuty.
7. **Zaktualizuj Claude Doc**: wczytaj go (`mcp__Claude_Docs__read`, node body) i podmień zmienione bloki tak, by treść odpowiadała Google Docowi (bez polecenia-instrukcji, które już wykonano). Najpierw `mcp__Claude_Docs__guide` z `["topic.editing"]`, jeśli nie znasz składni.
8. **Zapisz bazę**: `mv copy/teksty.new.md copy/teksty.md`.
9. **Commit i push** na `main` (`git push origin main`), komunikat po angielsku, co zmieniono.
10. **Odśwież podgląd**: zbuduj fragment bez doctype/html/head/body
   `node -e "const fs=require('fs');let h=fs.readFileSync('index.html','utf8');h=h.replace(/<!doctype html>/i,'').replace(/<\/?html[^>]*>/gi,'').replace(/<\/?head>/gi,'').replace(/<\/?body[^>]*>/gi,'');fs.writeFileSync(process.argv[1],h)" <scratchpad>/gwar-dla-firm.html`
   i opublikuj Artifact z `url` podglądu, `root` = repo, w `files` nowe media.
11. **Podsumowanie po polsku** dla właściciela: co zmieniono na stronie, jakie polecenia wykonano, czego nie dało się zrobić i dlaczego. Krótko.
