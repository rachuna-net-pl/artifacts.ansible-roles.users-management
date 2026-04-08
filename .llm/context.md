# users-management — Ansible Role

## Cel
Rola Ansible do zarządzania kontami użytkowników (zwykłych i technicznych), grupami systemowymi oraz kluczami SSH na systemach Linux.

## Struktura projektu
```
tasks/
  main.yml            # Punkt wejścia: walidacja → grupy → konta techniczne → konta użytkowników
  validate.yml        # Walidacja danych wejściowych (typy, wymagane pola, typ klucza SSH)
  create_account.yml  # Tworzenie grupy, użytkownika, include copy_ssh_keys.yml
  copy_ssh_keys.yml   # .ssh dir, klucz publiczny/prywatny, authorized_keys
defaults/main.yml     # Zmienne domyślne (puste listy, ssh_key_type: ed25519)
vars/main.yml         # Zmienne wewnętrzne (var_default_shell: /bin/bash)
meta/main.yml         # Metadane Galaxy, licencja CC-BY-NC-SA-4.0, min Ansible 2.10
handlers/main.yml     # Pusty (nie wymaga handlerów)
molecule/             # Testy Molecule (Docker: Ubuntu 22.04, Rocky 9)
```

## Konwencje
- **Język dokumentacji**: polski
- **Nazewnictwo zmiennych wejściowych**: prefiks `in_` (np. `in_user_accounts`)
- **Zmienne wewnętrzne roli**: prefiks `var_` (np. `var_account`, `var_home_dir`)
- **Zmienne globalne roli**: prefiks `users_management_` (np. `users_management_ssh_key_type`)
- **Dane wrażliwe**: zawsze `no_log: true` (klucze SSH, authorized_keys)
- **Moduły**: wyłącznie FQCN (`ansible.builtin.*`, `ansible.posix.*`)
- **Commity**: format Conventional Commits (`feat:`, `fix:`, `docs:`, `chore:`)

## Zmienne wejściowe
| Zmienna | Typ | Domyślna | Opis |
|---|---|---|---|
| `in_user_accounts` | list[dict] | `[]` | Konta użytkowników |
| `in_technical_accounts` | list[dict] | `[]` | Konta techniczne |
| `in_groups` | list[str\|dict] | `[]` | Grupy systemowe |
| `users_management_ssh_key_type` | str | `ed25519` | Typ klucza: `rsa`, `ed25519`, `ecdsa` |

## Przepływ zadań
```
validate.yml  →  Grupy (in_groups)  →  Konta techniczne  →  Konta użytkowników
                                            │                      │
                                      create_account.yml     create_account.yml
                                            │                      │
                                      copy_ssh_keys.yml      copy_ssh_keys.yml
```

## Zależności
- Kolekcja `ansible.posix` (moduł `authorized_key`)
- Platformy: Debian/Ubuntu, EL 7/8/9, Alpine

## Testowanie
```bash
molecule test          # Pełny cykl (lint → create → converge → verify → destroy)
molecule converge      # Tylko uruchomienie roli
molecule verify        # Tylko weryfikacja
```

## Ważne decyzje projektowe
- Konta definiowane jako **listy** (nie słowniki) — loop bezpośrednio po liście
- `append: true` w module user — nie nadpisuje istniejących grup
- `create_home: true` — jawne tworzenie katalogu domowego
- `manage_dir: false` w authorized_key — katalog `.ssh` tworzony osobnym taskiem z uprawnieniami 0700
- Nazwy plików kluczy dynamiczne: `id_{{ users_management_ssh_key_type }}`
