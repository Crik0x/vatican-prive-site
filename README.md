# Vatican Privé — Sito Web

Questa repository contiene **tutto il sito web di Vatican Privé**: dati eventi, pagine HTML, script di generazione markup e automazioni.  
È la **fonte unica di verità** per il sito, il widget calendario e (in futuro) la dashboard membership.

---

## 📁 Struttura della repository

```
vatican-prive-site/
│
├── events.json                        ← dati eventi (fonte unica)
├── generate-markup.js                 ← script generazione JSON-LD
├── README.md                          ← questa guida
│
├── .github/
│   └── workflows/
│       └── generate-markup.yml        ← GitHub Action automatica
│
├── generated/
│   ├── schema_markup.txt              ← markup JSON-LD per Tilda (auto-generato)
│   └── schema_events.json            ← array JSON-LD machine-readable (auto-generato)
│
├── membership/
│   └── index.html                     ← pagina /membership
│
├── eventi/                            ← futuro hub eventi
│   └── index.html
│
└── archive/
    └── events_2025.json               ← archivio eventi passati
```

---

## 🌐 URL del sito

| Pagina | URL |
|--------|-----|
| Membership | `https://www.vaticanprive.com/membership/` |
| Hub eventi | `https://www.vaticanprive.com/eventi/` *(in arrivo)* |
| JSON eventi (raw) | `https://raw.githubusercontent.com/Crik0x/vatican-prive-site/refs/heads/main/events.json` |

---

## ✏️ Come aggiungere o modificare un evento

Apri `events.json` direttamente su GitHub (click sul file → matita in alto a destra)  
oppure modificalo in locale con VS Code e fai `git push`.

### Struttura di un evento (v2)

```json
{
  "id": "evt_004",
  "format_id": "aperitivi_divini",
  "city": "roma",
  "edition": 1,
  "title": "Nome dell'evento",
  "date_start": "2026-07-10T20:00:00",
  "date_end":   "2026-07-10T23:00:00",
  "visibility": "public",
  "published": true,
  "featured": false,
  "language": "it",
  "host": "nicolaj_dortona",
  "location": {
    "name": "Vatican Privé — Sede del Club",
    "address": "Via della Conciliazione 44, 00193 Roma",
    "city": "roma",
    "latitude": 41.90257025285699,
    "longitude": 12.460015758530925,
    "maps_url": "https://maps.app.goo.gl/7onYiX7ZFN8Jfj2bA"
  },
  "capacity": {
    "total": 20,
    "available": 20,
    "waitlist": 0,
    "members_reserved": 10,
    "public_available": 10
  },
  "registration": {
    "required": true,
    "type": "whatsapp",
    "url": "https://api.whatsapp.com/send/?phone=393203194044&text=...",
    "deadline_hours_before": 24,
    "waitlist_enabled": true
  },
  "pricing_event": {
    "osservatore": null,
    "anima": null,
    "costruttore": null,
    "public": null
  },
  "media": {
    "image_card": "https://...",
    "image_hero": "",
    "video_url": ""
  },
  "status": "available",
  "google_calendar_event_id": ""
}
```

> **Nota:** se `pricing_event` è tutto `null`, il widget usa automaticamente i prezzi del format (`formats[].pricing`). Valorizza solo se vuoi sovrascrivere il prezzo per quel singolo evento.

---

## 📌 Valori possibili per i campi principali

### `format_id`
| Valore | Evento |
|--------|--------|
| `aperitivi_divini` | Aperitivi diVini |
| `menti_salotto` | Menti in Salotto |
| `scacchiera_re` | La Scacchiera dei Re |
| `conclave` | Il Conclave |
| `cena_membri` | Cena dei Membri |
| `fuori_mura` | Fuori le Mura |
| `verba_manent` | Verba Manent |
| `cash_flow` | Cash Flow (Kiyosaki) |
| `business_meeting` | Business Meeting |

### `status`
| Valore | Significato |
|--------|-------------|
| `available` | Prenotabile |
| `sold_out` | Tutto esaurito |
| `coming_soon` | Presto disponibile |
| `cancelled` | Annullato — nascosto |

### `visibility`
| Valore | Significato |
|--------|-------------|
| `public` | Visibile a tutti |
| `members` | Solo soci |
| `invite` | Solo su invito |

### `registration.type`
| Valore | Comportamento |
|--------|--------------|
| `whatsapp` | Apre WhatsApp con testo precompilato |
| `form` | Apre form esterno |
| `email` | Apre client email |
| `external` | Link esterno generico |

---

## 🔑 Come generare l'ID evento

Usa sempre il formato `evt_XXX` con numero progressivo:  
`evt_001`, `evt_002`, `evt_003`...

Non riutilizzare mai un ID già usato, nemmeno per eventi cancellati.

---

## ⚡ Flusso di aggiornamento

```
1. Modifica events.json in locale (VS Code)
2. git add events.json
3. git commit -m "Descrizione modifica"
4. git push
5. GitHub Action gira automaticamente (30–60 secondi)
6. GitHub → Actions: verifica che sia andato a buon fine
7. Le pagine Tilda con blocchi dinamici si aggiornano alla prossima visita
```

Il widget sul sito legge `events.json` direttamente da GitHub ad ogni caricamento pagina — **nessun deploy manuale necessario**.

---

## 🗓️ Sincronizzazione Google Calendar

Quando crei un evento anche su Google Calendar, copia l'**Event ID** nel campo `google_calendar_event_id` dell'evento nel JSON.

L'Event ID si trova in Google Calendar:  
*Apri evento → ⋮ menu → Pubblica evento → copia ID*

---

## 🗂️ Aggiungere una nuova pagina HTML

1. Crea una cartella con il nome della pagina (es. `fondatore/`)
2. Aggiungi `index.html` dentro quella cartella
3. `git add . && git commit -m "Aggiunge pagina X" && git push`
4. La pagina sarà disponibile su `https://www.vaticanprive.com/fondatore/`

---

## 📞 Supporto tecnico

Per modifiche strutturali al JSON, al widget o alle pagine HTML,  
fare riferimento al documento `VATICAN_PRIVE_MASTER_v2.md`.
