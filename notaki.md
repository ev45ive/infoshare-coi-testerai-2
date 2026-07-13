# infoshare-coi-testerai-2

# Instalacje

- git
- copilot



# Model mentalny 

## Model (LLM) 
 - wytrenowany na ogromnych zbiorach **w konkretnej dacie** (cutoff)
 - Plik z wagami (parametry - 80B) + warstwami - wiedza parametryczna - nie zmiania się potem.
 - Text wchodzi, text wychodzi - **model się nie zmienia.**
 - Każde zapytanie do modelu jest "na czysto" - jak rozmowa z zupełnie inną osobą.
 - Kontekst rozmowty musi być każdorazowo **dostarczony od nowa**
- Kontekst zmienia znaczenie zdań

## Chat (Harness)
 - Sesje chatu - tury = Prompt + historia bieżącej sesji 
 - + Kontekst dołączony recznie (pliki... -> text)

## Asystent
 - Stały zestaw instruckji (system instructions, files)

## Agent (Tool Calling)
 - Lista narzędzi i instuckje ich użycja -> model
 - Prompt wskazuje na potrzegbe narzędzia
 - model wysyłą specjalny XML/JSON do narzędzia (VSCODE)
 - wynik zadania jest promptem dla modelu (jako XML/JSON)

# Chain of thought (Thinking model)
 - Najpierw rozpisz jak rozumiesz zadanie, co wiesz czego brakuje
 - Zaplanuj odpowiedź
 - Odpowiedz

# Harness *dokłada* do każdego wywołania:
 - historię sesji (poprzednie wiadomości)
 - system prompt / instrukcje
 - narzędzia (web search, code runner, MCP)
 - zarządzanie kontekstem