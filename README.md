# Laxmi Chit Fund — Season 5 · Live Standings

Auto-updating dashboard for our FPL mini-league. A GitHub Action pulls the official
FPL API every 2 hours, computes every prize standing, and publishes a public page.
No servers, no cost, no logins — league standings are public data.

**Live dashboard:** `https://<your-username>.github.io/<repo-name>/`  *(fill in after setup)*

---

## One-time setup (~10 minutes, no coding)

You only do this once. After that it runs itself until GW38.

### 1. Create the repository
1. Go to **github.com** → sign in → click the **+** (top-right) → **New repository**.
2. **Repository name:** `lcf-s5` (or anything).
3. Set it to **Public** (required for free GitHub Pages). Leave everything else default.
4. Click **Create repository**.

### 2. Upload these files
1. On the new repo page, click **“uploading an existing file”** (the link in the middle).
2. Drag **all** of these in — keep the folder structure:
   - `fpl_tracker.py`
   - `config.json`
   - `requirements.txt`
   - `.gitignore`
   - `README.md`
   - the `.github` folder (contains `workflows/update.yml`)
   - the `docs` folder (contains the starter `index.html`)
3. Click **Commit changes**.

> Tip: if drag-drop won’t include the `.github` folder, use **Add file → Create new file**,
> type `.github/workflows/update.yml` as the name, and paste in that file’s contents.

### 3. Let the Action write back to the repo
1. **Settings** (top of repo) → **Actions** → **General** (left menu).
2. Scroll to **Workflow permissions** → select **Read and write permissions** → **Save**.

### 4. Run it once
1. Go to the **Actions** tab → if prompted, click **“I understand… enable workflows.”**
2. Click **“Update FPL dashboard”** (left) → **Run workflow** → **Run workflow** (green button).
3. Wait ~1 minute for the green tick. This generates the live `docs/` page.

### 5. Turn on the public page
1. **Settings** → **Pages** (left menu).
2. Under **Build and deployment → Source**, pick **Deploy from a branch**.
3. Branch: **main**, folder: **/docs** → **Save**.
4. Wait ~1 minute, refresh. Your URL appears at the top: `https://<username>.github.io/<repo>/`.
5. Share that link in the league group. Done.

---

## How it stays updated
- The Action runs **every 2 hours** automatically (and after every gameweek finishes).
- Each run captures an **ownership snapshot** at the current deadline (for Differential Diamond
  and Pity checks) and recomputes all standings.
- You can force an update anytime: **Actions → Update FPL dashboard → Run workflow**.

## Adding the mini-tournament draws
Mini-tournaments need the random group draw (I generate it after each qualification cut).
When ready, edit **`config.json`** and fill in `mini_tournaments`, e.g.:

```json
{
  "league_id": 1035071,
  "mini_tournaments": [
    {
      "name": "MT1 (GW3–GW8)",
      "group_gws": [3, 4, 5],
      "ko_gws": [6, 7, 8],
      "groups": {
        "A": [374875, 46711, 20569, 9695],
        "B": [1301167, 5419891, 6023506, 5970777]
      }
    }
  ]
}
```
(The numbers are each manager’s FPL entry ID — I’ll provide the filled-in block each MT.)

## What’s in the repo
| File | Purpose |
|---|---|
| `fpl_tracker.py` | Pulls the API, computes every prize, writes the dashboard + Excel |
| `config.json` | League ID and mini-tournament draws |
| `.github/workflows/update.yml` | The 2-hourly auto-run |
| `docs/index.html` | The published dashboard (regenerated each run) |
| `output/…_audit.xlsx` | Cash-league audit workbook (regenerated each run) |
| `snapshots/`, `cache/` | Deadline ownership snapshots + cached finished-GW data |

## Notes
- **Nothing is live-to-the-second during matches** — standings settle on final GW scores
  (net of hits), which is what the prizes are based on.
- **Differential Diamond & Pity** auto-compute from deadline snapshots; the admins confirm any
  Pity edge cases per the rulebook checklist.
- The FPL API is unofficial but has been stable for years; the Excel keeps a permanent record
  regardless.
