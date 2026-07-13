---
description: "Use when writing, reviewing, or generating test cases, test scenarios, test plans, or QA documentation. Covers required fields, document structure, formatting, and anti-patterns."
---

# Test Case Standard

## Wymagane pola każdego TC

Każdy przypadek testowy **musi** zawierać:

- **ID** — unikatowy identyfikator w formacie `TC-[MODUŁ]-[NR]` (np. `TC-LOGIN-001`)
- **Tytuł** — zwięzły opis co testujemy (max 80 znaków)
- **Priorytet** — `Critical` | `High` | `Medium` | `Low`
- **Typ** — `Functional` | `Negative` | `Edge Case` | `Smoke` | `Regression`
- **Powiązanie** — ID wymagania lub User Story (np. `US-12`)
- **Warunki wstępne (Preconditions)** — stan systemu i dane przed uruchomieniem
- **Kroki testowe** — numerowana lista konkretnych akcji
- **Oczekiwany rezultat** — mierzalny wynik dla każdego kroku
- **Dane testowe** *(opcjonalne)* — konkretne wartości inline lub link do pliku z danymi (patrz reguła poniżej)

## Reguła: Dane testowe

Sekcja **Dane testowe** jest opcjonalna — pomijaj ją gdy TC nie wymaga konkretnych wartości wejściowych (np. testy nawigacji, weryfikacja widoku).

Gdy dane są potrzebne, użyj jednego z dwóch formatów:

**Inline** — gdy danych jest mało i są proste:
```markdown
**Dane testowe:**
| Zmienna | Wartość                  |
|---------|--------------------------|
| email   | jan.kowalski@example.com |
| hasło   | SecurePass123!           |
```

**Link do pliku** — gdy dane są obszerne, wielokrotnie używane lub wrażliwe (np. zestawy CSV, fixtures):
```markdown
**Dane testowe:** [test-data/login-scenarios.csv](../test-data/login-scenarios.csv)
```

Pliki z danymi umieszczaj w katalogu `test-data/` obok dokumentu z TC.

## Format: Gherkin (preferowany dla BDD)

```gherkin
Feature: [Nazwa funkcjonalności]

Scenario: [Co dokładnie testujemy]
  Given [Stan początkowy]
  When  [Akcja użytkownika]
  Then  [Oczekiwany rezultat]
  And   [Dodatkowe warunki wyniku]
```

## Struktura dokumentu scenariuszy

Dokument musi zawierać **wszystkie** poniższe sekcje:

1. **Nagłówek** — tytuł, wersja, data, autor, status (Draft/Review/Approved), link do wymagań
2. **Zakres (Scope)** — co jest testowane i jawny punkt „Out of Scope"
3. **Założenia i warunki wstępne** — środowisko, konta, stan danych
4. **Dane testowe** — zestawy dla happy path, edge cases i danych negatywnych
5. **Scenariusze testowe** — zgrupowane per moduł/epik/user story
6. **Macierz pokrycia** — mapowanie TC → wymaganie, priorytet, typ
7. **Kryteria wejścia/wyjścia (Entry/Exit Criteria)** — kiedy można zacząć i kiedy testy są „done"

## Szablon TC (z placeholderami)

```markdown
### TC-[MODUŁ]-[NR] — [Tytuł: co testujemy, max 80 znaków]

| Pole              | Wartość                                      |
|-------------------|----------------------------------------------|
| **ID**            | TC-[MODUŁ]-[NR]                              |
| **Priorytet**     | [Critical / High / Medium / Low]             |
| **Typ**           | [Functional / Negative / Edge Case / Smoke / Regression] |
| **Powiązanie**    | [US-XX / REQ-XX]                             |

**Warunki wstępne:**
- [Stan środowiska / konto / rola użytkownika]
- [Stan danych w systemie przed testem]

**Dane testowe:**
| Zmienna       | Wartość                  |
|---------------|--------------------------|
| [zmienna_1]   | [konkretna wartość]      |
| [zmienna_2]   | [konkretna wartość]      |

**Kroki testowe:**

| # | Akcja                              | Oczekiwany rezultat                          |
|---|------------------------------------|----------------------------------------------|
| 1 | [Konkretna akcja użytkownika]      | [Mierzalny, konkretny wynik]                 |
| 2 | [Konkretna akcja użytkownika]      | [Mierzalny, konkretny wynik]                 |
| 3 | [Konkretna akcja użytkownika]      | [Mierzalny, konkretny wynik]                 |

**Notatki:** [Opcjonalne uwagi, zależności, znane ograniczenia]
```

---

## Przykład wypełnionego TC

```markdown
### TC-LOGIN-002 — Logowanie z niepoprawnym hasłem wyświetla komunikat błędu

| Pole              | Wartość             |
|-------------------|---------------------|
| **ID**            | TC-LOGIN-002        |
| **Priorytet**     | High                |
| **Typ**           | Negative            |
| **Powiązanie**    | US-04               |

**Warunki wstępne:**
- Użytkownik jest na stronie `/login`
- Konto `jan.kowalski@example.com` istnieje w systemie i jest aktywne
- Użytkownik nie jest zalogowany

**Dane testowe:**
| Zmienna   | Wartość                    |
|-----------|----------------------------|
| email     | jan.kowalski@example.com   |
| hasło     | BledneHaslo999!            |

**Kroki testowe:**

| # | Akcja                                              | Oczekiwany rezultat                                          |
|---|----------------------------------------------------|--------------------------------------------------------------|
| 1 | Wpisz `jan.kowalski@example.com` w pole Email      | Pole Email zawiera wpisany adres                             |
| 2 | Wpisz `BledneHaslo999!` w pole Hasło               | Pole Hasło wyświetla maskowane znaki                         |
| 3 | Kliknij przycisk „Zaloguj"                         | Formularz nie zostaje wysłany; strona nie zmienia adresu URL |
| 4 | Sprawdź komunikat pod formularzem                  | Widoczny tekst: „Nieprawidłowy email lub hasło"              |
| 5 | Sprawdź czy użytkownik jest zalogowany             | Brak tokenu sesji; użytkownik pozostaje na `/login`          |

**Notatki:** Komunikat błędu celowo nie wskazuje, czy błędne jest hasło czy email (security requirement SEC-03).
```

---

## Dobre praktyki

- Jeden TC = jedna rzecz do weryfikacji (atomowość)
- TC muszą być niezależne — uruchamialne w dowolnej kolejności
- Każdy TC powiązany z konkretnym wymaganiem (traceability)
- Używaj konkretnych danych testowych, nigdy ogólnych opisów
- Oddzielaj happy path od edge cases i scenariuszy negatywnych
- Czytelne ID: `TC-LOGIN-001` zamiast `TC-001`
- Dokument wersjonowany z historią zmian

## Unikaj

- Brak sekcji **Out of Scope** w dokumencie
- TC bez powiązania z wymaganiem lub user story
- Niejasne oczekiwane wyniki: „sprawdź czy działa" — zawsze podaj konkretny, mierzalny wynik
- Gigantyczne TC z >7 krokami — rozbij na mniejsze
- Brak konkretnych danych testowych — nigdy „zaloguj się jako admin" bez podania danych
- Mieszanie happy path i sad path w jednym TC
- Brak sekcji **Exit Criteria** — brak definicji „done"
- Kopiuj-wklej TC bez rewizji pod aktualny kontekst
