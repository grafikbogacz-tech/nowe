# Generator rachunków — działalność nierejestrowana

Jeden plik HTML. Bez Pythona, bez instalacji, bez serwera.
Zastępuje aplikację Flask + Excel + szablon Word z folderu
`D:\NARZEDZIA PRACY\generator rachunków`, która przestała się uruchamiać —
przyczyny opisane w [`archiwum/DIAGNOZA.md`](archiwum/DIAGNOZA.md).

## Jak używać

1. Pobierz `index.html`.
2. Kliknij dwa razy — otworzy się w przeglądarce.
3. Zakładka **Moje dane** → uzupełnij dane sprzedawcy i numer konta (raz).
4. Zakładka **Rachunek** → wypełnij i kliknij **Zapisz PDF**.

W oknie druku wybierz „Zapisz jako PDF". Dokument mieści się na stronie A4.

Działa offline. Nic nie wysyła na zewnątrz — wszystko liczy się w przeglądarce.
Ten sam plik możesz wrzucić na domenę (najlepiej pod hasłem) i mieć go z telefonu.

## Zakładki

**Rachunek** — numer, daty, nabywca i pozycje. Numer podpowiada się sam
(kolejny wolny w danym roku, format `NN/RRRR`). Termin płatności wylicza się
z daty wystawienia i domyślnej liczby dni.

**Klienci** — baza nabywców. Wybrany klient wypełnia dane i ustawia walutę:
PL → PLN, UK → GBP, pozostałe kraje → EUR.

**Usługi** — cennik. Pozycję wstawiasz jednym kliknięciem razem z ceną i jednostką.

**Rejestr** — wystawione rachunki ze statusem „Oczekuje na wpłatę" / „Opłacony".
Eksport do CSV otwiera się w Excelu.

**Moje dane** — dane sprzedawcy, konto, domyślny termin płatności
oraz kopia zapasowa wszystkich danych do pliku `.json`.

## Co się zmieniło względem starej wersji

- **Wiele pozycji na rachunku.** Stara wersja obsługiwała jedną — rachunek
  `01/2026` z dwiema pozycjami musiał powstać z ręcznej edycji Worda.
- **Kwota słownie z nazwą waluty.** W starym pliku PDF było
  `Czterysta sześćdziesiąt 00/100`, bez słowa „złotych". Teraz jest poprawnie,
  z odmianą dla PLN, GBP i EUR.
- **PDF od razu**, bez pośredniego kroku Word → PDF.
- **Excel nie musi być zamknięty** — bo nie ma Excela w obiegu.

## Dane i kopia zapasowa

Wszystko siedzi w `localStorage` tej przeglądarki, na tym komputerze.
Nie jest to backup: wyczyszczenie danych witryny kasuje bazę.

Przed zmianą komputera albo czyszczeniem historii zrób
**Moje dane → Zapisz kopię (.json)**. Tym samym miejscem wczytasz ją z powrotem.

## Czego to nie robi

Nie prowadzi księgowości i nie pilnuje limitu przychodu dla działalności
nierejestrowanej. Nie obsługuje KSeF — ten dotyczy faktur, nie rachunków
wystawianych w działalności nierejestrowanej.

## Plik dodatkowy

`rachunek-umowa-o-dzielo.html` — osobny generator rachunku do umowy o dzieło
lub zlecenia (brutto → koszty uzyskania → zaliczka PIT → do wypłaty).
Inny typ dokumentu, przyda się gdybyś sama komuś płaciła na umowę cywilnoprawną.
