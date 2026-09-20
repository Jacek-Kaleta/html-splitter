# HTML Splitter

Prosta, jednoplikowa aplikacja webowa do rozdzielania kodu HTML na osobne pliki biblioteczne `.css` i `.js`. Cały kod (HTML, CSS, JavaScript) mieści się w jednym pliku `.html` — bez zależności do instalacji, bez builda, bez backendu.

## Opis

Aplikacja wczytuje plik `.html`/`.htm`, wyszukuje w nim oznaczone bloki:

```html
<style name="nazwa">...</style>
<script name="nazwa">...</script>
```

wycina ich zawartość do osobnych plików `nazwa.css` i `nazwa.js`, a w miejscu wyciętych bloków wstawia odpowiednie odwołania:

```html
<link rel="stylesheet" href="nazwa.css">
<script src="nazwa.js"></script>
```

Na koniec pakuje zmodyfikowany plik HTML wraz z wyodrębnionymi plikami `.css`/`.js` do archiwum ZIP i automatycznie uruchamia pobieranie.

## Funkcjonalności

- 📂 Wczytywanie pliku `.html` / `.htm` z dysku.
- ✂️ Wykrywanie i wycinanie wielu bloków `<style name="...">` oraz `<script name="...">`.
- 🔗 Automatyczne podmienianie wyciętych bloków na `<link>` / `<script src="...">`.
- 📦 Pakowanie wyniku do archiwum `.zip` (biblioteka [JSZip](https://stuk.github.io/jszip/)).
- 🏷️ Inteligentne nazewnictwo archiwum na podstawie `<title>` strony (z usunięciem polskich znaków diakrytycznych) i znacznika czasu, np. `MojaAplikacja-20260920-163000.zip`.
- 🌗 Przełącznik trybu jasny / ciemny z zapamiętaniem preferencji.
- 🚫 Zero zależności produkcyjnych poza JSZip ładowanym z CDN — brak instalacji, brak buildu.

## Jak używać

1. Otwórz plik `html-splitter.html` w przeglądarce (dwuklik lub wrzucenie na dowolny hosting statyczny).
2. Kliknij **„Wybierz plik”** i wskaż swój plik `.html`/`.htm`.
3. Kliknij **„Przetwórz i pobierz ZIP”**.
4. Rozpakuj pobrane archiwum — otrzymasz zmodyfikowany plik HTML oraz wyodrębnione pliki `.css`/`.js` gotowe do umieszczenia w bibliotece/repozytorium.

### Przykład

Plik wejściowy:

```html
<style name="theme">
  body { background: #fff; }
</style>

<script name="app">
  console.log("start");
</script>
```

Po przetworzeniu:

```html
<link rel="stylesheet" href="theme.css">

<script src="app.js"></script>
```

oraz dodatkowe pliki `theme.css` i `app.js` z wyodrębnioną zawartością.

## Szczegóły techniczne

- **Bez `DOMParser`** — cały parsing i modyfikacja kodu wykonywane są wyłącznie na surowym tekście za pomocą wyrażeń regularnych i operacji na łańcuchach znaków.
- Znaki `<` i `>` w logice JavaScript są generowane wyłącznie przez `String.fromCharCode(60)` / `String.fromCharCode(62)`, aby uniknąć uszkodzenia struktury dokumentu przez parser HTML przeglądarki (szczególnie ryzykownej sekwencji `</script>` wewnątrz kodu skryptu).
- Nazwy plików wynikowych są sanityzowane: usuwane są polskie znaki diakrytyczne (`ą→a`, `ę→e`, `ł→l` itd.), a znaki specjalne i spacje zamieniane są na myślniki.
- Motyw jasny/ciemny wykorzystuje `prefers-color-scheme` oraz `localStorage` do zapamiętania wyboru użytkownika.

## Wymagania

- Nowoczesna przeglądarka z obsługą JavaScript (Chrome, Firefox, Edge, Safari).
- Połączenie z internetem przy pierwszym uruchomieniu (do pobrania biblioteki JSZip z CDN `cdnjs.cloudflare.com`).

## Struktura projektu

```
.
└── html-splitter.html   # cała aplikacja w jednym pliku
```

## Ograniczenia

- Rozpoznawane są wyłącznie bloki z atrybutem `name` — zwykłe `<style>`/`<script>` bez tego atrybutu pozostają bez zmian.
- Aplikacja operuje na tekście, nie na drzewie DOM — nietypowo sformatowany lub uszkodzony HTML może nie zostać poprawnie rozpoznany.

## Licencja

[MIT](https://choosealicense.com/licenses/mit/)
