# User Stories - Finance Dashboard

## Übersicht
Dieses Dokument enthält alle User Stories für das Finance Dashboard Projekt.

**Legende:**
- 🔴 Must-Have (Kernfunktionalität)
- 🟡 Should-Have (Wichtig, aber nicht kritisch)
- 🟢 Nice-to-Have (Zusatzfeatures)

---

## Programmablauf:
1. **Registrierung/Login** → User-Authentifizierung
2. **Startseite** → Übersicht aller Budget-Pläne des Users
3. **Plan auswählen/anlegen** → Budget-Posten (Einnahmen/Ausgaben) verwalten
4. **Plan-Details** → Monatliche Auswertungen, Diagramme, Kategorieübersicht

---

## Kernkonzept:
Ein **E&A-Plan** ist ein monatlicher Haushaltsplan, der aus **Budget-Posten** besteht.

**Budget-Posten** haben einen **Zahlungsrhythmus**:
- Monatlich → Betrag × 1
- Vierteljährlich → Betrag ÷ 3
- Halbjährlich → Betrag ÷ 6
- Jährlich → Betrag ÷ 12

Alle Beträge werden auf **monatliche Kosten** umgerechnet und in Auswertungen dargestellt.

---

## 0. Benutzerverwaltung & Authentifizierung

### US-000: Benutzerregistrierung 🔴 — 3h
**Als** neuer Besucher
**möchte ich** mich registrieren können,
**um** einen persönlichen Account zu erstellen und meine Budget-Pläne zu verwalten.

**Akzeptanzkriterien:**
- Ich kann eine E-Mail-Adresse und Passwort eingeben
- Das Passwort muss mindestens 8 Zeichen lang sein
- Die E-Mail-Adresse muss eindeutig sein
- Nach erfolgreicher Registrierung werde ich automatisch eingeloggt
- Mein Passwort wird verschlüsselt (gehasht) gespeichert

---

### US-001: Benutzer-Login 🔴 — 3h
**Als** registrierter Benutzer
**möchte ich** mich einloggen können,
**um** auf meine Budget-Pläne zuzugreifen.

**Akzeptanzkriterien:**
- Ich kann mich mit E-Mail und Passwort anmelden
- Bei falschen Zugangsdaten erhalte ich eine aussagekräftige Fehlermeldung
- Nach erfolgreichem Login werde ich zur Startseite (Plan-Übersicht) weitergeleitet
- Meine Session bleibt erhalten (JWT-Token)

---

### US-002: Benutzer-Logout 🔴 — 1h
**Als** eingeloggter Benutzer
**möchte ich** mich ausloggen können,
**um** meine Session zu beenden und meinen Account zu schützen.

**Akzeptanzkriterien:**
- Ich kann mich jederzeit ausloggen
- Mein Token wird invalidiert
- Ich werde zur Login-Seite weitergeleitet

---

## 1. Budget-Plan Verwaltung

### US-010: Budget-Plan anlegen 🔴 — 3h
**Als** eingeloggter Benutzer
**möchte ich** einen neuen Budget-Plan anlegen können,
**um** meine monatlichen Einnahmen und Ausgaben zu planen.

**Akzeptanzkriterien:**
- Ich kann einen aussagekräftigen Plan-Namen vergeben (z.B. "Haushalt 2025", "WG-Budget", "Sparplan Urlaub")
- Ich kann optional eine Beschreibung hinzufügen
- Der Plan wird meinem Account zugeordnet
- Nach Erstellung werde ich zum neuen Plan weitergeleitet
- Der Plan ist zunächst leer (keine Budget-Posten)

---

### US-011: Budget-Pläne anzeigen 🔴 — 4h
**Als** eingeloggter Benutzer
**möchte ich** alle meine Budget-Pläne auf der Startseite sehen,
**um** zwischen verschiedenen Plänen zu wählen oder einen neuen anzulegen.

