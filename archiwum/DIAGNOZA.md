# Dlaczego stary generator nie startował

Diagnoza oryginalnej aplikacji z folderu `D:\NARZEDZIA PRACY\generator rachunków`
(pliki odtworzone z kopii na Dysku Google, 02.09.2026).

## Co tam było

Aplikacja w Pythonie, trzy generacje narosłe na sobie:

| Plik | Rola |
|---|---|
| `rachunki_logic.py` | wspólna logika: odczyt Excela, kwota słownie, wypełnianie szablonu Word |
| `webapp.py` | serwer Flask na `127.0.0.1:5057`, interfejs w przeglądarce |
| `generate_rachunek.py` | wersja konsolowa (zapasowa) |
| `Uruchom_Generator.bat` | launcher: instaluje biblioteki i startuje serwer |
| `Baza_i_Generator_Rachunkow_ClickUp.xlsx` | baza: Ustawienia, Klienci, Usługi, Baza Rachunków |
| `Szablon_Rachunku_Dzialalnosc_Nierejestrowana.docx` | szablon Word z polami `[Nazwa_Pola]` |
| `Wystawione/` | katalog wyjściowy |

## Cztery przyczyny, każda wystarczająca żeby położyć aplikację

**1. Brak katalogu `templates/` z plikiem `index.html`.**
`webapp.py` wywołuje `render_template("index.html")`. Flask szuka tego pliku
w podkatalogu `templates/` obok skryptu. Tego katalogu nie ma nigdzie w projekcie.
Serwer startuje, ale każde wejście na stronę kończy się błędem 500 — pusty ekran.

**2. Launcher ukrywa wszystkie błędy.**
`Uruchom_Generator.bat` startuje serwer przez `pythonw.exe`, czyli wariant Pythona
bez okna konsoli. Każdy komunikat o błędzie — łącznie z tym z punktu 1 — leci
w próżnię. Z Twojej perspektywy: klikasz, otwiera się przeglądarka, nic nie działa
i nie ma żadnej informacji dlaczego. Dokładnie to zgłosiłaś.

**3. Ścieżki wskazują na katalog nadrzędny.**
```python
TOOL_DIR     = katalog ze skryptami
DESKTOP_DIR  = TOOL_DIR.parent
XLSX_PATH    = DESKTOP_DIR / "Baza_i_Generator_Rachunkow_ClickUp.xlsx"
TEMPLATE_PATH= DESKTOP_DIR / "Szablon_Rachunku_Dzialalnosc_Nierejestrowana.docx"
```
Kod oczekuje Excela i szablonu Word **piętro wyżej** niż skrypty. W kopii, którą
widzę, wszystko leży w jednym katalogu. Dodatkowo pliki pobrane z Dysku mają
w nazwach dopiski `(1)` i `(2)` — przy takich nazwach dopasowanie i tak by nie zadziałało.

**4. Martwe odwołania po poprzednich wersjach.**
`generate_rachunek.py` w komentarzu odsyła do `Generator_Rachunkow.pyw` i
`Uruchom_Generator.vbs`. Żaden z tych plików nie istnieje. Projekt przeszedł co
najmniej trzy przebudowy i został po nich gruz.

## Piąta rzecz — nie awaria, ale realne ograniczenie

Logika obsługuje **jedną pozycję na rachunku**: jedna usługa, jedna ilość,
jedna wartość, a w szablonie Word wypełniany jest na sztywno wiersz
`items_table.rows[1]`.

Tymczasem `Rachunek_nr_01-2026` ma dwie pozycje (obsługa strony www 4 × 70 zł
oraz projekt ulotki 1 × 180 zł, razem 460 zł). Ten rachunek musiał powstać
z ręcznej edycji Worda — generator nie umiał go wystawić.

W tym samym pliku widać jeszcze jeden defekt: `Słownie: Czterysta sześćdziesiąt 00/100`
— bez słowa „złotych". Kwota słownie bez nazwy waluty to błąd formalny dokumentu.

## Decyzja

Stos: Python 3.14 + Flask + openpyxl + python-docx + launcher .bat + pythonw
+ Excel, który musi być zamknięty w trakcie generowania + ręczna konwersja
Word → PDF.

To sześć niezależnych punktów awarii w narzędziu, którego jedna osoba używa
kilka razy w miesiącu. Naprawienie czterech usterek przywróciłoby działanie,
ale nie ruszyłoby przyczyny źródłowej: architektura jest nieproporcjonalna
do zadania.

Zastąpione jednym plikiem HTML (`../index.html`) — bez Pythona, bez instalacji,
bez serwera, z obsługą wielu pozycji i drukiem prosto do PDF.

## Oryginały

Pliki źródłowe zostają nietknięte na Dysku Google w folderze `generator rachunków`
oraz na dysku lokalnym. Celowo nie odtwarzam tu kodu Pythona: przy odczycie
z Dysku zgubiły się wcięcia, a wcięcia w Pythonie są składnią — kopia byłaby
niedziałająca i myląca.
