# Buchung-Huette

## Übersicht
Single-Page-App für die Hüttenbuchung und Veranstaltungsverwaltung. Alle Funktionen sind in `index.html` enthalten.

## Kalender-Features

### Überlappungsprüfung für Belegungen
- ✅ Die Hütte kann nur von **einer Person/Familie gleichzeitig** belegt sein
- ❌ Überlappende Belegungen werden blockiert
- ✅ **Ausnahme:** Veranstaltungen (Familienversammlung, Arbeitswochenende, Sonstige) können sich mit Belegungen überlappen
- Prüfung erfolgt beim **Erstellen** und **Bearbeiten** von Terminen

### Halbtägige Buchungszeiten
- **Checkin & Checkout:** 12:00 Mittag (nicht 00:00 bis 23:59)
- **Ermöglicht:** Am gleichen Tag können zwei Personen wechseln (eine checkt aus, eine checkt ein)
  - Beispiel: `20.5. (Anreise) – 23.5. (Abreise)` = 20.5. um 12:00 bis 23.5. um 12:00
- **Visuell sichtbar:** Im Kalender werden die Zeiten korrekt angezeigt

## Technische Details

### Neue Funktionen im Code
- `dateToNoon(dateStr)` – Konvertiert Daten zu 12:00-Format
- `hasOverlap(vonStr, bisStr, exceptId)` – Prüft auf Überlappungen mit bestehenden Belegungen

### Datenspeicherung
- **Daten-Format:** `YYYY-MM-DD` (wird intern zu `YYYY-MM-DDTHH:00:00`)
- Supabase speichert nur die Daten, keine Zeiten