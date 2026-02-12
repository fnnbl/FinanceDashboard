# Finance Dashboard - Personal Finance Budget Planner

**Berufsschulprojekt - Fachinformatiker in Fachrichtung Anwendungsentwicklung**

## Projektübersicht

Das Finance Dashboard ist eine webbasierte Anwendung zur Verwaltung persönlicher Budgets und Finanzen. Benutzer können monatliche Einnahmen- und Ausgabenpläne erstellen, verwalten und auswerten.

## Repository-Struktur

Dieses Projekt ist in drei separate Repositories aufgeteilt:

- **[FinanceDashboard](https://github.com/fnnbl/FinanceDashboard)** (Dieses Repositorie) - Hauptdokumentation, Projektmanagement
- **[FinanceDashboardBackend](https://github.com/fnnbl/FinanceDashboardBackend)** - Python/FastAPI Backend
- **[FinanceDashboardFrontend](https://github.com/fnnbl/FinanceDashboardFrontend)** - React Frontend

## Ordnerstruktur

```
FinanceDashboard/
├── docs/           # Projektdokumentation
│   ├── Projektantrag
│   ├── User Stories
│   ├── Benutzerdokumentation
│   └── Projektpräsentation
├── uml/            # UML-Diagramme
├── db/             # Datenbankmodell-Dokumentation
├── test/           # Testprotokolle und Testberichte
├── weeklys/        # Wöchentliche Projektberichte
└── README.md
```

## Technologie-Stack

### Backend
- **Programmiersprache:** Python 3
- **Web-Framework:** FastAPI
- **API-Stil:** REST
- **Asynchrone Programmierung:** async/await
- **ORM:** SQLAlchemy (async) mit AsyncSession
- **Datenbank:** PostgreSQL
- **Datenvalidierung:** Pydantic

### Frontend
- **Programmiersprache:** JavaScript
- **Framework:** React
- **Build-Tool:** Vite

## Projektziele

Das Finance Dashboard ermöglicht:
- Registrierung und Authentifizierung von Benutzern
- Erstellen und Verwalten mehrerer Budget-Pläne
- Anlegen von Einnahmen und Ausgaben mit verschiedenen Zahlungsrhythmen
- Automatische Umrechnung auf monatliche Beträge
- Visualisierung und Auswertung der Finanzplanung

## Installation & Setup

Detaillierte Installationsanweisungen finden sich in den jeweiligen Repository-READMEs:
- [Backend Setup](https://github.com/fnnbl/FinanceDashboardBackend)
- [Frontend Setup](https://github.com/fnnbl/FinanceDashboardFrontend)

## Projektmanagement

- **Projektboard:** [GitHub Projects](https://github.com/users/fnnbl/projects/3)
- **Issue-Tracking:** GitHub Issues in den jeweiligen Repositories
- **Workflow:** GitFlow (main, develop, feature branches)

## Autor

Fynn Blaurock - Fachinformatiker in Fachrichtung Anwendungsentwicklung

## Lizenz

Dieses Projekt ist ein Schulprojekt und dient ausschließlich zu Bildungszwecken.
