---
description: "QA perspective: analyze functional requirements for testability gaps, contradictions, undefined concepts, and missing edge cases. Returns structured assessment by requirement ID with acceptance criteria issues and readiness status."
name: "Functional Requirements QA Assessment"
argument-hint: "Assess PRD section or specific requirement IDs"
agent: "ask"
---

# Functional Requirements QA Assessment
**Role:** Quality Assurance Engineer

---

## Krok 1: Identyfikacja PRD

Wskaż plik specyfikacji wymagań:
- Domyślnie: `docs/product/product-requirements.md`
- Lub podaj inną ścieżkę jeśli plik znajduje się gdzie indziej

## Krok 2: Zakres oceny

Wybierz co chcesz analizować:

1. **Konkretne sekcje** (np. "5.2 Katalog produktów", "5.4 Checkout")
2. **Konkretne numery wymagań** (np. "REG-03, REG-07, CHK-08")
3. **Całą sekcję numerowaną** (np. "5" dla wszystkich wymagań funkcjonalnych)

---

## Ocena — Kryteria analityczne

### 📋 Trudne do weryfikacji (Hard to Test)
- ❌ Brak jasnych kryteriów akceptacji
- ❌ Niejasne warunki sukcesu ("powinno działać", "rozsądnie", "jeśli dotyczy")
- ❌ Brak warunków brzegowych i limitów
- ❌ Nie określone formaty, oczekiwane wartości

### ⚔️ Sprzeczności (Contradictions)
- ❌ Wymagania kolidujące w obrębie sekcji
- ❌ Nierówna szczegółowość (jedno super szczegółowe, inne generyczne)
- ❌ Niezgodne ograniczenia

### 🎯 Brakujące edge-case'y (Missing Scenarios)
- ❌ Brak scenariusza: dane brakujące, błędne, ekstremalne
- ❌ Brak obsługi: zewnętrzny serwis nie działa, timeout, błąd sieci
- ❌ Co gdy: użytkownik nie ma historii, brak opcji, pusta lista
- ❌ Operacje równoczesne, retry'e, resetowanie liczników

### 📚 Niezdefiniowane pojęcia (Undefined Concepts)
- ❌ Terminy bez definicji ("dostępny", "bezczynność", "ostatnio", "potwierdzone")
- ❌ Formaty nieokreślone (email, data, telefon, kod pocztowy)
- ❌ Limity nie skwantyfikowane ("rozsądnie", "szybko", "wiele")
- ❌ Czas nie określony ("krótko", "bezpośrednio", "natychmiast")

---

## Format wynikowy

Dla każdego analizowanego wymagania:

```
**[ID WYMAGANIA]** 
[Treść wymagania]
- Brak ...
- Sprzeczne z ...
- Brak scenariusza ...
- Brak definicji pojęcia: ...
```

### Rekomendacja (opcjonalnie)
Jeśli istnieje jasny sposób naprawy:
```
- **Rekomendacja:** Wyjaśnić [co] i dodać kryteria: (1) ..., (2) ...
```

---

## Podsumowanie — Gotowość do testowania

Po analizie wszystkich wymagań przygotuj tabelę statusu:

```
| Kategoria | Status | Ilość problemów |
|-----------|--------|-----------------|
| **[Nazwa sekcji]** | 🟢/🟡/🔴 | N |
```

### Legenda statusów
- **🟢 Gotowe** — Można pisać test case'y, kryteria jasne, brak blokerów
- **🟡 Częściowo gotowe** — Wymaga wyjaśnień dla niektórych scenariuszy, głównie testowalne
- **🔴 Nie gotowe** — Brakuje kluczowych definicji, wymaga refactoru przed testami

### Wniosek
Podsumowanie gotowości sekcji(i), priorytetowe obszary wymagające doprecyzowania, krytyczne luki (oznacz **KRYTYCZNIE**).

---

## Procedura

1. **Wczytaj PRD** z wskazanej ścieżki
2. **Ekstrakcja wymagań** — znajdź wymagania z zakresu oceny
3. **Analiza każdego wymagania** — apply wszystkie kryteria (Testability, Contradictions, Edge Cases, Undefined)
4. **Lista problemów** — wylicz dla każdego wymagania
5. **Status tabelka** — posumuj po kategoriach/sekcjach
6. **Wniosek** — rekomendacja działań
