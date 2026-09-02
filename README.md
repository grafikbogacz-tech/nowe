# Generator rachunków

Rachunek do umowy o dzieło / zlecenia — jeden plik HTML, bez instalacji.
Liczy: kwota brutto → koszty uzyskania przychodu → zaliczka na podatek → do wypłaty,
i podaje kwotę słownie. Dokument drukuje się na jedną stronę A4.

## Jak używać

1. Pobierz plik `index.html`.
2. Kliknij go dwa razy — otworzy się w przeglądarce.
3. Wypełnij formularz po lewej. Dokument po prawej aktualizuje się na bieżąco.
4. Kliknij **Wydrukuj / Zapisz PDF** i w okienku druku wybierz „Zapisz jako PDF".

Nie trzeba niczego instalować: żadnego Node, PHP ani serwera. Działa też offline.

## Opcjonalnie: na domenie

Ten sam plik możesz wrzucić przez FTP na serwer (najlepiej do katalogu zabezpieczonego
hasłem, np. `.htpasswd`) i wtedy masz generator dostępny również z telefonu.
Plik nie wysyła danych nigdzie — całość liczy się w przeglądarce.

## Przyciski

- **Wydrukuj / Zapisz PDF** — otwiera okno druku przeglądarki.
- **Nowy rachunek** — zwiększa numer o 1, czyści przedmiot umowy i kwotę,
  zostawia dane zleceniodawcy i wykonawcy.
- **Wyczyść wszystko** — czyści cały formularz.

Dane wpisane w formularz są zapamiętywane w przeglądarce (`localStorage`),
więc po ponownym otwarciu pliku nie trzeba wpisywać ich od nowa.
Jeśli przeglądarka blokuje zapis dla plików lokalnych, pojawi się informacja w panelu.

## Stawki podatkowe

Stawka zaliczki PIT i koszty uzyskania przychodu to **pola edytowalne**, nie są
zabetonowane w kodzie. Przed pierwszym użyciem sprawdź, czy domyślne wartości
odpowiadają aktualnym przepisom i Twojej sytuacji.

Zaokrąglenia zgodne z praktyką: podstawa opodatkowania i zaliczka na podatek
zaokrąglane do pełnych złotych. Wszystkie kwoty liczone na liczbach całkowitych
(w groszach), żeby uniknąć błędów zaokrągleń zmiennoprzecinkowych.

Generator nie obsługuje składek ZUS — jest przeznaczony dla umowy o dzieło
oraz umowy zlecenia bez obowiązku składkowego.

## Czego to nie zastępuje

Narzędzie do wystawiania dokumentu, nie księgowość. Nie prowadzi rejestru
rachunków, nie pilnuje limitu ulgi dla osób do 26 lat i nie obsługuje KSeF
(który dotyczy faktur B2B, nie rachunków do umów cywilnoprawnych).
