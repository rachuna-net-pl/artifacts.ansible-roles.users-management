# Konwencje projektu

## Ansible

### Moduły
- Zawsze używaj FQCN: `ansible.builtin.user`, nie `user`
- Preferowane moduły: `ansible.builtin.*`, `ansible.posix.*`

### Zmienne
- **Nigdy nie używaj zmiennych bez prefiksu** — unikaj kolizji z innymi rolami i zmiennymi hostów
- Wejściowe (interfejs roli): prefiks `in_` — np. `in_user_accounts`
  - Każda zmienna `in_*` **musi** mieć domyślną wartość w `defaults/main.yml`
- Wewnętrzne (przekazywane między taskami): prefiks `var_` — np. `var_account`, `var_home_dir`
  - Zmienne `var_*` definiowane **wyłącznie** w `vars/main.yml` — nigdy w `defaults/main.yml`
- Globalne roli (konfiguracja): prefiks `users_management_` — np. `users_management_ssh_key_type`
- Prywatne (lokalne w bloku): prefiks `_users_management_` lub pełna nazwa z kontekstem

### Taski
- Nazwy tasków w języku angielskim, w cudzysłowach: `name: "Create user account"`
- Dane wrażliwe zawsze z `no_log: true`
- Używaj `loop_control.label` aby ukryć wrażliwe dane z logów
- Warunki `when` jako lista (nie inline `and`)
- Domyślne wartości przez `| default()` lub `| default(omit)` — nigdy puste `default`
- Używaj `omit` dla opcjonalnych parametrów modułów — nie podawaj pustych stringów

### Struktura tasków
- Jeden plik = jedno logiczne zadanie (np. `create_account.yml`, `copy_ssh_keys.yml`)
- Plik `main.yml` orkiestruje — nie zawiera logiki biznesowej
- Walidacja wejścia w osobnym pliku (`validate.yml`), includowanym na początku `main.yml`
- Bloki (`block:`) do grupowania powiązanych operacji ze współdzielonymi zmiennymi

### Uprawnienia plików
- `.ssh/` — `0700`
- Klucz prywatny — `0600`
- Klucz publiczny — `0644`
- `authorized_keys` — zarządzany przez moduł `ansible.posix.authorized_key`

## YAML
- Wcięcia: 2 spacje
- Nagłówek `---` na początku każdego pliku
- Stringi z Jinja2: zawsze w cudzysłowach `"{{ zmienna }}"`
- Listy bloków preferowane nad listy inline (wyjątek: krótkie listy jak `[sudo]`)
- Tryb mode jako string w cudzysłowach: `mode: "0700"`, nie `mode: 0700`

## Git
- Format commitów: [Conventional Commits](https://www.conventionalcommits.org/)
  - `feat:` — nowa funkcja
  - `fix:` — poprawka błędu
  - `docs:` — dokumentacja
  - `chore:` — konfiguracja, CI
  - `refactor:` — refaktoryzacja
  - `test:` — testy
- Język commitów: polski (opis zmian po polsku)
- Branching: `feat/`, `fix/`, `docs/` + krótki opis

## Dokumentacja
- Język: polski
- README.md — opis roli, zmienne (tabela), przykład użycia, testowanie
- CHANGELOG.md — format Keep a Changelog, po polsku
- CONTRIBUTING.md — instrukcja dla kontrybutorów

## Testowanie
- Framework: Molecule z driverem Docker
- Platformy testowe: Ubuntu 22.04, Rocky Linux 9
- Weryfikacja: `verifier: ansible` (nie testinfra)
- Scenariusze w `molecule/default/`
- Wymagane kolekcje w `molecule/requirements.yml`
