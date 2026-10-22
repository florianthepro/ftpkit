# ftpkit

`ftpkit` ist eine interne Python-Bibliothek für wiederverwendbare, projektunabhängige Funktionen.

## Struktur

```text
ftpkit/
├── pyproject.toml
├── README.md
├── CHANGELOG.md
├── LICENSE
├── src/
│   └── ftpkit/
│       ├── __init__.py
│       ├── files.py
│       ├── network.py
│       ├── security.py
│       ├── system.py
│       └── validation.py
└── tests/
    ├── test_files.py
    ├── test_network.py
    ├── test_security.py
    ├── test_system.py
    └── test_validation.py
```

## Ablageregeln

- Nur projektunabhängige Python-Funktionen in `ftpkit` ablegen.
- Keine Frontend-Dateien, Zugangsdaten, Datenbanken oder projektspezifische Logik ablegen.
- Jede Datei enthält Funktionen eines klaren Themenbereichs.
- Dateinamen und Funktionen werden klein und mit Unterstrichen geschrieben.
- Eine Funktion erledigt möglichst genau eine Aufgabe.
- Öffentliche Funktionen erhalten Typhinweise und einen kurzen Docstring.
- Externe Pakete nur ergänzen, wenn die Python-Standardbibliothek nicht ausreicht.
- Wächst ein Modul stark, wird es in ein gleichnamiges Unterpaket aufgeteilt.

## Zuordnung

```text
files.py       Dateien, Ordner und Pfade
network.py     Netzwerk, IP, Domain und DNS
security.py    Sicherheitsprüfungen und sichere Hilfsfunktionen
system.py      Betriebssystem- und Systeminformationen
validation.py  Prüfung und Normalisierung von Eingaben
```

Beispiel für eine saubere Funktion:

```python
from pathlib import Path


def resolve_safe_path(base: Path, value: str) -> Path:
    """Löst einen Pfad innerhalb eines erlaubten Basisordners auf."""
    base = base.resolve()
    target = (base / value).resolve()

    if target != base and base not in target.parents:
        raise ValueError("Pfad liegt außerhalb des erlaubten Bereichs")

    return target
```

## Verwendung während der Entwicklung

Repository lokal klonen und im bearbeitbaren Modus installieren:

```bash
git clone https://github.com/florianthepro/ftpkit.git
cd ftpkit
python3 -m venv .venv
. .venv/bin/activate
python -m pip install --editable .
```

Änderungen unter `src/ftpkit/` stehen dadurch sofort in der aktiven Entwicklungsumgebung bereit.

Import in einem Python-Projekt:

```python
from ftpkit.files import resolve_safe_path
from ftpkit.system import system_info
from ftpkit.validation import validate_domain
```

Nicht verwenden:

```python
from ftpkit.files import *
```

## Verwendung in einem anderen lokalen Projekt

```bash
cd /pfad/zum/projekt
python3 -m venv .venv
. .venv/bin/activate
python -m pip install --editable /pfad/zu/ftpkit
```

## Installation aus GitHub

Immer eine feste Version verwenden:

```bash
python -m pip install   "git+https://github.com/florianthepro/ftpkit.git@v0.1.0"
```

Nicht dauerhaft direkt von `main` installieren.

## Tests

```bash
python -m pip install --editable ".[dev]"
python -m pytest
```

Für jede öffentliche Funktion gehört mindestens ein normaler Test und, falls sinnvoll, ein Test für ungültige Eingaben unter `tests/`.

## Neue Funktion hinzufügen

1. Passendes Modul unter `src/ftpkit/` auswählen.
2. Funktion mit Typhinweisen und kurzem Docstring ergänzen.
3. Passenden Test unter `tests/` hinzufügen.
4. Alle Tests ausführen.
5. Änderung in `CHANGELOG.md` eintragen.
6. Für veröffentlichte Änderungen eine neue Version markieren.

## Versionen

```text
0.1.0  neue Funktionen
0.1.1  kompatible Fehlerkorrektur
1.0.0  erste stabile Schnittstelle
2.0.0  inkompatible Änderung
```

Bestehende öffentliche Funktionsnamen und Parameter nicht ohne neue Hauptversion inkompatibel ändern.