**Akzeptanzkriterien:**
- Ich sehe eine übersichtliche Liste/Kacheln aller meiner Pläne
- Pro Plan sehe ich:
  - Name und Beschreibung
  - Erstellungsdatum
  - Anzahl der Budget-Posten
  - Monatliche Gesamtbilanz (Einnahmen - Ausgaben)
- Ich kann einen Plan auswählen, um ihn zu öffnen
- Ich kann einen neuen Plan anlegen

---

### US-012: Budget-Plan bearbeiten 🔴 — 2h
**Als** eingeloggter Benutzer
**möchte ich** einen bestehenden Plan bearbeiten können,
**um** Name oder Beschreibung anzupassen.

**Akzeptanzkriterien:**
- Ich kann Name und Beschreibung eines Plans ändern
- Die Änderungen werden sofort gespeichert
- Alle Budget-Posten bleiben dem Plan zugeordnet

---

### US-013: Budget-Plan löschen 🔴 — 2h
**Als** eingeloggter Benutzer
**möchte ich** einen Plan löschen können,
**um** nicht mehr benötigte Pläne zu entfernen.

**Akzeptanzkriterien:**
- Ich kann einen Plan zum Löschen auswählen
- Ich werde gewarnt, dass alle Budget-Posten des Plans ebenfalls gelöscht werden
- Ich muss den Löschvorgang explizit bestätigen
- Der Plan und alle zugehörigen Budget-Posten werden permanent gelöscht

---

## 2. Budget-Posten Verwaltung (innerhalb eines Plans)

### US-020: Budget-Posten anlegen 🔴 — 5h
**Als** eingeloggter Benutzer
**möchte ich** innerhalb eines Plans einen neuen Budget-Posten (Einnahme oder Ausgabe) anlegen können,
**um** meine monatlichen Finanzen zu planen.

**Akzeptanzkriterien:**
- Ich kann zwischen "Einnahme" und "Ausgabe" wählen
- Ich kann eine Beschreibung eingeben (Pflichtfeld, z.B. "Miete", "Gehalt", "Netflix")
- Ich kann einen Betrag eingeben (Pflichtfeld, positiv, max. 2 Dezimalstellen)
- Ich kann eine Kategorie aus einem Dropdown auswählen (Pflichtfeld)
- Ich kann einen Zahlungsrhythmus auswählen (Pflichtfeld):
  - Monatlich
  - Vierteljährlich
  - Halbjährlich
  - Jährlich
- Ich kann optional eine Bemerkung hinzufügen (z.B. "wird im Januar fällig")
- Der monatliche Betrag wird automatisch berechnet und angezeigt
- Der Budget-Posten wird gespeichert und erscheint in der Übersicht

**Berechnungslogik:**
- Monatlich: Betrag × 1
- Vierteljährlich: Betrag ÷ 3
- Halbjährlich: Betrag ÷ 6
- Jährlich: Betrag ÷ 12

---

### US-021: Budget-Posten anzeigen 🔴 — 5h
**Als** eingeloggter Benutzer
**möchte ich** alle Budget-Posten eines Plans in einer Übersicht sehen,
**um** meine geplanten Einnahmen und Ausgaben zu überblicken.

**Akzeptanzkriterien:**
- Ich sehe alle Budget-Posten des aktuellen Plans als Tabelle/Liste
- Die Ansicht ist in zwei Bereiche unterteilt:
  - **Einnahmen** (oben, z.B. grün markiert)
  - **Ausgaben** (unten, z.B. rot markiert)
- Pro Budget-Posten sehe ich:
  - Beschreibung
  - Kategorie
  - Original-Betrag
  - Zahlungsrhythmus
  - **Monatlicher Betrag** (hervorgehoben)
  - Bemerkung (falls vorhanden)
- Am Ende jedes Bereichs sehe ich die Summe der monatlichen Beträge
- Ich sehe die Gesamtbilanz (Einnahmen - Ausgaben)
- Budget-Posten anderer Pläne werden nicht angezeigt

---

### US-022: Budget-Posten bearbeiten 🔴 — 3h
**Als** eingeloggter Benutzer
**möchte ich** einen bestehenden Budget-Posten bearbeiten können,
**um** Änderungen vorzunehmen oder Fehler zu korrigieren.

