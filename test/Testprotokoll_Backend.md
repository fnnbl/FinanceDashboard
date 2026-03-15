# Testprotokoll - Backend

**Projekt:** Finance Dashboard

**Autor:** Fynn Blaurock

**Datum:** 15.03.2026

**Testumgebung:** Raspberry Pi, Python 3.13, pytest 8.3.4, SQLite In-Memory (aiosqlite)

---

## Testaufbau

Die Tests laufen vollständig isoliert gegen eine SQLite-In-Memory-Datenbank.
Vor jedem Test werden System-Kategorien geseedet. Die Produktionsdatenbank (PostgreSQL/Supabase)
wird nicht berührt.

Ausführen:
```
cd FinanceDashboardBackend
source venv/bin/activate
pytest tests/ -v
```

---

## 1. Authentifizierung (test_auth.py)

**Endpunkte:** `POST /api/v1/auth/register`, `POST /api/v1/auth/token`, `GET /api/v1/auth/me`

| # | Testfall | Erwartetes Ergebnis | Ergebnis |
|---|----------|---------------------|----------|
| 1 | Registrierung mit gültigen Daten | 201, Benutzerdaten ohne Passwort | BESTANDEN |
| 2 | Registrierung mit bereits genutzter E-Mail | 400, Fehlermeldung | BESTANDEN |
| 3 | Registrierung mit ungültiger E-Mail | 422 | BESTANDEN |
| 4 | Registrierung mit zu kurzem Passwort (< 8 Zeichen) | 422 | BESTANDEN |
| 5 | Registrierung mit zu langem Passwort (> 72 Zeichen) | 422 | BESTANDEN |
| 6 | Registrierung ohne E-Mail | 422 | BESTANDEN |
| 7 | Registrierung ohne Name | 201, name ist null | BESTANDEN |
| 8 | Login mit korrekten Zugangsdaten | 200, JWT-Token | BESTANDEN |
| 9 | Login mit falschem Passwort | 401 | BESTANDEN |
| 10 | Login mit nicht existierendem Benutzer | 401 | BESTANDEN |
| 11 | Login ohne Zugangsdaten | 422 | BESTANDEN |
| 12 | Profil abrufen mit gültigem Token | 200, Benutzerdaten | BESTANDEN |
| 13 | Profil abrufen ohne Token | 403 | BESTANDEN |
| 14 | Profil abrufen mit ungültigem Token | 401 | BESTANDEN |

**Ergebnis: 14/14 bestanden**

---

## 2. Plan-Verwaltung (test_plans.py)

**Endpunkte:** `GET/POST /api/v1/plans/`, `GET/PUT/DELETE /api/v1/plans/{id}`,
`POST /api/v1/plans/{id}/duplicate`, `GET /api/v1/plans/{id}/export/pdf`,
`GET /api/v1/plans/{id}/export/csv`

| # | Testfall | Erwartetes Ergebnis | Ergebnis |
|---|----------|---------------------|----------|
| 1 | Planliste abrufen (leer) | 200, leeres Array | BESTANDEN |
| 2 | Nur eigene Pläne werden zurückgegeben | Pläne anderer Nutzer nicht sichtbar | BESTANDEN |
| 3 | Planliste ohne Authentifizierung | 403 | BESTANDEN |
| 4 | Plan erstellen mit gültigen Daten | 201, Plan mit Stats (alle 0) | BESTANDEN |
| 5 | Plan erstellen ohne Beschreibung | 201, description ist null | BESTANDEN |
| 6 | Plan erstellen ohne Name | 422 | BESTANDEN |
| 7 | Plan erstellen mit leerem Namen | 422 | BESTANDEN |
| 8 | Plan erstellen ohne Authentifizierung | 403 | BESTANDEN |
| 9 | Plan abrufen | 200, Plandaten | BESTANDEN |
| 10 | Nicht existierenden Plan abrufen | 404 | BESTANDEN |
| 11 | Plan eines anderen Nutzers abrufen | 403 | BESTANDEN |
| 12 | Plan abrufen enthält berechnete Stats | Monatsbeträge korrekt berechnet | BESTANDEN |
| 13 | Plan aktualisieren | 200, aktualisierte Daten | BESTANDEN |
| 14 | Nicht existierenden Plan aktualisieren | 404 | BESTANDEN |
| 15 | Plan eines anderen Nutzers aktualisieren | 403 | BESTANDEN |
| 16 | Plan löschen | 204, Plan danach nicht mehr abrufbar | BESTANDEN |
| 17 | Nicht existierenden Plan löschen | 404 | BESTANDEN |
| 18 | Plan eines anderen Nutzers löschen | 403 | BESTANDEN |
| 19 | Plan löschen entfernt verknüpfte Budget-Posten | Budget-Posten werden kaskadiert gelöscht | BESTANDEN |
| 20 | Plan duplizieren | 201, Kopie mit allen Budget-Posten | BESTANDEN |
| 21 | Nicht existierenden Plan duplizieren | 404 | BESTANDEN |
| 22 | Plan eines anderen Nutzers duplizieren | 403 | BESTANDEN |
| 23 | PDF-Export | 200, Content-Type: application/pdf | BESTANDEN |
| 24 | Excel-Export | 200, Content-Type: spreadsheetml | BESTANDEN |
| 25 | Export eines Plans eines anderen Nutzers | 403 | BESTANDEN |

