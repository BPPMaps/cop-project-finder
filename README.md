# COP Project Finder

A web tool to track the offshore wind projects we are involved in, find projects similar to yours
(region, phase, foundation type, foundation detail, capacity, turbine rating, water depth) and see who the project responsible is.

## Files
- `index.html` – the whole app (Find project · Project timelines · Portfolio overview tabs; the Database tab is switched off with `SHOW_DATABASE = false` in `index.html`)
- `data/projects.csv` – **the database**. Edit this file to update the site for everyone.
- `data/edit-keys.json` – fingerprints (SHA-256) of each project's edit password, used to unlock **Edit project** in the project pop-up. The passwords themselves are never published.
- `assets/` – Blue Power Partners logo and icons from the BPP brand library, and `assets/flags/` with the BPP round flags for the countries in the database (keep them next to `index.html`). For a project in a new country, add its flag SVG from BPP_Flags.pptx to `assets/flags/` and its name to `FLAG_SLUGS` in `index.html`.

## Put it online with GitHub Pages (one-time, ~5 minutes)
1. On github.com click **New repository**, name it e.g. `cop-project-finder`.
2. Click **uploading an existing file** and drag in `index.html`, `README.md` and the `data` and `assets` folders. Commit.
3. Go to **Settings → Pages**. Under *Build and deployment* choose **Deploy from a branch**, branch `main`, folder `/ (root)`. Save.
4. After a minute the site is live at `https://<your-user-or-org>.github.io/cop-project-finder/`. Share that link.

> Note: on a free GitHub account, Pages sites are public on the internet even if the repository is private.
> If project names or responsibles are confidential, use a GitHub Enterprise organization (private Pages, sign-in required)
> or host the same two files on the company intranet / SharePoint / Azure Static Web Apps.

## Updating the data
- **In Excel:** open `data/projects.csv` in Excel, edit it, save as CSV (UTF-8) named `projects.csv` and upload it to `data/`.
- **Optional – live Google Sheet:** import `projects.csv` into a Google Sheet, *File → Share → Publish to web → CSV*,
  and paste that link into `DATA_URL` near the top of the script in `index.html`. The site then reads the sheet directly.

## Column conventions
| Column | Format |
|---|---|
| Region | `AMER`, `EMEA`, `APAC` |
| Country | Country name as in the BPP flag set (e.g. `Netherlands`, `South Korea`); `USA`, `UK` and similar short forms also work. Shown as a flag on each card |
| SharePoint Link | Full https:// link to the project's SharePoint page. Shown only as a **Project SharePoint** button in the project pop-up |
| Phase | `0 - Potential`, `1 - Concept`, `2 - Early development`, `3 - Under development`, `4 - Under construction`, `5 - Operational` |
| Site Awarded / FC Obtained / Start Installation / COD | A quarter such as `Q2 2030` (also accepts `2030-05-14` or `2030`). Development bar = Site Awarded → FC Obtained; Construction bar = FC Obtained (or Start Installation when FC is missing) → COD (or the end of the last year shown when COD is missing) |
| OSS Type / Nr of OSS | `HVAC`, `HVDC` or `None`, and a number; shown on each card as e.g. `1 × HVDC` |
| EPC Director / Transmission Director | Name or initials; shown on each card under the Project Responsible |
| On Hold | `Yes` / `No` |
| Foundation Type | `Monopile`, `Jacket`, `Floating`, `Multipile`, `Other`, `TBD` (several separated by `;`) |
| Foundation Detail | e.g. `TP`, `TP-less`, `PP`, `SBJ` |
| Project Size (MW) | `806`, `800-1200`, `1.2 GW`, `1+ GW` |
| Water Depth (m) / Turbine Rating (MW) | single value or range `35-49` |

## How matching works
Each active filter is scored per project: 1 = matches, 0.5 = close (adjacent phase, same foundation family such as TP vs TP-less or PP vs SBJ,
or a capacity, depth or turbine rating slightly outside the range), 0 = no match. Unknown values count as no match and are flagged “no data”.
Projects meeting every filter are **full matches**; projects scoring 60 % or more are listed as **similar**.

> Do not put passwords or credentials in `projects.csv`: the file is served with the site and can be downloaded by anyone who can open it.

## Suggested edits from the project pop-up
- **Edit project** asks for that project's edit password (the *Credentials* value in your master sheet).
- After editing, **Review changes** shows old vs new values; **Send by email** opens an email to gfa@bluepp.dk with the changes and the updated CSV row. Nothing changes on the site until the row is pasted into `data/projects.csv`.
- If credentials change or projects are added, regenerate `data/edit-keys.json` from the master sheet (never upload the Credentials column itself).
- This is a convenience lock, not strong security: with short passwords, a determined person could guess them from the fingerprints. Real protection comes from the company sign-in in front of the site.
