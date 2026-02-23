# Grist Data Import Widget / Widget d'Import de Données pour Grist

> **Author / Auteur :** Said Hamadou
> **License / Licence :** Apache-2.0

---

*[English](#english) | [Français](#français)*

---

<a id="english"></a>

## 🇬🇧 English

Standalone custom widget to import Excel (.xlsx, .xls), CSV, and JSON files into Grist tables.

**Widget URL:** `https://grist-import-widget.vercel.app/`

### 🚀 Quick Start

1. In Grist, click **"Add widget to page"**
2. Select **"Custom"** as the widget type
3. Enter the custom widget URL:
   ```
   https://grist-import-widget.vercel.app/
   ```
4. Set the access level to **"Full document access"**
5. Done! Drag and drop your files to import them.

### 📋 Features

- **Excel import** (.xlsx, .xls): Sheet selection when multiple sheets are available
- **CSV import**: Configurable delimiter and encoding
- **JSON import**: Supports arrays of objects
- **Data preview** before import
- **Automatic column creation** for missing columns
- **New table creation** directly from the widget
- **Batch import** (1000 rows/batch) for large files
- **Table deletion** for existing tables
- **Clear data option** before import
- **Drag & drop** or file picker
- **Bilingual interface** (French / English, auto-detected)
- Maximum file size: 50 MB

### 🔒 Security

- **XSS protection**: All preview data uses `textContent` (no `innerHTML` for user data)
- **Formula injection prevention**: Cell values starting with `=` are prefixed with `'` to prevent Grist formula execution
- **Identifier sanitization**: Table and column names are strictly validated (alphanumeric + underscore only, max 64 chars)
- **Cell size limit**: 100 KB per cell to prevent abuse
- **File size limit**: 50 MB maximum

### 🛠️ Alternative Installation

#### Local Development

```bash
git clone https://github.com/isaytoo/grist-import-widget.git
cd grist-import-widget
python3 -m http.server 8585
```

Then in Grist, use: `http://localhost:8585/index.html`

#### Deploy to Vercel

```bash
vercel --prod
```

### ⚙️ Required Configuration

The widget requires **Full document access** to:
- List available tables
- Read and write data
- Create/delete tables and columns

### 📁 File Structure

```
grist-import-widget/
├── index.html       # Widget UI (HTML + CSS)
├── widget.js        # JavaScript logic (i18n, security, import)
├── package.json     # Metadata (grist section for manifest)
├── vercel.json      # Vercel config (iframe headers)
├── netlify.toml     # Netlify config (alternative)
├── LICENSE          # Apache-2.0
├── .gitignore
└── README.md
```

### 🔧 Grist API Used

- `grist.ready({ requiredAccess: 'full' })` — Initialize with full access
- `grist.docApi.listTables()` — List tables (returns array of strings)
- `grist.docApi.fetchTable(tableId)` — Fetch table data
- `grist.docApi.applyUserActions([...])` — Apply actions (add/remove records, tables, columns)

### 📝 Technical Notes

#### Grist Identifiers

Grist does not support accented characters in column/table identifiers. The widget automatically converts accents to ASCII via `removeAccents()` and `sanitizeIdentifier()`.

#### BulkAddRecord Format

```javascript
await grist.docApi.applyUserActions([
  ['BulkAddRecord', 'TableName', [null, null, ...], {
    column1: [val1, val2, ...],
    column2: [val1, val2, ...],
  }]
]);
```

#### Iframe Headers

For Grist to load the widget in an iframe, the following headers are configured via `vercel.json`:
- `X-Frame-Options: ALLOWALL`
- `Access-Control-Allow-Origin: *`
- `Content-Security-Policy: frame-ancestors *`

---

<a id="français"></a>

## 🇫🇷 Français

Widget personnalisé autonome permettant d'importer des fichiers Excel (.xlsx, .xls), CSV et JSON dans Grist.

**URL du widget :** `https://grist-import-widget.vercel.app/`

### 🚀 Utilisation rapide

1. Dans Grist, cliquez sur **"Ajouter un widget à la page"**
2. Sélectionnez **"Personnalisé"** comme type de widget
3. Dans l'URL du widget personnalisé, entrez :
   ```
   https://grist-import-widget.vercel.app/
   ```
4. Définissez le niveau d'accès sur **"Full document access"**
5. C'est prêt ! Glissez-déposez vos fichiers pour les importer.

### 📋 Fonctionnalités

- **Import Excel** (.xlsx, .xls) : Sélection de la feuille si plusieurs disponibles
- **Import CSV** : Configuration du séparateur et de l'encodage
- **Import JSON** : Support des tableaux d'objets
- **Aperçu des données** avant import
- **Création automatique** des colonnes manquantes
- **Création de nouvelles tables** directement depuis le widget
- **Import par lots** (1000 lignes/lot) pour les gros fichiers
- **Suppression de tables** existantes
- **Option d'effacement** des données existantes avant import
- **Drag & drop** ou sélection de fichier
- **Interface bilingue** (Français / Anglais, détection automatique)
- Taille maximale : 50 MB

### 🔒 Sécurité

- **Protection XSS** : Toutes les données d'aperçu utilisent `textContent` (pas de `innerHTML` pour les données utilisateur)
- **Prévention d'injection de formules** : Les valeurs de cellules commençant par `=` sont préfixées par `'` pour empêcher l'exécution de formules Grist
- **Sanitization des identifiants** : Les noms de tables et colonnes sont strictement validés (alphanumérique + underscore uniquement, max 64 caractères)
- **Limite de taille par cellule** : 100 Ko par cellule pour prévenir les abus
- **Limite de taille de fichier** : 50 Mo maximum

### 🛠️ Installation alternative

#### Développement local

```bash
git clone https://github.com/isaytoo/grist-import-widget.git
cd grist-import-widget
python3 -m http.server 8585
```

Puis dans Grist, utilisez l'URL : `http://localhost:8585/index.html`

#### Déploiement sur Vercel

```bash
vercel --prod
```

### ⚙️ Configuration requise

Le widget nécessite un **accès complet au document** ("Full document access") pour :
- Lister les tables disponibles
- Lire et écrire des données
- Créer/supprimer des tables et colonnes

### 📁 Structure des fichiers

```
grist-import-widget/
├── index.html       # Interface HTML + CSS du widget
├── widget.js        # Logique JavaScript (i18n, sécurité, import)
├── package.json     # Métadonnées (section grist pour le manifest)
├── vercel.json      # Configuration Vercel (headers iframe)
├── netlify.toml     # Configuration Netlify (alternative)
├── LICENSE          # Apache-2.0
├── .gitignore
└── README.md
```

### 🔧 API Grist utilisée

- `grist.ready({ requiredAccess: 'full' })` — Initialisation avec accès complet
- `grist.docApi.listTables()` — Liste des tables (retourne un tableau de strings)
- `grist.docApi.fetchTable(tableId)` — Récupération des données d'une table
- `grist.docApi.applyUserActions([...])` — Application d'actions (ajout/suppression)

### 📝 Notes techniques

#### Identifiants Grist

Grist ne supporte pas les accents dans les identifiants de colonnes/tables. Le widget convertit automatiquement les accents en ASCII via `removeAccents()` et `sanitizeIdentifier()`.

#### Format des actions BulkAddRecord

```javascript
await grist.docApi.applyUserActions([
  ['BulkAddRecord', 'NomTable', [null, null, ...], {
    colonne1: [val1, val2, ...],
    colonne2: [val1, val2, ...],
  }]
]);
```

#### Headers iframe

Pour que Grist puisse charger le widget dans un iframe, les headers suivants sont configurés via `vercel.json` :
- `X-Frame-Options: ALLOWALL`
- `Access-Control-Allow-Origin: *`
- `Content-Security-Policy: frame-ancestors *`

---

## 🔗 Resources / Ressources

- [Grist Custom Widgets Documentation](https://support.getgrist.com/widget-custom/)
- [Grist Plugin API](https://support.getgrist.com/code/modules/grist_plugin_api/)
- [Official grist-widget repository](https://github.com/gristlabs/grist-widget)
