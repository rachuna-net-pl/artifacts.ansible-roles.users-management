# CLAUDE.md

## Opis projektu

Rola Ansible (`users-management`) do zarządzania kontami użytkowników (zwykłych i technicznych), grupami systemowymi oraz kluczami SSH na systemach Linux.

**Repozytoria:** GitLab (origin), GitHub (mirror)
**Licencja:** CC-BY-NC-SA-4.0
**Min. Ansible:** 2.10
**Platformy:** Debian/Ubuntu, EL 7/8/9, Alpine

## Struktura projektu

```
defaults/main.yml      — zmienne domyślne (puste listy, ssh_key_type)
vars/main.yml          — zmienne wewnętrzne roli (var_default_shell)
tasks/
  main.yml             — punkt wejścia: walidacja → grupy → konta techniczne → konta użytkowników
  validate.yml         — walidacja danych wejściowych
  create_account.yml   — tworzenie grupy i użytkownika, include copy_ssh_keys.yml
  copy_ssh_keys.yml    — katalog .ssh, klucze pub/priv, authorized_keys
meta/main.yml          — metadane Ansible Galaxy
molecule/              — testy Molecule (Docker: Ubuntu 22.04, Rocky 9)
```

## Konwencje kodu

### Zmienne — prefiksy są obowiązkowe
- `in_` — zmienne wejściowe (interfejs roli), muszą mieć default w `defaults/main.yml`
- `var_` — zmienne wewnętrzne (między taskami), definiowane wyłącznie w `vars/main.yml`
- `users_management_` — zmienne globalne roli (konfiguracja)

### Ansible
- **FQCN zawsze** — `ansible.builtin.user`, nie `user`
- Dane wrażliwe: `no_log: true` + `loop_control.label`
- Wartości domyślne: `| default()` lub `| default(omit)`, nigdy puste `default`
- Opcjonalne parametry modułów: `omit`, nie puste stringi
- Warunki `when` jako lista (nie inline `and`)
- Nazwy tasków w języku angielskim, w cudzysłowach

### YAML
- Wcięcia: 2 spacje
- Nagłówek `---` na początku każdego pliku
- Stringi z Jinja2 w cudzysłowach: `"{{ zmienna }}"`
- Mode jako string: `mode: "0700"`, nie `mode: 0700`

### Uprawnienia plików SSH
- `.ssh/` — `0700`
- Klucz prywatny — `0600`
- Klucz publiczny — `0644`

## Git i commity

- Format: [Conventional Commits](https://www.conventionalcommits.org/) — `feat:`, `fix:`, `docs:`, `chore:`, `refactor:`, `test:`
- Język commitów: **polski**
- Branching: `feat/`, `fix/`, `docs/` + krótki opis

## Testowanie

```bash
molecule test       # pełny cykl (lint → create → converge → verify → destroy)
molecule converge   # tylko uruchomienie roli
molecule verify     # tylko weryfikacja
```

- Framework: Molecule z driverem Docker
- Weryfikacja: `verifier: ansible` (nie testinfra)
- Wymagane kolekcje: `ansible-galaxy collection install -r molecule/requirements.yml`

## Decyzje projektowe

- Konta jako **listy** (nie słowniki) — loop bezpośrednio po liście
- `append: true` w module user — nie nadpisuje istniejących grup
- `create_home: true` — jawne tworzenie katalogu domowego
- `manage_dir: false` w authorized_key — katalog `.ssh` tworzony osobnym taskiem
- Nazwy plików kluczy dynamiczne: `id_{{ users_management_ssh_key_type }}`

## Dokumentacja

- Język dokumentacji: **polski**
- README.md — opis roli, zmienne, przykład użycia
- CONTRIBUTING.md — instrukcja dla kontrybutorów (Conventional Commits, MR workflow)
