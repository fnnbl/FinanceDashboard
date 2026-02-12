# Datenbankmodell - Finance Dashboard (3. Normalform)

## Übersicht

Dieses Dokument beschreibt das relationale Datenbankmodell für das Finance Dashboard in **3. Normalform (3NF)**.

---

## Entitäten (Entities)

### 1. User (Benutzer)
Speichert registrierte Benutzer des Systems.

| Attribut | Datentyp | Constraints | Beschreibung |
|----------|----------|-------------|--------------|
| **id** | INTEGER | PRIMARY KEY, AUTO_INCREMENT | Eindeutige Benutzer-ID |
| email | VARCHAR(255) | UNIQUE, NOT NULL | E-Mail-Adresse (Login) |
| password_hash | VARCHAR(255) | NOT NULL | Gehashtes Passwort (bcrypt) |
| name | VARCHAR(100) | NULL | Name des Benutzers (optional) |
| created_at | TIMESTAMP | NOT NULL, DEFAULT CURRENT_TIMESTAMP | Erstellungszeitpunkt |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT CURRENT_TIMESTAMP ON UPDATE | Letztes Update |

**Primärschlüssel:** `id`
**Unique Constraints:** `email`

---

### 2. Plan (Budget-Plan)
Speichert Budget-Pläne eines Benutzers.

| Attribut | Datentyp | Constraints | Beschreibung |
|----------|----------|-------------|--------------|
| **id** | INTEGER | PRIMARY KEY, AUTO_INCREMENT | Eindeutige Plan-ID |
| user_id | INTEGER | FOREIGN KEY (User.id), NOT NULL, ON DELETE CASCADE | Zugehöriger Benutzer |
| name | VARCHAR(200) | NOT NULL | Name des Plans (z.B. "Haushalt 2025") |
| description | TEXT | NULL | Optionale Beschreibung |
| created_at | TIMESTAMP | NOT NULL, DEFAULT CURRENT_TIMESTAMP | Erstellungszeitpunkt |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT CURRENT_TIMESTAMP ON UPDATE | Letztes Update |

**Primärschlüssel:** `id`
**Fremdschlüssel:** `user_id` → `User(id)` ON DELETE CASCADE

**Indices:**
- `idx_plan_user_id` auf `user_id` (für schnelle Abfrage aller Pläne eines Users)

---

### 3. Category (Kategorie)
Speichert Kategorien für Budget-Posten (system-weit).

| Attribut | Datentyp | Constraints | Beschreibung |
|----------|----------|-------------|--------------|
| **id** | INTEGER | PRIMARY KEY, AUTO_INCREMENT | Eindeutige Kategorie-ID |
| name | VARCHAR(100) | UNIQUE, NOT NULL | Name der Kategorie |
| type | ENUM('income', 'expense') | NOT NULL | Typ: Einnahme oder Ausgabe |
| is_system | BOOLEAN | NOT NULL, DEFAULT FALSE | System-Kategorie (nicht löschbar) |
| created_at | TIMESTAMP | NOT NULL, DEFAULT CURRENT_TIMESTAMP | Erstellungszeitpunkt |

**Primärschlüssel:** `id`
**Unique Constraints:** `name`

**Standard-Kategorien (is_system=TRUE):**

**Einnahmen:**
- Gehalt
- Nebenverdienst
- Sonstiges

**Ausgaben:**
- Miete
- Lebenshaltung
- Telefon/Internet
- Vermögensabsicherung
- Persönliche Absicherung
- Freizeit
- Urlaub
- Sport
- ÖPNV
- Abonnements
- Studium
- Vermögensaufbau
- Altersvorsorge
- Kredite/Darlehen
- Konsum
- Sonstige

---

### 4. BudgetItem (Budget-Posten)
Speichert Einnahmen und Ausgaben innerhalb eines Plans.