**Akzeptanzkriterien:**
- Ich kann jeden Budget-Posten auswählen und bearbeiten
- Alle Felder können geändert werden (Beschreibung, Betrag, Kategorie, Zahlungsrhythmus, Bemerkung)
- Der monatliche Betrag wird bei Änderungen automatisch neu berechnet
- Die Änderungen werden gespeichert
- Ich erhalte eine Bestätigung nach erfolgreicher Bearbeitung

---

### US-023: Budget-Posten löschen 🔴 — 2h
**Als** eingeloggter Benutzer
**möchte ich** einen Budget-Posten löschen können,
**um** nicht mehr relevante Posten zu entfernen.

**Akzeptanzkriterien:**
- Ich kann einen Budget-Posten zum Löschen auswählen
- Ich werde um Bestätigung gebeten (Sicherheitsabfrage)
- Der Budget-Posten wird permanent aus der Datenbank gelöscht
- Die Summen und Auswertungen werden automatisch aktualisiert

---

## 3. Kategorienverwaltung

### US-030: Kategorien anzeigen 🔴 — 3h
**Als** eingeloggter Benutzer
**möchte ich** alle verfügbaren Kategorien sehen,
**um** meine Budget-Posten korrekt zu kategorisieren.

**Akzeptanzkriterien:**
- Kategorien sind system-weit verfügbar (nicht pro Plan)
- Kategorien sind nach Typ gruppiert (Einnahmen/Ausgaben)
- Standard-Kategorien sind vorbelegt:

  **Ausgaben-Kategorien:**
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

  **Einnahmen-Kategorien:**
  - Gehalt
  - Nebenverdienst
  - Sonstiges

---

### US-031: Kategorie anlegen 🟡 — 3h
**Als** eingeloggter Benutzer
**möchte ich** eigene Kategorien erstellen können,
**um** meine Budget-Posten individuell zu organisieren.

**Akzeptanzkriterien:**
- Ich kann eine neue Kategorie mit Namen anlegen
- Ich kann den Typ festlegen (Einnahme/Ausgabe)
- Der Kategoriename muss eindeutig sein
- Die Kategorie steht sofort in allen Plänen zur Verfügung

---

### US-032: Kategorie bearbeiten 🟡 — 2h
**Als** eingeloggter Benutzer
**möchte ich** Kategorien umbenennen können,
**um** meine Struktur anzupassen.

**Akzeptanzkriterien:**
- Ich kann den Namen einer Kategorie ändern
- Alle Budget-Posten mit dieser Kategorie werden automatisch aktualisiert
- Der neue Name muss eindeutig sein

---

### US-033: Kategorie löschen 🟡 — 3h
**Als** eingeloggter Benutzer
**möchte ich** nicht mehr benötigte Kategorien löschen können,
**um** meine Kategorienliste übersichtlich zu halten.

**Akzeptanzkriterien:**
- Standard-Kategorien können nicht gelöscht werden
- Wenn Budget-Posten mit dieser Kategorie existieren, werde ich gewarnt
- Ich kann entscheiden, ob die Budget-Posten einer anderen Kategorie zugeordnet werden oder ob der Löschvorgang abgebrochen wird

---

## 4. Auswertungen & Visualisierungen (innerhalb eines Plans)

### US-040: Monatliche Übersicht 🔴 — 4h
**Als** eingeloggter Benutzer
**möchte ich** eine monatliche Übersicht meines Budget-Plans sehen,
**um** zu verstehen, wie viel ich pro Monat einnehme und ausgebe.

**Akzeptanzkriterien:**
- Ich sehe die Gesamtsumme aller monatlichen Einnahmen (alle umgerechnet)
- Ich sehe die Gesamtsumme aller monatlichen Ausgaben (alle umgerechnet)
- Ich sehe die Differenz/Bilanz (Einnahmen - Ausgaben)
- Die Bilanz ist farblich hervorgehoben:
  - Grün = Positiv (Überschuss)
  - Rot = Negativ (Defizit)
