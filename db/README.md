# Datenbankmodell-Dokumentation

Dieser Ordner enthält die vollständige Dokumentation des Datenbankmodells in **3. Normalform (3NF)** gemäß den Projektanforderungen (GI).

## Inhalte

- **ER-Diagramm** - Entity-Relationship-Diagramm (visuell)
- **Tabellenstruktur** - Detaillierte Tabellendefinitionen
- **Beziehungen** - Fremdschlüssel und Relationen (1:n, n:m)
- **Normalformen** - Erklärung und Nachweis der 3. Normalform

## Dateien

| Datei | Beschreibung |
|-------|--------------|
| `DATABASE_MODEL.md` | Vollständige Datenbankdokumentation in 3NF

## Datenbank-Technologie

- **Datenbank:** PostgreSQL (produktiv via Railway)
- **ORM:** SQLAlchemy (async) mit AsyncSession

## Entitäten (4 Tabellen)

1. **User** - Benutzerkonten (Authentifizierung)
2. **Plan** - Budget-Pläne (monatliche Finanzplanung)
3. **BudgetItem** - Einnahmen/Ausgaben (Budget-Posten)
4. **Category** - Kategorien (system-weit, Einnahmen/Ausgaben)

## Beziehungen

- **User (1) ↔ (n) Plan** - Ein Benutzer hat mehrere Pläne
- **Plan (1) ↔ (n) BudgetItem** - Ein Plan hat mehrere Budget-Posten
- **Category (1) ↔ (n) BudgetItem** - Eine Kategorie wird in mehreren Posten verwendet
