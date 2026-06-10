[README.md](https://github.com/user-attachments/files/28779045/README.md)
# RVU Productivity Worksheet

A browser-based RVU productivity tracker for clinical providers. Runs as a standalone HTML file — no server, no database, no dependencies to install. Open it locally in any browser or host it on GitHub Pages for access from any device.

**GitHub Pages URL:** `https://brakonsc.github.io/rvu-worksheet`

---

## What it does

- Configure RVU weights per visit type (fill in now or later — app works with partial config)
- Log provider activity by date, visit type, and count
- Calculate total RVUs and estimated compensation (base pay + RVU bonus) in real time
- View compensation breakdown as grouped or stacked bar charts
- Save snapshots of completed reports and compare multiple providers or periods side by side
- Export individual reports and comparisons to CSV
- Export and import full data backups as JSON (for moving data between devices)

---

## Visit types (defaults — all editable, more can be added)

| Visit type | Notes |
|---|---|
| New patient | e.g. 99205 |
| Post-op | Global period — may be 0 |
| Pre-op | Complexity dependent |
| RN visit | e.g. 99211 ~0.18 |
| OR day | Flat daily RVU — enter per day |
| Image review | Modality dependent |

Custom visit types can be added at any time via the **+ Add visit type** button.

---

## Compensation model

```
Period base pay  =  Annual base pay × period fraction
Bonus RVUs       =  max(0, Total RVUs − RVU threshold × period fraction)
RVU bonus        =  Bonus RVUs × conversion factor ($/RVU)
Total comp       =  Period base pay + RVU bonus
```

- **RVU threshold** is optional — leave blank or set to 0 to apply the conversion factor to all RVUs
- Chart and metrics render progressively as config is filled in — no need to complete everything before seeing results
- All three compensation fields (base pay, conversion factor, threshold) are independent — partial config still calculates what it can

---

## Period modes

| Mode | Description |
|---|---|
| 6-month rolling | Default — any 6-month window from the start date |
| Monthly | Single calendar month |
| Weekly | Single week |

All RVU threshold and base pay values are automatically prorated to the selected period.

---

## Data storage

All data is stored in **browser localStorage** on the device where it was entered. Nothing is sent to any external server.

- Data persists automatically between sessions on the same device and browser
- To move data to another device: use **Export all data** → **Import backup**
- Each section has its own reset button (weights, comp config, activity log) — nothing nukes everything at once

---

## Using on multiple devices

Data does not automatically sync between devices. To use on a second device:

1. On the original device click **Export all data** in the header
2. Save the `.json` file
3. On the new device open the app and click **Import backup**
4. Paste the JSON contents and click Import

For a colleague to use the app independently with their own data, they simply open the same URL — their browser stores their own separate data with no overlap.

---

## GitHub Pages deployment

### First-time setup

1. Create a new GitHub repository named `rvu-worksheet`
2. Upload `index.html` to the repo root
3. Go to **Settings → Pages**
4. Set source to **Deploy from a branch → main → / (root)**
5. Click Save — live at `https://[your-username].github.io/rvu-worksheet` within 1–3 minutes

### Updating the app

1. Go to your repo on GitHub
2. Click `index.html` → pencil icon (Edit)
3. Select all → delete → paste the new file contents
4. Click **Commit changes**

GitHub Pages updates within 1–3 minutes. All locally stored data is unaffected by app updates.

---

## Snapshots and comparison view

- Click **Save snapshot** in the Report section to capture a point-in-time record of the current report
- Snapshots store: provider name, period, base pay, RVU bonus, total comp, total RVUs
- Snapshots are self-contained — changing weights or activity after saving does not alter existing snapshots
- In the **Comparison view** section, click any two or more snapshots to compare them side by side on a stacked bar chart
- Use this for: same provider different periods, different providers same period, before/after weight changes

---

## Export formats

| Export | Format | Contents |
|---|---|---|
| Export CSV | `.csv` | Current report: activity summary + compensation estimate |
| Export comparison | `.csv` | Selected snapshots side by side |
| Export all data | `.json` | Complete backup: weights, log, comp config, snapshots, provider meta |

---

## Troubleshooting

**Warning says RVU weights not set even though they are filled in**
The app normalizes visit type names (lowercase, trimmed) before matching weights to log entries. If you see this warning, delete the affected activity log rows and re-add them — the dropdown will pull the current weight labels exactly.

**Chart shows base pay and total but no RVU bonus bar**
Make sure the conversion factor ($/RVU) field is filled in. The RVU bonus calculation requires at least: all weights set + conversion factor entered. The threshold field is optional.

**Data disappeared after clearing browser data**
localStorage is cleared when you clear browser history/cache. Use **Export all data** regularly as a backup, especially before clearing your browser.

**App works on one device but not another**
Data is device and browser specific. Use Export/Import to transfer data between devices.

---

## Stack

- Vanilla HTML, CSS, JavaScript — no build step, no framework
- Chart.js 4.4.1 (loaded from CDN — requires internet on first load; cached after)
- Browser localStorage for persistence
- Single file — everything in `index.html`

---

## Resume / project notes

Built to solve a real clinical operations problem: analyzing NP/PA productivity for RVU-based pay adjustments across multiple visit types including new patients, post-ops, pre-ops, RN visits, OR days, and image reviews. Designed for individual provider worksheets that can be rerun for any provider using the same template.