- Die Übersicht ist prominent auf der Plan-Detailseite platziert

---

### US-041: Kategorieauswertung 🔴 — 4h
**Als** eingeloggter Benutzer
**möchte ich** sehen, wie viel ich pro Kategorie monatlich ausgebe/einnehme,
**um** meine Ausgabenstruktur zu verstehen.

**Akzeptanzkriterien:**
- Ich sehe eine Übersicht aller Kategorien mit ihren monatlichen Summen
- Die Kategorien sind nach Höhe sortiert (höchste zuerst)
- Ich sehe den prozentualen Anteil jeder Kategorie an den Gesamtausgaben/-einnahmen
- Ich kann zwischen Einnahmen und Ausgaben wechseln
- Die Darstellung erfolgt als Tabelle und/oder Balkendiagramm

---

### US-042: Kategorieverteilung als Diagramm 🔴 — 5h
**Als** eingeloggter Benutzer
**möchte ich** ein Diagramm sehen, das meine Ausgabenverteilung visualisiert,
**um** auf einen Blick zu erkennen, wofür ich am meisten Geld ausgebe.

**Akzeptanzkriterien:**
- Ich sehe ein Tortendiagramm (Pie Chart) oder Donut-Chart der Ausgabenkategorien
- Jede Kategorie ist farblich unterschiedlich dargestellt
- Der prozentuale Anteil ist ersichtlich
- Ich kann zwischen Einnahmen und Ausgaben wechseln
- Kategorien ohne Budget-Posten werden nicht angezeigt

---

### US-043: Plan-Dashboard 🔴 — 5h
**Als** eingeloggter Benutzer
**möchte ich** beim Öffnen eines Plans sofort die wichtigsten Kennzahlen sehen,
**um** schnell einen Überblick zu bekommen.

**Akzeptanzkriterien:**
- Das Dashboard zeigt:
  - **Monatliche Einnahmen** (Summe, grün)
  - **Monatliche Ausgaben** (Summe, rot)
  - **Bilanz** (Differenz, farblich hervorgehoben)
  - **Anzahl Budget-Posten** (Einnahmen/Ausgaben)
  - **Top 5 Ausgabenkategorien** mit Beträgen
  - **Kategorieverteilung als Diagramm**
- Das Dashboard ist die erste Ansicht beim Öffnen eines Plans
- Ich kann von dort aus zur Detail-Tabelle der Budget-Posten wechseln

---

### US-044: Budget-Posten Tabelle mit Sortierung 🟡 — 2h
**Als** eingeloggter Benutzer
**möchte ich** die Budget-Posten nach verschiedenen Kriterien sortieren können,
**um** bestimmte Posten schnell zu finden.

**Akzeptanzkriterien:**
- Ich kann die Tabelle sortieren nach:
  - Beschreibung (alphabetisch)
  - Kategorie
  - Betrag (aufsteigend/absteigend)
  - Monatlichem Betrag (aufsteigend/absteigend)
  - Zahlungsrhythmus
- Die Sortierung erfolgt sofort (client-side)
- Die gewählte Sortierung bleibt während der Session erhalten

---

## 5. Erweiterte Funktionen (Optional)

### US-050: Budget-Posten nach Beschreibung suchen 🟡 — 2h
**Als** eingeloggter Benutzer
**möchte ich** innerhalb eines Plans nach Budget-Posten suchen können,
**um** schnell bestimmte Einträge zu finden.

**Akzeptanzkriterien:**
- Ich kann einen Suchbegriff eingeben
- Die Suche durchsucht Beschreibung, Kategorie und Bemerkung
- Die Ergebnisse werden in Echtzeit gefiltert
- Die Summen berücksichtigen nur die gefilterten Ergebnisse

---

### US-051: Budget-Posten nach Kategorie filtern 🟡 — 2h
**Als** eingeloggter Benutzer
**möchte ich** Budget-Posten nach Kategorie filtern können,
**um** alle Posten einer bestimmten Kategorie zu sehen.

