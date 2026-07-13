---
name: wcag-a11y-audit
description: 'Kompleksowy audyt dostępności WCAG 2.1 AA. Użyj gdy chcesz przeprowadzić audyt dostępności strony lub komponentu, wygenerować raport WCAG, stworzyć przypadki testowe dostępności, lub sprawdzić zgodność UI z WCAG AA. Triggers: "audyt dostępności", "WCAG", "a11y", "accessibility", "wcag audit", "test dostępności".'
argument-hint: 'Nazwa strony lub komponentu do audytu'
---

# WCAG A11y Audit Skill

Przeprowadza kompletny, interaktywny audyt dostępności WCAG 2.1 AA: zbiera kontekst, weryfikuje kryteria sekcja po sekcji, generuje raport i przypadki testowe.

## Zasoby

- Standardy i priorytety WCAG: [wcag-accessibility.instructions.md](../../instructions/wcag-accessibility.instructions.md)
- Szablon raportu i prompt interaktywny: [wcag-audit.prompt.md](../../prompts/wcag-audit.prompt.md)
- Szablon raportu: [./assets/report-template.md](./assets/report-template.md)
- Checklist szybkiego audytu: [./references/quick-checklist.md](./references/quick-checklist.md)

## Kiedy użyć

- Audyt dostępności nowego komponentu lub strony
- Weryfikacja zgodności z WCAG AA przed release
- Generowanie raportu dla klienta lub PO
- Tworzenie zestawu TC dostępności na podstawie makiet / use case

## Procedura

### Faza 1 — Kontekst (zawsze)

1. Załaduj standardy z [wcag-accessibility.instructions.md](../../instructions/wcag-accessibility.instructions.md).
2. Zapytaj o zakres audytu:
   - Nazwa komponentu / strony
   - Dostępne artefakty: wymagania, use case (`.usecase.md`), makieta, URL
   - Środowisko: aplikacja live / kod do inspekcji
3. Wybierz tryb pracy (patrz: Punkty decyzyjne).

### Faza 2 — Weryfikacja (tryb zależny od wejścia)

Przejdź przez cztery zasady WCAG w kolejności priorytetu (Critical → High → Medium):

1. **Percepcja** — alt, kontrast, semantyka, zmiana rozmiaru
2. **Funkcjonalność** — klawiatura, fokus, skip links, tytuł
3. **Zrozumiałość** — formularze, etykiety, język
4. **Niezawodność** — ARIA, poprawny HTML, statusy

Dla każdego kryterium zbierz: ✅ Spełnione / ❌ Naruszenie / ⚠️ Do weryfikacji / N/A

Przy naruszeniu (❌) zbierz:
- Opis problemu
- Kroki reprodukcji
- Sugerowaną poprawkę

### Faza 3 — Raport końcowy

Wygeneruj raport na podstawie [./assets/report-template.md](./assets/report-template.md).

Raport musi zawierać:
- Tabelę podsumowania (✅/❌/⚠️/N/A)
- Ogólną ocenę: **PASS** / **FAIL** / **WYMAGA WERYFIKACJI**
- Sekcję naruszeń krytycznych i wysokich z rekomendacjami
- Wyniki szczegółowe per zasada WCAG
- Listę następnych kroków / defektów do zgłoszenia

### Faza 4 — TC (opcjonalnie)

Jeśli tester prosi o przypadki testowe — wygeneruj TC w formacie z [wcag-accessibility.instructions.md](../../instructions/wcag-accessibility.instructions.md):
- ID: `TC-A11Y-[NR]`
- Jedno kryterium WCAG na TC
- Jeden konkretny komponent na TC

## Punkty decyzyjne

```
Dostępna działająca aplikacja?
├── TAK → Audyt manualny + axe DevTools
│         Priorytet: Critical najpierw, potem High
└── NIE → Inspekcja kodu / makiet
          Priorytet: 1.3.1, 4.1.2, 3.3.2 (wykrywalne statycznie)

Zakres: cała strona czy pojedynczy komponent?
├── Cała strona → pełny przebieg 4 zasad
└── Komponent → tylko kryteria dotyczące komponentu
                (pomiń 2.4.1, 2.4.2, 3.1.1)
```

## Kryteria ukończenia

Audyt jest kompletny gdy:
- [ ] Wszystkie 4 zasady WCAG sprawdzone
- [ ] Każde kryterium ma status (nie może być puste)
- [ ] Każde naruszenie ❌ ma opis i rekomendację
- [ ] Raport zawiera ogólną ocenę PASS/FAIL
- [ ] Lista defektów/TC do zgłoszenia wygenerowana