**Ergebnis: 25/25 bestanden**

---

## 3. Budget-Posten (test_budget_items.py)

**Endpunkte:** `GET/POST /api/v1/plans/{id}/items/`, `PUT/DELETE /api/v1/plans/{id}/items/{item_id}`

| # | Testfall | Erwartetes Ergebnis | Ergebnis |
|---|----------|---------------------|----------|
| 1 | Budget-Posten-Liste abrufen (leer) | 200, leeres Array | BESTANDEN |
| 2 | Liste ohne Authentifizierung | 403 | BESTANDEN |
| 3 | Liste eines Plans eines anderen Nutzers | 403 | BESTANDEN |
| 4 | Ausgaben-Posten erstellen | 201, korrekte Daten und monthly_amount | BESTANDEN |
| 5 | Einnahmen-Posten erstellen | 201, type = income | BESTANDEN |
| 6 | Monatsbetrag bei monatlichem Rhythmus | monthly_amount = Betrag | BESTANDEN |
| 7 | Monatsbetrag bei vierteljährlichem Rhythmus | monthly_amount = Betrag / 3 | BESTANDEN |
| 8 | Monatsbetrag bei halbjährlichem Rhythmus | monthly_amount = Betrag / 6 | BESTANDEN |
| 9 | Monatsbetrag bei jährlichem Rhythmus | monthly_amount = Betrag / 12 | BESTANDEN |
| 10 | Posten mit Notiz erstellen | 201, note korrekt gespeichert | BESTANDEN |
| 11 | Posten ohne Notiz erstellen | 201, note ist null | BESTANDEN |
| 12 | Posten mit negativem Betrag | 422 | BESTANDEN |
| 13 | Posten mit Betrag 0 | 422 | BESTANDEN |
| 14 | Posten ohne Bezeichnung | 422 | BESTANDEN |
| 15 | Posten mit unbekanntem Zahlungsrhythmus | 422 | BESTANDEN |
| 16 | Posten zu nicht existierendem Plan | 404 | BESTANDEN |
| 17 | Posten zu Plan eines anderen Nutzers | 403 | BESTANDEN |
| 18 | Posten erstellen aktualisiert Plan-Stats | income/expense/balance korrekt | BESTANDEN |
| 19 | Betrag aktualisieren | 200, neuer Betrag und monthly_amount | BESTANDEN |
| 20 | Bezeichnung aktualisieren | 200, neue Bezeichnung | BESTANDEN |
| 21 | Zahlungsrhythmus ändern berechnet Monatsbetrag neu | monthly_amount korrekt neu berechnet | BESTANDEN |
| 22 | Nicht existierenden Posten aktualisieren | 404 | BESTANDEN |
| 23 | Posten eines anderen Nutzers aktualisieren | 403 | BESTANDEN |
| 24 | Posten löschen | 204, Posten nicht mehr in Liste | BESTANDEN |
| 25 | Posten löschen aktualisiert Plan-Stats | budget_item_count und Beträge korrekt | BESTANDEN |
| 26 | Nicht existierenden Posten löschen | 404 | BESTANDEN |
| 27 | Posten eines anderen Nutzers löschen | 403 | BESTANDEN |