**Akzeptanzkriterien:**
- Ich kann eine oder mehrere Kategorien auswählen
- Nur Budget-Posten der gewählten Kategorien werden angezeigt
- Die Summen werden entsprechend angepasst
- Ich kann den Filter jederzeit zurücksetzen

---

### US-052: Plan duplizieren 🟢
**Als** eingeloggter Benutzer
**möchte ich** einen bestehenden Plan duplizieren können,
**um** einen neuen Plan auf Basis eines vorhandenen zu erstellen.

**Akzeptanzkriterien:**
- Ich kann einen Plan als Vorlage auswählen
- Der neue Plan erhält einen eigenen Namen (z.B. "Kopie von...")
- Alle Budget-Posten werden kopiert
- Ich kann den neuen Plan unabhängig bearbeiten

---

### US-053: Plan als PDF exportieren 🟢
**Als** eingeloggter Benutzer
**möchte ich** einen Plan als PDF exportieren können,
**um** ihn auszudrucken oder offline zu speichern.

**Akzeptanzkriterien:**
- Ich kann einen Plan als PDF herunterladen
- Das PDF enthält:
  - Plan-Name und Beschreibung
  - Alle Budget-Posten in Tabellenform
  - Summen und Bilanz
  - Kategorieauswertung
- Das PDF ist übersichtlich formatiert

---

### US-054: Plan als CSV exportieren 🟢
**Als** eingeloggter Benutzer
**möchte ich** Budget-Posten als CSV exportieren können,
**um** die Daten extern zu analysieren oder zu bearbeiten.

**Akzeptanzkriterien:**
- Ich kann alle Budget-Posten eines Plans als CSV exportieren
- Die CSV enthält alle relevanten Felder (Beschreibung, Typ, Betrag, Kategorie, Zahlungsrhythmus, Monatlicher Betrag)
- Die Datei kann in Excel oder Google Sheets geöffnet werden

---

## Zusammenfassung

**Must-Have (🔴):** 16 User Stories — 54h geplant

**Should-Have (🟡):** 6 User Stories — 14h geplant

**Nice-to-Have (🟢):** 3 User Stories — nicht eingeplant

**Gesamt:** 25 User Stories — **68h Entwicklungszeit (Must-Have + Should-Have)**

---

## Priorisierung für MVP (Minimum Viable Product)

### Phase 1 - Authentifizierung & Basis (Sprint 1) — 14h:
- US-000: Benutzerregistrierung (3h)
- US-001: Benutzer-Login (3h)
- US-002: Benutzer-Logout (1h)
- US-010: Budget-Plan anlegen (3h)
- US-011: Budget-Pläne anzeigen (4h)

### Phase 2 - Plan-Verwaltung (Sprint 2) — 7h:
- US-012: Budget-Plan bearbeiten (2h)
- US-013: Budget-Plan löschen (2h)
- US-030: Kategorien anzeigen mit Standard-Kategorien (3h)

### Phase 3 - Budget-Posten CRUD (Sprint 3) — 15h:
- US-020: Budget-Posten anlegen (5h)
- US-021: Budget-Posten anzeigen (5h)
- US-022: Budget-Posten bearbeiten (3h)
- US-023: Budget-Posten löschen (2h)

### Phase 4 - Auswertungen & Visualisierungen (Sprint 4) — 18h:
- US-040: Monatliche Übersicht (4h)
- US-041: Kategorieauswertung (4h)
- US-042: Kategorieverteilung als Diagramm (5h)
- US-043: Plan-Dashboard (5h)

**MVP fertig nach Phase 4 — 54h**

### Phase 5 - Erweiterte Features — 14h:
- US-031: Kategorie anlegen (3h)
- US-032: Kategorie bearbeiten (2h)
- US-033: Kategorie löschen (3h)
- US-044: Sortierung (2h)
- US-050: Suche (2h)
- US-051: Filter (2h)

**Alle Must-Have + Should-Have nach Phase 5 — 68h**

### Phase 6 - Nice-to-Have (optional):
- US-052: Plan duplizieren
- US-053: PDF Export
- US-054: CSV Export
