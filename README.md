# 🛡 SOC Toolkit

A collection of **browser-based, offline-first** security analyst tools.  
Drop the folder anywhere, open `index.html`, done. No install, no server, no data ever leaves your machine.

---

## Tools

### 📡 Wazuh Log Analyzer [`wazuh-log-viewer.html`]
Visualize and investigate **Wazuh SIEM alert exports** (Elasticsearch JSON format).

**Features**
- Drag & drop or paste raw JSON (`[{"_id":..., "_source":...}]`)
- 5 live charts: event timeline, alerts by agent/user, top processes, rule level distribution
- 21 toggleable columns (timestamp, system time, agent IP, domain, logon ID, image, commandline, and more)
- Drag column headers to resize them
- Click any cell to view its full untruncated content + copy to clipboard
- Filters: agent, user, rule level, event ID, process name (text search), time range
- Sortable table

**Compatible format:** Elasticsearch bulk export — `[{"_id": "...", "_source": {...}}, ...]`

---

### 📊 CSV Filter & Viewer [`csv-filter.html`]
Load and interactively filter **any CSV/TSV/pipe-delimited file**.

**Features**
- Auto-detects delimiter (`,` `;` `\t` `|`), quoted fields with escaped `""`, BOM, CRLF/LF
- Per-column filter inputs and a global search box (multi-term AND)
- Click any column header to sort (numeric vs text auto-detected)
- Pagination: 50 → 1000 rows, or All
- Export currently filtered rows back to CSV

**Filter syntax**
| Expression | Meaning |
|---|---|
| `paris` | contains "paris" (case-insensitive) |
| `paris france` | contains both terms |
| `>100` / `<=50` | numeric comparison |
| `=foo` / `!=bar` | equality / not-equal |
| `10..20` | numeric range (inclusive) |

---

## Usage

### GitHub Pages (recommended)
```bash
# 1. Clone your repo
git clone https://github.com/YOUR_USERNAME/soc-toolkit.git
cd soc-toolkit

# 2. Open locally
open index.html      # macOS
xdg-open index.html  # Linux
start index.html     # Windows
```

Then go to **Settings → Pages → Branch: main → / (root)** to publish.  
Live URL: `https://YOUR_USERNAME.github.io/soc-toolkit/`

---

## Privacy
All file processing happens in your browser using vanilla JavaScript.  
No data is sent to any server. No analytics. No dependencies to install.
