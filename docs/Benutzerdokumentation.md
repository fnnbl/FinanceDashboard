# Benutzerdokumentation - Finance Dashboard

**Projekt:** Finance Dashboard - Personal Finance Budget Planner

**Autor:** Fynn Blaurock

**Version:** 1.0

**Datum:** 16.03.2026

---

## Inhaltsverzeichnis

1. [Systemvoraussetzungen](#1-systemvoraussetzungen)
2. [Installation](#2-installation)
3. [Nutzung](#3-nutzung)
4. [Wartung](#4-wartung)

---

## 1. Systemvoraussetzungen

### Backend
- Python 3.12 oder höher
- pip (Python-Paketmanager)

### Frontend
- Node.js 18 oder höher
- npm

### Datenbank
- Für lokale Ausführung: keine - SQLite wird automatisch verwendet
- Für Produktivbetrieb: PostgreSQL-Datenbankzugang (z.B. Supabase)

---

## 2. Installation

### 2.1 Repository klonen

```bash
git clone https://github.com/fnnbl/FinanceDashboardBackend.git
git clone https://github.com/fnnbl/FinanceDashboardFrontend.git
```

### 2.2 Backend einrichten

```bash
cd FinanceDashboardBackend
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

**Lokale Ausführung (ohne Konfiguration):**
Ohne `.env`-Datei verwendet das Backend automatisch SQLite. Die Datenbankdatei
`finance.db` wird beim ersten Start im Backend-Verzeichnis angelegt. Tabellen
und Standardkategorien werden ebenfalls automatisch erstellt.

**Produktivbetrieb (PostgreSQL/Supabase):**
Eine `.env`-Datei im Backend-Verzeichnis anlegen:

```
DATABASE_URL=postgresql+asyncpg://<user>:<password>@<host>/<database>
SECRET_KEY=<beliebiger-geheimer-schluessel>
```

Server starten:

```bash
uvicorn app.main:app --host 0.0.0.0 --port 8000
```

Die API ist danach erreichbar unter `http://localhost:8000`.
Die interaktive API-Dokumentation ist verfügbar unter `http://localhost:8000/docs`.

### 2.3 Frontend einrichten

```bash
cd FinanceDashboardFrontend
npm install
npm run dev
```

Das Frontend ist danach erreichbar unter `http://localhost:5173`.

> Der Vite-Dev-Server leitet alle Anfragen unter `/api` automatisch an das Backend auf Port 8000 weiter.

---

## 3. Nutzung

### 3.1 Registrierung und Anmeldung

Beim ersten Aufruf der Anwendung erscheint die Anmeldeseite. Über den Link
"Jetzt registrieren" kann ein neues Konto erstellt werden. Benötigt werden:

- E-Mail-Adresse
- Passwort (mindestens 8, maximal 72 Zeichen)
- Name (optional)

Nach der Registrierung wird man direkt zur Anmeldeseite weitergeleitet.
Nach erfolgreicher Anmeldung gelangt man zum Dashboard.

### 3.2 Dashboard

Das Dashboard gibt einen schnellen Überblick über die eigenen Budget-Pläne.

- **Aktiver Plan** - Einen Plan aus dem Dropdown auswählen, um dessen Kennzahlen
  (Einnahmen, Ausgaben, Bilanz, Anzahl Posten) anzuzeigen.
- **Plan-Vergleich** - Bis zu 3 Pläne gleichzeitig nebeneinander vergleichen.

### 3.3 Pläne

Unter "Pläne" werden alle Budget-Pläne aufgelistet. Folgende Aktionen stehen zur Verfügung:

- **Plan erstellen** - Über den Button "Neuen Plan erstellen". Name ist Pflichtfeld,
  Beschreibung optional.
- **Plan öffnen** - Klick auf "Öffnen" zeigt die Detail-Ansicht mit allen Budget-Posten.
- **Plan bearbeiten** - Name und Beschreibung eines Plans ändern.
- **Plan duplizieren** - Erstellt eine vollständige Kopie des Plans inklusive aller Posten.
- **Plan löschen** - Löscht den Plan und alle zugehörigen Budget-Posten permanent.

#### Plan-Detail

In der Detail-Ansicht eines Plans können Budget-Posten verwaltet werden.
Oben werden die berechneten Kennzahlen angezeigt (monatliche Einnahmen, Ausgaben, Bilanz).

Die Liste der Posten kann gefiltert und sortiert werden:

- **Suche** - Nach Bezeichnung, Kategorie oder Bemerkung suchen.
- **Kategoriefilter** - Nur Posten einer bestimmten Kategorie anzeigen.
- **Sortierung** - Nach Monatsbetrag, Betrag, Bezeichnung, Kategorie oder Rhythmus sortieren.

**Exporte:**

- **PDF exportieren** - Lädt einen formatierten Bericht als PDF-Datei herunter.
- **Excel exportieren** - Lädt alle Posten als Excel-Datei (.xlsx) herunter.

### 3.4 Budget-Posten

Ein Budget-Posten wird über "Posten hinzufügen" angelegt. Folgende Felder stehen zur Verfügung:

| Feld | Beschreibung | Pflichtfeld |
|------|--------------|-------------|
| Typ | Einnahme oder Ausgabe | Ja |
| Kategorie | Zugeordnete Kategorie | Ja |
| Bezeichnung | Name des Postens (z.B. "Miete") | Ja |
| Betrag | Betrag in Euro (positiv) | Ja |
| Zahlungsrhythmus | Monatlich, Vierteljährlich, Halbjährlich, Jährlich | Ja |
| Bemerkung | Optionale Notiz | Nein |

Der monatliche Betrag wird automatisch berechnet:

- Monatlich: Betrag x 1
- Vierteljährlich: Betrag / 3
- Halbjährlich: Betrag / 6
- Jährlich: Betrag / 12

### 3.5 Kategorien

Unter "Kategorien" können eigene Kategorien für Einnahmen und Ausgaben verwaltet werden.

- **Systemkategorien** (z.B. "Miete", "Gehalt") sind vorgegeben und können nicht
  gelöscht werden.
- **Benutzerdefinierte Kategorien** können erstellt, umbenannt und gelöscht werden.
- Beim Löschen einer Kategorie, der noch Posten zugeordnet sind, muss eine
  Ersatzkategorie angegeben werden.

### 3.6 Einstellungen

In der Sidebar unten links:

- **Theme** - Klick auf das Sonne/Mond-Symbol wechselt zwischen hellem und dunklem Modus.
- **Sprache** - Klick auf den Benutzernamen öffnet das Benutzer-Menü. Dort kann unter
  "Sprache" zwischen Deutsch und Englisch gewechselt werden.
- **Abmelden** - Ebenfalls im Benutzer-Menü.

---

## 4. Wartung

### 4.1 Abhängigkeiten aktualisieren

```bash
# Backend
cd FinanceDashboardBackend
source venv/bin/activate
pip install --upgrade -r requirements.txt

# Frontend
cd FinanceDashboardFrontend
npm update
```

### 4.2 Datenbank

Das Backend unterstützt zwei Datenbankmodi:

- **SQLite** (Standard) - Wird automatisch verwendet, wenn keine `.env` vorhanden ist.
  Die Datenbankdatei `finance.db` liegt im Backend-Verzeichnis und kann bei Bedarf
  einfach gelöscht werden, um einen sauberen Zustand herzustellen.
- **PostgreSQL** (Produktiv) - Wird über `DATABASE_URL` in der `.env`-Datei konfiguriert.
  Zugangsdaten dürfen nicht in den Quellcode oder in Git eingecheckt werden.

### 4.3 Logs

Der uvicorn-Server gibt Logs direkt in der Konsole aus. Für einen dauerhaften
Betrieb empfiehlt sich eine Prozessverwaltung wie `systemd` oder `supervisor`,
um den Server bei einem Neustart automatisch wieder zu starten.

### 4.4 Tests ausführen

```bash
cd FinanceDashboardBackend
source venv/bin/activate
pytest tests/ -v
```

Alle Tests laufen gegen eine SQLite-In-Memory-Datenbank und berühren die
Produktionsdatenbank nicht.
