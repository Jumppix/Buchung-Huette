# Belegungs-Genehmigungssystem – Datenbank-Setup

## Erforderliche Änderungen in Supabase

### 1. Bestehende Tabelle `belegungen` erweitern

Führe folgende SQL-Befehle in der Supabase SQL-Konsole aus:

```sql
-- Spalten zur belegungen-Tabelle hinzufügen
ALTER TABLE belegungen 
ADD COLUMN IF NOT EXISTS status TEXT DEFAULT 'approved',
ADD COLUMN IF NOT EXISTS erstellt_von_email TEXT,
ADD COLUMN IF NOT EXISTS erstellt_am TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
ADD COLUMN IF NOT EXISTS genehmigt_von_email TEXT,
ADD COLUMN IF NOT EXISTS genehmigt_am TIMESTAMP WITH TIME ZONE,
ADD COLUMN IF NOT EXISTS aenderungstyp TEXT;
-- Mögliche Werte für status: 'pending', 'approved', 'rejected'
-- Mögliche Werte für aenderungstyp: NULL (Neuerstellung), 'modified' (Änderung)
```

### 2. Neue Tabelle für Änderungsanfragen erstellen

```sql
CREATE TABLE IF NOT EXISTS booking_change_requests (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  booking_id UUID NOT NULL REFERENCES belegungen(id) ON DELETE CASCADE,
  status TEXT DEFAULT 'pending', -- 'pending', 'approved', 'rejected'
  change_type TEXT NOT NULL, -- 'create', 'modify'
  angefordert_von_email TEXT NOT NULL,
  neue_daten JSONB,
  alte_daten JSONB,
  erstellt_am TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  genehmigt_von_email TEXT,
  genehmigt_am TIMESTAMP WITH TIME ZONE,
  ablehnung_grund TEXT
);

CREATE INDEX IF NOT EXISTS idx_booking_change_requests_booking_id 
ON booking_change_requests(booking_id);

CREATE INDEX IF NOT EXISTS idx_booking_change_requests_status 
ON booking_change_requests(status);
```

### 3. Tabelle für Admin-Email-Adressen (optional, für Email-Versand)

```sql
CREATE TABLE IF NOT EXISTS admin_emails (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email TEXT NOT NULL UNIQUE,
  erstellt_am TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Nach dem Erstellen können Admin-Emails hinzugefügt werden:
-- INSERT INTO admin_emails (email) VALUES ('admin1@example.com'), ('admin2@example.com');
```

## Wichtige Hinweise

1. **Bestehende Belegungen**: Alle bestehenden Belegungen bekommen `status = 'approved'`, da sie bereits genehmigt sind.

2. **Email-Versand**: Der Email-Versand bei neuen Anfragen kann über:
   - Supabase Edge Functions (empfohlen für Produktion)
   - Externe API (z.B. SendGrid, Mailgun)
   - Implementiert werden (siehe JavaScript-Code)

3. **Reihenfolge der Schritte**:
   - Zuerst SQL-Befehle in Supabase ausführen
   - Dann das aktualisierte `index.html` hochladen
   - Fertig – das System läuft automatisch
