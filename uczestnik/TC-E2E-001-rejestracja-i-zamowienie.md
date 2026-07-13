# Scenariusz E2E — Rejestracja nowego użytkownika i złożenie zamówienia

| Pole        | Wartość                                                                 |
|-------------|-------------------------------------------------------------------------|
| **Wersja**  | 1.0                                                                     |
| **Data**    | 2026-07-13                                                              |
| **Autor**   | GitHub Copilot (sesja Playwright)                                       |
| **Status**  | Draft                                                                   |
| **Wymagania** | [product-requirements.md](../docs/product/product-requirements.md)  |

---

## Zakres (Scope)

**In Scope:**
- Rejestracja nowego konta użytkownika
- Aktywacja konta przez link e-mail (dev: debug skrzynka)
- Logowanie po aktywacji
- Dodanie produktów do koszyka
- Przejście ścieżki checkout: dostawa → płatność
- Potwierdzenie złożenia zamówienia

**Out of Scope:**
- Logowanie przez zewnętrznych dostawców (OAuth)
- Edycja zamówienia po złożeniu
- Obsługa zwrotów i reklamacji
- Płatności BLIK
- Walidacja danych karty po stronie bramki płatniczej

---

## Założenia i warunki wstępne

- Aplikacja ShopEasy działa pod adresem `http://localhost:3000`
- Baza danych zawiera co najmniej 2 produkty (seed wykonany)
- Podany adres e-mail **nie istnieje** wcześniej w systemie
- Użytkownik nie jest zalogowany
- Środowisko dev z aktywną debug-skrzynką e-mail (`/debug/emails`)

---

## Dane testowe

| Zmienna              | Wartość                        |
|----------------------|--------------------------------|
| imię i nazwisko      | Jan Testowy                    |
| email                | jan.testowy@example.com        |
| hasło                | Test1234!                      |
| produkt 1            | Słuchawki bezprzewodowe SoundMax X3 (299,99 zł) |
| produkt 2            | Powerbank UltraCharge 20000 (149,99 zł)          |
| imię (dostawa)       | Jan                            |
| nazwisko (dostawa)   | Testowy                        |
| ulica                | ul. Testowa 1/2                |
| kod pocztowy         | 00-001                         |
| miasto               | Warszawa                       |
| telefon              | +48 600 000 000                |
| metoda dostawy       | Paczkomat InPost (9,99 zł)     |
| imię na karcie       | Jan Testowy                    |
| numer karty          | 4111111111111111               |
| data ważności        | 12/26                          |
| CVV                  | 123                            |

---

## Scenariusze testowe

### TC-E2E-001 — Rejestracja nowego użytkownika i złożenie zamówienia (happy path)

| Pole           | Wartość                          |
|----------------|----------------------------------|
| **ID**         | TC-E2E-001                       |
| **Priorytet**  | Critical                         |
| **Typ**        | Functional                       |
| **Powiązanie** | US-01, US-02, US-05              |

**Warunki wstępne:**
- Aplikacja dostępna pod `http://localhost:3000`
- Adres `jan.testowy@example.com` nie istnieje w systemie
- Użytkownik niezalogowany

**Dane testowe:** patrz sekcja „Dane testowe" powyżej

**Kroki testowe:**

