# Contributions

Dziękujemy za zainteresowanie i chęć wniesienia wkładu do tego projektu! Doceniamy Twój czas i wysiłek. Oto kilka wskazówek, które pomogą Ci zacząć.

## Jak wnieść wkład

1. **Zforkuj repozytorium**: Stwórz kopię tego repozytorium na swoim koncie GitLab, klikając przycisk „Fork" na górze.
2. **Sklonuj swojego forka**: Sklonuj zforkowane repozytorium na swój lokalny komputer za pomocą komendy:
   ```bash
   git clone https://gitlab.com/twoja-nazwa-użytkownika/nazwa-projektu.git
   ```
3. **Utwórz gałąź**: Stwórz nową gałąź dla swoich zmian:
   ```bash
   git checkout -b twoja-nazwa-gałęzi
   ```
4. **Wprowadź zmiany**: Zaimplementuj swoje zmiany w tej gałęzi.
5. **Przetestuj swoje zmiany**: Upewnij się, że Twoje zmiany działają poprawnie, uruchamiając odpowiednie testy lub skrypty projektu.
6. **Zacommituj swoje zmiany**: Napisz jasny komunikat commit zgodny ze standardem [Conventional Commits](https://www.conventionalcommits.org/):
   ```bash
   git add .
   git commit -m "feat: opis zmian"
   ```
7. **Wyślij na GitLab**: Wyślij swoje zmiany do swojego forka:
   ```bash
   git push origin twoja-nazwa-gałęzi
   ```
8. **Utwórz Merge Request**: Otwórz MR z Twojego forka do głównego repozytorium, wyjaśniając co zrobiłeś i dlaczego jest to przydatne.

## Zgłaszanie problemów

Jeśli napotkasz błędy, masz sugestie lub coś jest niezrozumiałe, otwórz zgłoszenie w sekcji Issues repozytorium. Podczas zgłaszania problemu podaj:

- Jasny opis problemu
- Kroki do odtworzenia problemu
- Odpowiednie logi, komunikaty błędów lub zrzuty ekranu

## Wytyczne dotyczące commitów

Przestrzegaj specyfikacji [Conventional Commits](https://www.conventionalcommits.org/):

- `feat` — nowa funkcja
- `fix` — poprawka błędu
- `docs` — zmiany w dokumentacji
- `chore` — zadania techniczne, konfiguracja
- `refactor` — refaktoryzacja bez zmiany zachowania
- `ci` — zmiany w pipeline CI/CD

Szczegóły: [Standardy/Conventional Commits](https://gitlab.com/pl.rachuna-net/ai/openclaw/gitlab-profile/-/wikis/Standardy/Conventional-Commits)