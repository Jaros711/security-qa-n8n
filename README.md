# 🐛 Bug Report Generator — n8n + Groq + GitHub

Automatyczny pipeline do generowania bug reportów z notatek testera.
Notatki w języku naturalnym → ustrukturyzowany issue na GitHubie w ~10 sekund.

## Demo

**Input (notatka testera):**
> "SQL injection możliwy w polu wyszukiwarki. Dane użytkowników są eksponowane."

**Output (GitHub Issue):**
→ Tytuł, severity, środowisko, kroki reprodukcji, expected/actual, rekomendacja

## Stack

- **n8n** — orkiestracja workflow
- **Groq API** (llama-3.3-70b) — klasyfikacja i strukturyzacja danych
- **GitHub API** — automatyczne tworzenie issues z labelami

## Architektura

Webhook → walidacja → LLM → sanityzacja → GitHub Issue

1. Webhook przyjmuje POST z notatkami testera
2. Walidacja czy pole `notes` nie jest puste (zwraca 400 jeśli tak)
3. Groq generuje structured JSON z polami bug reportu
4. Węzeł Code sanityzuje output LLM
5. GitHub API tworzy issue z odpowiednim labelem severity

## Uruchomienie
```bash
curl -X POST http://localhost:5678/webhook/bug-report \
  -H "Content-Type: application/json" \
  -d '{"notes": "Twój opis błędu"}'
```

## Przykładowe issues

- [#19 Nieprawidłowy odcień przycisku Anuluj](link)
- [#16 Podatność na SQL injection w wyszukiwarce](link)
