# 007 Advent Calendar

Responsywna aplikacja PWA z 25 filmami EON, lokalnymi plakatami i kalendarzem adwentowym. Dane są przechowywane w IndexedDB przeglądarki. Projekt nie używa backendu ani zewnętrznych bibliotek.

## Struktura

```text
index.html
manifest.webmanifest
sw.js
favicon.svg
src/
  app.js          # logika, IndexedDB, historia i ustawienia
  movies.js       # konfiguracja 25 filmów i plakatów
  styles.css      # responsywny interfejs
public/assets/posters/  # 25 lokalnych plakatów WebP
```

## Uruchomienie lokalne

Service Worker i instalacja PWA wymagają bezpiecznego kontekstu HTTP(S); nie uruchamiaj aplikacji przez `file://`.

1. Uruchom w katalogu projektu `py -m http.server 8000` (na systemach z Pythonem dostępnym bez launchera: `python -m http.server 8000`).
2. Otwórz `http://localhost:8000`. Po pierwszym załadowaniu aplikacja zapisze pliki i plakaty do cache.

Nie ma kroku budowania ani zależności aplikacji do instalowania. W produkcji publikuj zawartość katalogu projektu jako statyczną stronę HTTPS.

## GitHub Pages

1. Umieść wszystkie pliki w repozytorium GitHub i wypchnij je do wybranej gałęzi.
2. W ustawieniach repozytorium otwórz **Pages** i wskaż gałąź oraz katalog `/ (root)` (albo katalog publikacyjny, jeśli przeniesiesz tam całość aplikacji).
3. Poczekaj na publikację. Odnośniki są względne (`./`), więc aplikacja działa również pod ścieżką repozytorium `https://uzytkownik.github.io/nazwa-repo/`.
4. Otwórz adres HTTPS i poczekaj na pobranie cache; następnie można uruchamiać aplikację offline.

## Plakaty

Wszystkie 25 dostarczonych grafik WebP znajduje się w `public/assets/posters/`. Ich nazwy są zgodne z wpisami w `src/movies.js`. Aby podmienić plakat, umieść nowy plik w tym katalogu i zmień właściwość `poster` w konfiguracji filmu. Pozostaw grafiki lokalnie, aby były dostępne bez sieci. Po zmianie zasobów zwiększ nazwę cache w `sw.js` (np. `007-advent-v2`), co pozwoli przeglądarce pobrać nową wersję.

## Dane i zachowanie

- Pierwsze uruchomienie tworzy osobną, losową permutację filmów dla edycji roku. Algorytm Fishera–Yatesa używa `crypto.getRandomValues`; poprawne przypisanie jest zachowane przy kolejnych uruchomieniach.
- Dostępność okienek bazuje na lokalnej dacie urządzenia. Zaległe dni pozostają otwarte do odkrycia.
- Historia oraz status obejrzenia są niezależne. Reset tworzy nową kolejność, ale zachowuje wybrany plakat.
- Tryb demonstracyjny korzysta z odrębnego przypisania i nie zmienia głównej edycji. Demo jest przypięte do roku wybranej edycji.
- Kopia JSON obejmuje bieżącą edycję oraz wybrany plakat. Import zastępuje stan bieżącego roku.
- Dane nie synchronizują się między przeglądarkami ani urządzeniami. Eksportuj kopię, jeśli chcesz je przenieść.

## Publikacja i aktualizacje

Zachowaj razem `index.html`, manifest, `sw.js`, `favicon.svg`, katalog `src/` oraz `public/assets/posters/`. Service Worker zapisuje pliki aplikacji i plakaty, a przy aktualizacji usuwa poprzedni cache. Baza IndexedDB ma osobną nazwę i nie jest kasowana podczas aktualizacji cache.