| #  | Akcja                                                                                         | Oczekiwany rezultat                                                                                       |
|----|-----------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| 1  | Przejdź do `http://localhost:3000/`                                                           | Strona główna ShopEasy widoczna; lista polecanych produktów załadowana                                    |
| 2  | Kliknij „Dodaj do koszyka" przy produkcie **Słuchawki bezprzewodowe SoundMax X3**             | Produkt dodany; przycisk reaguje wizualnie                                                                |
| 3  | Kliknij „Dodaj do koszyka" przy produkcie **Powerbank UltraCharge 20000**                     | Produkt dodany; przycisk reaguje wizualnie                                                                |
| 4  | Kliknij link „🛒 Koszyk" w nawigacji                                                          | Widok `/cart`; nagłówek „Koszyk (2 szt.)"; oba produkty widoczne; suma = 449,98 zł                       |
| 5  | Kliknij „Przejdź do kasy"                                                                     | Przekierowanie na `/login?next=%2Fcheckout%2Fdelivery`; formularz logowania widoczny                      |
| 6  | Kliknij link „Zarejestruj się"                                                                | Przekierowanie na `/register`; formularz rejestracji widoczny                                             |
| 7  | Wypełnij pola: Imię i nazwisko = `Jan Testowy`, E-mail = `jan.testowy@example.com`, Hasło = `Test1234!`, Powtórz hasło = `Test1234!` | Wszystkie pola wypełnione poprawnie                                                         |
| 8  | Kliknij przycisk „Zarejestruj się"                                                            | Ekran potwierdzenia: „Sprawdź skrzynkę e-mail!"; widoczny link do `/debug/emails`                        |
| 9  | Przejdź do `/debug/emails`                                                                    | Widoczna 1 wiadomość do `jan.testowy@example.com` z tytułem „Potwierdź rejestrację w ShopEasy"            |
| 10 | Kliknij link aktywacyjny w wiadomości (`/api/auth/verify?token=…`)                            | Przekierowanie na `/login?verified=1`; komunikat „✅ Konto zostało aktywowane! Możesz się zalogować."     |
| 11 | Wypełnij pola: E-mail = `jan.testowy@example.com`, Hasło = `Test1234!`                        | Pola wypełnione                                                                                           |
| 12 | Kliknij przycisk „Zaloguj się"                                                                | Przekierowanie na `/`; w nawigacji widoczne „👤 Jan Testowy" i przycisk „Wyloguj"                        |
| 13 | Kliknij „Dodaj do koszyka" przy **Słuchawkach SoundMax X3** i **Powerbanku UltraCharge 20000** | Oba produkty dodane do koszyka                                                                           |
| 14 | Przejdź do `/checkout/delivery`                                                               | Strona „Dostawa i adres" widoczna; podsumowanie koszyka: 2 produkty, 449,98 zł                           |
| 15 | Wypełnij adres dostawy: Imię=`Jan`, Nazwisko=`Testowy`, Ulica=`ul. Testowa 1/2`, Kod=`00-001`, Miasto=`Warszawa`, Telefon=`+48 600 000 000` | Wszystkie pola adresu wypełnione |
| 16 | Kliknij „Paczkomat InPost 2–3 dni robocze 9,99 zł"                                            | Opcja zaznaczona; suma dostawy w podsumowaniu = 9,99 zł; łącznie = 459,97 zł                             |
| 17 | Kliknij „Przejdź do płatności"                                                                | Przekierowanie na `/checkout/payment`; formularz karty płatniczej widoczny; timer odliczania aktywny      |
| 18 | Wybrana jest zakładka „Karta płatnicza" (domyślna)                                            | Formularz karty widoczny (Imię na karcie, Numer karty, Data ważności, CVV)                               |
| 19 | Wypełnij dane karty: Imię=`Jan Testowy`, Numer=`4111111111111111`, Data=`12/26`, CVV=`123`    | Wszystkie pola karty wypełnione                                                                           |
| 20 | Kliknij przycisk „Zapłać"                                                                     | Przekierowanie na `/checkout/success?orderId=…`; nagłówek „✅ Zamówienie złożone!"                       |
| 21 | Sprawdź szczegóły zamówienia na stronie sukcesu                                               | Widoczne: numer zamówienia `#XXXXXXXX`, produkty z cenami, dostawa 9,99 zł, łącznie 459,97 zł            |

**Notatki:**
- Po zalogowaniu koszyk gościa nie jest automatycznie przenoszony — produkty należy dodać ponownie (znane ograniczenie aplikacji demo).
- Link aktywacyjny jest dostępny w debug-skrzynce `/debug/emails` wyłącznie w środowisku dev.
- Numer karty `4111111111111111` to standardowa karta testowa Visa (nie przechodzi przez realną bramkę).

---

## Macierz pokrycia

| TC ID       | Wymaganie     | Priorytet | Typ        | Status  |
|-------------|---------------|-----------|------------|---------|
| TC-E2E-001  | US-01, US-02, US-05 | Critical | Functional | Draft |

---

## Kryteria wejścia / wyjścia (Entry / Exit Criteria)

**Entry Criteria (kiedy można uruchomić):**
- Aplikacja uruchomiona i dostępna pod `http://localhost:3000`
- Seed bazy danych wykonany (`npm run seed`)
- Adres testowy nie istnieje w bazie (lub baza zresetowana)
- Środowisko dev z debug-skrzynką aktywną

**Exit Criteria (kiedy testy są „done"):**
- TC-E2E-001 zakończony statusem PASS
- Strona `/checkout/success` wyświetla numer zamówienia
- Brak błędów w konsoli przeglądarki podczas całego flow
