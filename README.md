# 🛡 SOC Toolkit

Browser-based security tools. Open `index.html` — no install, no server, nothing leaves your machine.

---

## Tools

### 📡 Wazuh Log Analyzer
Drop a Wazuh JSON export (Elasticsearch format) and get instant charts + a filterable, sortable event table with 21 columns including `Image` and `CommandLine`.

**Example log (Event 4688 — process creation):**
```json
{
  "agent": { "name": "DC01", "ip": "10.0.0.5" },
  "rule": { "level": 15, "description": "Mimikatz detected." },
  "data": { "win": { "eventdata": {
    "subjectUserName": "svc_backup",
    "newProcessName": "C:\\Temp\\m64.exe",
    "commandLine": "m64.exe sekurlsa::logonpasswords",
    "parentProcessName": "C:\\Windows\\System32\\cmd.exe"
  }}}
}
```

Filter by agent, user, rule level, event ID, process name, or time range. Click any cell to see its full value.

**Export from Wazuh Or use Just a Json file:**
```bash
curl -k -u admin:admin \
  "https://WAZUH:9200/wazuh-alerts-4.x-*/_search?size=5000" \
  -d '{"query":{"range":{"timestamp":{"gte":"now-24h"}}}}' \
  | jq '[.hits.hits[]]' > alerts.json
```

---

### 📊 CSV Filter & Viewer
Load any CSV/TSV file, filter per column, sort, and export filtered results.

**Filter syntax:**

| Expression | Meaning |
|---|---|
| `powershell` | contains "powershell" (case-insensitive) |
| `powershell cmd` | process is PowerShell AND parent is cmd |
| `>102400` | file size over 100 KB |
| `=4688` | event ID exactly 4688 |
| `!=SYSTEM` | exclude SYSTEM account |
| `2025-10-09..2025-10-10` | activity within incident window |

#### MFT ($MFT) — extract with MFTECmd:
```powershell
MFTECmd.exe -f "C:\$MFT" --csv C:\out --csvf mft.csv
```
Useful filters: `Extension = .exe` + `IsDeleted = True` (deleted executables) · `HasAds = True` (hidden payloads) · compare `Created0x10` vs `Created0x30` to spot timestomping.

#### UsnJrnl ($J) — extract with MFTECmd:
```powershell
MFTECmd.exe -f "C:\$Extend\$UsnJrnl:$J" --csv C:\out --csvf usnjrnl.csv
```
Useful filters: `UpdateReasons = FileCreate` + `Extension = .exe` · `UpdateReasons = RenameNewName` · `FileAttributes = Hidden`. Cross-reference `EntryNumber` with MFT for full file metadata.

Also works with: Hayabusa, Chainsaw, Zeek, Volatility, EDR exports, PECmd, LECmd, AppCompatCache.

---

## Deploy

```bash
git init && git add . && git commit -m "init"
git remote add origin https://github.com/YOUR_USERNAME/soc-toolkit.git
git push -u origin main
```
Enable **Settings → Pages → main → / (root)** for a live URL.

---

```
soc-toolkit/
├── index.html
├── wazuh-log-viewer.html
├── csv-filter.html
└── README.md
```

> All processing is local. Safe on air-gapped machines.