| Attribut | Datentyp | Constraints | Beschreibung |
|----------|----------|-------------|--------------|
| **id** | INTEGER | PRIMARY KEY, AUTO_INCREMENT | Eindeutige Budget-Posten-ID |
| plan_id | INTEGER | FOREIGN KEY (Plan.id), NOT NULL, ON DELETE CASCADE | Zugehöriger Plan |
| category_id | INTEGER | FOREIGN KEY (Category.id), NOT NULL | Kategorie |
| description | VARCHAR(200) | NOT NULL | Beschreibung (z.B. "Miete", "Netflix") |
| amount | DECIMAL(10,2) | NOT NULL, CHECK (amount > 0) | Betrag (Original) |
| type | ENUM('income', 'expense') | NOT NULL | Typ: Einnahme oder Ausgabe |
| payment_rhythm | ENUM('monthly', 'quarterly', 'semi_annually', 'annually') | NOT NULL | Zahlungsrhythmus |
| note | TEXT | NULL | Optionale Bemerkung |
| created_at | TIMESTAMP | NOT NULL, DEFAULT CURRENT_TIMESTAMP | Erstellungszeitpunkt |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT CURRENT_TIMESTAMP ON UPDATE | Letztes Update |

**Primärschlüssel:** `id`
**Fremdschlüssel:**
- `plan_id` → `Plan(id)` ON DELETE CASCADE
- `category_id` → `Category(id)` (kein CASCADE, da Kategorie erhalten bleibt)

**Indices:**
- `idx_budgetitem_plan_id` auf `plan_id` (für schnelle Abfrage aller Items eines Plans)
- `idx_budgetitem_category_id` auf `category_id` (für Kategorieauswertungen)

**Check Constraints:**
- `amount > 0` (Betrag muss positiv sein)

---

## Beziehungen (Relationships)

```
User (1) ────< Plans (n)
                  │
                  └────< BudgetItems (n) ────> Category (1)
```

### 1:n - User → Plan
- Ein **User** kann **mehrere Plans** haben
- Ein **Plan** gehört zu **genau einem User**
- Bei Löschung des Users werden alle zugehörigen Pläne gelöscht (CASCADE)

### 1:n - Plan → BudgetItem
- Ein **Plan** kann **mehrere BudgetItems** haben
- Ein **BudgetItem** gehört zu **genau einem Plan**
- Bei Löschung des Plans werden alle zugehörigen BudgetItems gelöscht (CASCADE)

### n:1 - BudgetItem → Category
- Ein **BudgetItem** hat **genau eine Category**
- Eine **Category** kann in **mehreren BudgetItems** verwendet werden
- Kategorien werden nicht automatisch gelöscht (kein CASCADE)

---

## ER-Diagramm (Text-Darstellung)

```
┌──────────────────┐
│      User        │
├──────────────────┤
│ PK  id           │
│     email        │◄────────┐
│     password_hash│         │
│     name         │         │ 1:n
│     created_at   │         │
│     updated_at   │         │
└──────────────────┘         │
                             │
                    ┌────────┴─────────┐
                    │      Plan        │
                    ├──────────────────┤
                    │ PK  id           │
                    │ FK  user_id      │
                    │     name         │◄────────┐
                    │     description  │         │
                    │     created_at   │         │ 1:n
                    │     updated_at   │         │
                    └──────────────────┘         │
                                                 │
                                        ┌────────┴─────────┐
                                        │   BudgetItem     │
                                        ├──────────────────┤
                                        │ PK  id           │
                                        │ FK  plan_id      │
                                        │ FK  category_id  ├───┐
                                        │     description  │   │ n:1
                                        │     amount       │   │
                                        │     type         │   │
                                        │     payment_rhy. │   │
                                        │     note         │   │
                                        │     created_at   │   │
                                        │     updated_at   │   │
                                        └──────────────────┘   │
                                                               │
                                                     ┌─────────▼──────────┐
                                                     │     Category       │
                                                     ├────────────────────┤
                                                     │ PK  id             │
                                                     │     name           │
                                                     │     type           │
                                                     │     is_system      │
                                                     │     created_at     │
                                                     └────────────────────┘
```

---

## Berechnete Felder (nicht in DB gespeichert)

### monthly_amount (Monatlicher Betrag)
Wird **nicht** in der Datenbank gespeichert, sondern **on-the-fly** berechnet.

**Berechnung:**
```python
def calculate_monthly_amount(amount, payment_rhythm):
    if payment_rhythm == "monthly":
        return amount
    elif payment_rhythm == "quarterly":
        return amount / 3
    elif payment_rhythm == "semi_annually":
        return amount / 6
    elif payment_rhythm == "annually":
        return amount / 12
```

**Begründung:**
- Vermeidet Datenredundanz
- Garantiert Konsistenz (Wert wird immer korrekt berechnet)
- Einfachere Wartung bei Änderungen der Berechnungslogik