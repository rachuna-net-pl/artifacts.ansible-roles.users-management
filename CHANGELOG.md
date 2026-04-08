# Changelog

Format oparty na [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

## [Unreleased]

### Naprawione
- Krytyczne: naprawiono filtr `.values() | list` na zmiennych listowych w `tasks/main.yml`
- Licencja w `meta/main.yml` zmieniona z MIT na CC-BY-NC-SA-4.0 (zgodnie z plikiem LICENSE)
- Nazwy plików kluczy SSH teraz odzwierciedlają wybrany typ (ed25519 -> id_ed25519)
- Dodano `create_home: true` do tworzenia kont
- Dodano `append: true` aby nie nadpisywać istniejących grup użytkownika
- Dodano `state: present` jawnie do modułu user

### Dodane
- Walidacja danych wejściowych (`tasks/validate.yml`)
- Konfigurowalny typ klucza SSH (`users_management_ssh_key_type`, domyślnie: ed25519)
- Scenariusze testowe Molecule (Ubuntu 22.04, Rocky Linux 9)
- Plik wymagań kolekcji (`molecule/requirements.yml`)
- Etykiety w `loop_control` dla lepszej czytelności logów
- Sekcja testowania w README

### Zmienione
- Minimalna wersja Ansible podniesiona z 2.9 do 2.10 (wymagana przez ansible.posix FQCN)
- Domyślny typ klucza SSH zmieniony z RSA na Ed25519

## [1.0.0]

### Dodane
- Tworzenie grup systemowych
- Tworzenie kont użytkowników i technicznych
- Zarządzanie kluczami SSH (publiczne, prywatne, authorized_keys)
