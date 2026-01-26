# <img src="docs/linux.png" alt="linux" height="20"/> users-management

Rola do tworzenia kont użytkowników (zwykłych i technicznych) oraz ich kluczy SSH. 

## Wymagania
- Ansible >= 2.9 z `become` na hostach.
- Obsługiwane rodziny: Debian/Ubuntu, EL 7/8/9, Alpine.

## Zmienne
- `in_user_accounts`: lista użytkowników do utworzenia.
- `in_technical_accounts`: lista kont technicznych do utworzenia.
- `in_groups`: grupy do utworzenia przed zakładaniem użytkowników (lista nazw lub obiektów z `name`, opcjonalnie `gid`, `system`).

Struktura konta:
```yaml
- username: user1
  comment: "Opis"
  shell: /bin/bash           # opcjonalnie
  uid: 1000                  # opcjonalnie
  gid: 1000                  # opcjonalnie
  home_path: /home/user1     # opcjonalnie
  system_groups:             # opcjonalnie
    - sudo
  public_ssh_key: "ssh-... user@host"          # opcjonalnie
  private_ssh_key: "-----BEGIN OPENSSH PRIVATE KEY-----..."  # opcjonalnie
  authorized_keys:
    - authorized_key: "ssh-ed25519 AAAA..."
      state: present
```

## Przykład użycia
```yaml
- hosts: all
  become: true
  roles:
    - role: users-management
      vars:
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

---
## Contributions
Jeśli masz pomysły na ulepszenia, zgłoś problemy, rozwidl repozytorium lub utwórz Merge Request. Wszystkie wkłady są mile widziane!
[Contributions](CONTRIBUTING.md)

---
## License
Projekt licencjonowany jest na warunkach [Licencji MIT](LICENSE).

---
# Author Information
### &emsp; Maciej Rachuna
# <img src="docs/linux.png" alt="rachuna-net.pl" height="100"/>
