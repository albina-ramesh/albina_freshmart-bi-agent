# 🛒 FreshMart AI-Agent – BI-Projekt FHNW
## Business Intelligence | ffnlw04 | Prof. Dr. Manuel Renold

---

## Projektbeschreibung

Agentischer BI-Analytik-Agent für **FreshMart Schweiz** – eine fiktive Supermarkt-Kette.

**Datenfluss (genau wie im Kurs gelernt):**
```
Excel (.xlsx)  →  SQLite Datenbank  →  SQL-Abfragen  →  AI-Agent  →  Antworten
```

| Was | Details |
|-----|---------|
| Daten | 10'000 Verkaufstransaktionen, Jahr 2024 |
| Filialen | 8 Filialen in der ganzen Schweiz |
| Produkte | 30 Produkte in 10 Kategorien |
| Technologie | Python + SQLite + Groq API (LLaMA 3) |

---

## Installation (einmalig)

```bash
pip install groq pandas openpyxl
```

---

## Starten – 3 Schritte

```bash
# Schritt 1: Excel-Datei mit 10'000 Datensätzen erstellen
python schritt1_daten_erstellen.py

# Schritt 2: Excel → SQLite Datenbank laden
python schritt2_datenbank.py

# Schritt 3: Groq-Key in schritt3_agent.py eintragen, dann:
python schritt3_agent.py
```

**Groq-Key kostenlos holen:** https://console.groq.com/keys  
In `schritt3_agent.py` Zeile 16 eintragen:
```python
GROQ_API_KEY = "gsk_dein_key_hier"
```

---

## Projektstruktur

```
freshmart_projekt/
├── schritt1_daten_erstellen.py  → Erstellt Excel mit 10'000 Datensätzen
├── schritt2_datenbank.py        → Lädt Excel in SQLite-Datenbank
├── schritt3_agent.py            → AI-Agent (Hauptprogramm)
├── freshmart_verkaufsdaten.xlsx → Excel-Rohdaten (4 Sheets)
├── freshmart.db                 → SQLite-Datenbank
└── README.md                    → Diese Anleitung
```

---

## Datenbankstruktur (SQL)

```sql
-- Dimensionstabellen
filialen (filiale_id, filialname, kanton, groesse, typ)
produkte (produkt_id, produktname, kategorie)

-- Faktentabelle
verkaeufe (transaktion_id, datum, wochentag, monat, quartal,
           filiale_id, produkt_id, menge, preis_chf,
           umsatz_chf, gewinn_chf, marge_pct, zahlungsmethode)

-- Vordefinierte Views
v_umsatz_pro_filiale    → Filial-Ranking
v_umsatz_pro_kategorie  → Kategorie-Analyse
v_top_produkte          → Produkt-Ranking
v_monatstrend           → Zeitreihe
```

---

## Architektur (Agentische Analytik)

```
Frage der Geschäftsleitung
        │
        ▼
  AI-Agent (LLaMA 3 via Groq)
        │
        ├── get_gesamtuebersicht()     → SELECT aus verkaeufe
        ├── get_filialen_ranking()     → SELECT aus v_umsatz_pro_filiale
        ├── get_top10_produkte()       → SELECT aus v_top_produkte
        ├── get_kategorien_analyse()   → SELECT aus v_umsatz_pro_kategorie
        ├── get_monatstrend()          → SELECT aus v_monatstrend
        ├── get_wochentag_analyse()    → GROUP BY wochentag
        ├── get_quartal_vergleich()    → GROUP BY quartal
        ├── get_zahlungsmethoden()     → GROUP BY zahlungsmethode
        └── get_handlungsempfehlungen()→ Mehrere SQL-Abfragen kombiniert
                │
                ▼
         SQLite Datenbank
         (aus Excel geladen)
                │
                ▼
     Antwort + Handlungsempfehlung
```

---

## Konzept (für Dokumentation)

**Problem:**
FreshMart hat 8 Filialen und täglich hunderte Transaktionen.
Die Geschäftsleitung braucht schnelle, datenbasierte Entscheidungsunterstützung
ohne BI-Spezialkenntnisse zu besitzen.

**Lösung – Agentische Analytik:**
- Rohdaten strukturiert in Excel gespeichert
- Daten in SQL-Datenbank mit normalisierten Tabellen geladen
- AI-Agent beantwortet natürlichsprachliche Fragen via SQL-Abfragen
- Konkrete Handlungsempfehlungen werden automatisch abgeleitet

**Methoden aus dem Kurs:**
| Kurs-Thema | Anwendung im Projekt |
|------------|---------------------|
| BI Grundlagen (Woche 1) | Datenquellen, KPIs, Entscheidungsunterstützung |
| SQL & Datenbanken (Woche 1+3) | SQLite mit Fakt- und Dimensionstabellen |
| Data Warehouse (Woche 3+4) | Normalisiertes Schema, SQL-Views |
| Statistische Modelle (Woche 5) | Trend- und Margenanalyse |
| Data Cleansing (Woche 10) | Excel → Datenbank mit Validierung |
| Agentische Analytik (Woche 11-13) | AI-Agent mit Tool-Calling |