**Ergebnis: 27/27 bestanden**

---

## 4. Kategorien (test_categories.py)

**Endpunkte:** `GET /api/v1/categories/`, `POST /api/v1/categories/`,
`PUT /api/v1/categories/{id}`, `DELETE /api/v1/categories/{id}`

| # | Testfall | Erwartetes Ergebnis | Ergebnis |
|---|----------|---------------------|----------|
| 1 | Alle Kategorien abrufen | 200, nicht-leere Liste | BESTANDEN |
| 2 | System-Kategorien enthalten | "Gehalt" und "Miete" vorhanden | BESTANDEN |
| 3 | Nach Typ "expense" filtern | Nur Ausgaben-Kategorien | BESTANDEN |
| 4 | Nach Typ "income" filtern | Nur Einnahmen-Kategorien | BESTANDEN |
| 5 | Kategorien ohne Authentifizierung | 403 | BESTANDEN |
| 6 | Filtern mit unbekanntem Typ | 422 | BESTANDEN |
| 7 | Ausgaben-Kategorie erstellen | 201, is_system = false | BESTANDEN |
| 8 | Einnahmen-Kategorie erstellen | 201, type = income | BESTANDEN |
| 9 | Kategorie mit doppeltem Namen erstellen | 409 | BESTANDEN |
| 10 | Kategorie mit System-Kategorie-Namen erstellen | 409 | BESTANDEN |
| 11 | Kategorie ohne Namen erstellen | 422 | BESTANDEN |
| 12 | Kategorie ohne Typ erstellen | 422 | BESTANDEN |
| 13 | Kategorie mit unbekanntem Typ erstellen | 422 | BESTANDEN |
| 14 | Kategorie mit leerem Namen erstellen | 422 | BESTANDEN |
| 15 | Kategorie ohne Authentifizierung erstellen | 403 | BESTANDEN |
| 16 | Erstellte Kategorie erscheint in Liste | Kategorie abrufbar | BESTANDEN |
| 17 | Kategoriename umbenennen | 200, neuer Name | BESTANDEN |
| 18 | Kategorie mit gleichem Namen umbenennen | 200, unverändert | BESTANDEN |
| 19 | Umbenennen auf bereits vorhandenen Namen | 409 | BESTANDEN |
| 20 | Umbenennen auf System-Kategorie-Namen | 409 | BESTANDEN |
| 21 | Nicht existierende Kategorie umbenennen | 404 | BESTANDEN |
| 22 | Kategorie ohne Name-Feld aktualisieren | 422 | BESTANDEN |
| 23 | Kategorie ohne Authentifizierung umbenennen | 403 | BESTANDEN |
| 24 | Benutzerdefinierte Kategorie löschen | 204 | BESTANDEN |
| 25 | Gelöschte Kategorie nicht mehr in Liste | Kategorie nicht abrufbar | BESTANDEN |
| 26 | System-Kategorie löschen | 403 | BESTANDEN |
| 27 | Kategorie mit Posten löschen ohne Zuweisung | 409 | BESTANDEN |
| 28 | Kategorie mit Posten löschen mit Zuweisung | 204, Posten neu zugewiesen | BESTANDEN |
| 29 | Ziel-Kategorie bei Zuweisung nicht vorhanden | 404 | BESTANDEN |
| 30 | Nicht existierende Kategorie löschen | 404 | BESTANDEN |
| 31 | Kategorie ohne Authentifizierung löschen | 403 | BESTANDEN |

**Ergebnis: 31/31 bestanden**

---

## Gesamtergebnis

| Testdatei | Bestanden | Gesamt |
|-----------|-----------|--------|
| test_auth.py | 14 | 14 |
| test_plans.py | 25 | 25 |
| test_budget_items.py | 27 | 27 |
| test_categories.py | 31 | 31 |
| **Gesamt** | **97** | **97** |

Alle 97 Tests bestanden. Keine Fehler.
