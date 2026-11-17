# WPSnippets Library

Κοινή βιβλιοθήκη snippets για το plugin **WPSnippets**. Κάθε site με το plugin τη διαβάζει
από τη σελίδα **WPSnippets → Βιβλιοθήκη** και μπορεί να προσθέσει snippets με ένα κλικ.

> ⚠️ Το repository είναι δημόσιο. **Μην** αποθηκεύετε εδώ κωδικούς, API keys ή στοιχεία πελατών.
> Ό,τι διαφέρει ανά site γράφεται ως μεταβλητή `{{όνομα}}` (βλ. παρακάτω).

## Δομή

```
library/                         ← όλα τα snippets μένουν μέσα σε αυτόν τον φάκελο
├── Admin/
│   └── disable-admin-bar.json
├── Analytics/                   ← κατηγορία
│   ├── Google/                  ← υποκατηγορία (Analytics › Google)
│   │   └── ga4.json
│   └── Meta/
│       └── meta-pixel.json
├── Performance/
│   └── disable-emojis.json
├── Security/
│   ├── disable-xmlrpc.json
│   └── hide-wp-version.json
└── WooCommerce/
    └── Checkout/
        └── remove-company-field.json
```

- **Κάθε φάκελος είναι κατηγορία**, κάθε υποφάκελος υποκατηγορία (όσο βάθος χρειάζεται).
- **Κάθε snippet είναι ένα αρχείο `.json`.** Το όνομα του αρχείου είναι μόνο αναγνωριστικό· ο τίτλος που φαίνεται
  έρχεται από το πεδίο `title`.
- Νέα κατηγορία = νέος φάκελος. Το Git δεν κρατά άδειους φακέλους, άρα μια κατηγορία εμφανίζεται όταν έχει τουλάχιστον ένα snippet.
- Μετακίνηση snippet σε άλλη κατηγορία = μετακίνηση του αρχείου σε άλλο φάκελο.

Συνήθως **δεν χρειάζεται να επεξεργάζεστε αρχεία με το χέρι**: από το plugin, «Αποθήκευση στη βιβλιοθήκη»
→ επιλογή ή πληκτρολόγηση κατηγορίας (π.χ. `Analytics/Google`) → το plugin δημιουργεί τον φάκελο και το αρχείο με commit.

## Μορφή αρχείου

```json
{
  "title": "Google Analytics 4",
  "description": "Τι κάνει — εμφανίζεται στη βιβλιοθήκη.",
  "type": "html",
  "code": "<script>…{{ga4_id}}…</script>",
  "location": "site_header",
  "paragraph": 1,
  "priority": 10,
  "device": "any",
  "conditions": [],
  "tags": ["tracking"],
  "variables": [
    { "name": "ga4_id", "label": "Measurement ID", "default": "", "description": "π.χ. G-XXXXXXX" }
  ],
  "updated": "2026-11-17T00:00:00+00:00"
}
```

| Πεδίο | Υποχρεωτικό | Τιμές |
|---|---|---|
| `title` | ναι | Τίτλος snippet |
| `code` | ναι | Ο κώδικας. Για PHP το `<?php` είναι προαιρετικό |
| `type` | ναι | `php`, `js`, `css`, `html`, `text` |
| `description` | — | Περιγραφή για τη βιβλιοθήκη |
| `location` | — | `everywhere`, `frontend_only`, `admin_only`, `on_demand` (μόνο PHP) · `site_header`, `site_body`, `site_footer`, `before_content`, `after_content`, `before_paragraph`, `after_paragraph`, `archive_before_post`, `admin_header`, `admin_footer`, `shortcode` |
| `paragraph` | — | Αριθμός παραγράφου για `before_paragraph` / `after_paragraph` |
| `priority` | — | Σειρά εκτέλεσης, μικρότερο = νωρίτερα (προεπιλογή 10) |
| `device` | — | `any`, `desktop`, `mobile` |
| `conditions` | — | Συνθήκες εκτέλεσης (όπως τις αποθηκεύει το plugin) |
| `tags` | — | Ετικέτες για αναζήτηση |
| `variables` | — | Περιγραφή των μεταβλητών `{{…}}` (ετικέτα, προεπιλογή) |

## Μεταβλητές

Ό,τι αλλάζει από site σε site (IDs, URLs, κείμενα) γράφεται στον κώδικα ως `{{όνομα}}` — μόνο λατινικά, αριθμοί και `_`.
Όταν ένα site προσθέτει το snippet, το plugin εμφανίζει πεδίο για κάθε μεταβλητή. Οι τιμές αποθηκεύονται **μόνο στο site**
και μπαίνουν στον κώδικα τη στιγμή της εκτέλεσης — δεν επιστρέφουν ποτέ εδώ.
