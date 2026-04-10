# <img src="docs/linux.png" alt="linux" height="30"/> users-management

Rola do tworzenia kont użytkowników (zwykłych i technicznych) oraz ich kluczy SSH.

## Architektura przepływu

```mermaid
flowchart TD
    Start([Start roli]) --> Groups[Utworzenie grup systemowych<br/>ansible.builtin.group]
    Groups --> TechLoop[/Loop: in_technical_accounts/]
    TechLoop --> CreateTech[create_account.yml]
    CreateTech --> UserLoop[/Loop: in_user_accounts/]
    UserLoop --> CreateUser[create_account.yml]
    CreateUser --> End([Koniec])

    subgraph CA [create_account.yml]
        direction TB
        CA1[Utworzenie grupy użytkownika] --> CA2[Utworzenie konta<br/>ansible.builtin.user]
        CA2 --> CA3[Konfiguracja kluczy SSH<br/>.ssh/, pub, priv, authorized_keys]
    end

    CreateTech -.-> CA
    CreateUser -.-> CA
```

## Wymagania
- Ansible >= 2.10 z `become` na hostach.
- Kolekcja `ansible.posix` (do zarządzania `authorized_keys`):
- Obsługiwane rodziny: Debian/Ubuntu, EL 7/8/9, Alpine.

## Zmienne

| Zmienna | Domyślna | Opis |
|---|---|---|
| `in_user_accounts` | `[]` | Lista kont użytkowników do utworzenia |
| `in_technical_accounts` | `[]` | Lista kont technicznych do utworzenia |
| `in_groups` | `[]` | Grupy do utworzenia (lista nazw lub obiektów z `name`, `gid`, `system`) |
| `users_management_ssh_key_type` | `ed25519` | Typ klucza SSH: `rsa`, `ed25519` lub `ecdsa` |

### Struktura konta
```yaml
- username: user1           # wymagane
  comment: "Opis"           # opcjonalnie
  shell: /bin/bash           # opcjonalnie (domyślnie /bin/bash)
  uid: 1000                  # opcjonalnie
  gid: 1000                  # opcjonalnie (tworzy grupę o nazwie username)
  home_path: /home/user1     # opcjonalnie (domyślnie /home/{username})
  system_groups:             # opcjonalnie
    - sudo
  public_ssh_key: "ssh-ed25519 AAAA... user@host"               # opcjonalnie
  private_ssh_key: "-----BEGIN OPENSSH PRIVATE KEY-----..."      # opcjonalnie
  authorized_keys:                                                # opcjonalnie
    - authorized_key: "ssh-ed25519 AAAA..."
      state: present         # present/absent (domyślnie present)
```

## Przykład użycia
```yaml
- hosts: all
  become: true
  roles:
    - role: users-management
      vars:
        users_management_ssh_key_type: ed25519
        in_groups:
          - name: administratorzy
            gid: 4000
          - deweloperzy
        in_user_accounts:
          - username: alice
            comment: "Developer"
            system_groups: [sudo]
            authorized_keys:
              - authorized_key: "ssh-ed25519 AAAA... alice@laptop"
        in_technical_accounts:
          - username: deploy
            comment: "CI deploy user"
            public_ssh_key: "ssh-ed25519 BBBB... ci@server"
```

## Testowanie (Molecule)

Rola zawiera scenariusze Molecule do testów na Ubuntu 22.04 i Rocky Linux 9.

```bash
pip install molecule molecule-docker
ansible-galaxy collection install -r molecule/requirements.yml
molecule test
```

## Walidacja

Rola automatycznie waliduje dane wejściowe:
- Typy zmiennych (`in_groups`, `in_user_accounts`, `in_technical_accounts` muszą być listami)
- Każde konto musi mieć pole `username`
- `users_management_ssh_key_type` musi być jednym z: `rsa`, `ed25519`, `ecdsa`

---
## Contributions
Jeśli masz pomysły na ulepszenia, zgłoś problemy, rozwidl repozytorium lub utwórz Merge Request. Wszystkie wkłady są mile widziane!
[Contributions](CONTRIBUTING.md)

---
## License
[Licencja](LICENSE) oparta na zasadach Creative Commons BY-NC-SA 4.0, dostosowana do potrzeb projektu.

---
# Author Information
### Maciej Rachuna
# <img src="docs/logo.png" alt="rachuna-net.pl" height="100"/>
