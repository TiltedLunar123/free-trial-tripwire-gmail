# Free-Trial Tripwire for Gmail

Chrome extension that scans your Gmail for **free trials** and **upcoming subscription charges** so you can cancel before you get billed.

> v0.2.0 – early beta. Still in active development, expect rough edges.

---

## What it does

- Looks through your Gmail inbox for emails about:
  - Free trials that are about to roll into paid plans  
  - Subscription renewals and upcoming charges  
- Extracts key details where possible:
  - Service / merchant name  
  - Next charge or renewal date  
  - Estimated amount and currency  
  - Sender, subject, and a short snippet  
- Shows a dashboard in the popup so you can:
  - Scan for “at-risk” trials and subs in one place  
  - Filter by **All / Trials / Subs**  
  - Click **Open** to jump straight to the Gmail thread  
  - Click **Remind** to download a calendar reminder (`.ics`) for canceling

All parsing and detection runs **locally in your browser**.

---

## What’s new in 0.2.0

- 🧮 **Smarter parsing**
  - Better date parsing (more formats + some relative phrases like “in 3 days” / “next month”)  
  - More robust amount parsing and basic multi-currency support (USD/EUR/GBP)

- 📊 **Richer popup UI**
  - Summary pills: Trials vs Subs count and an estimate of charges in the **next 30 days**  
  - Per-row **Open** + **Remind** actions  
  - Improved status updates: already scanning, waiting for results, scan errors, etc.

- 📦 **Internal cleanup**
  - Shared `FTTUtils` and `FTTStorage` helpers (less duplication, easier to extend)  
  - Each trial now tracks `firstSeenAt` / `lastSeenAt` and optional confidence

---

## Status

- **Version:** 0.2.0 (developer preview)
- **Audience:** Developers / power users comfortable loading unpacked extensions
- **Not polished** for non-technical users yet

If you find bugs or false positives/negatives, please open an issue with:
- A redacted screenshot of the email
- What you expected vs what you saw in the extension

---

## How it works (high level)

1. You click the extension while you’re in Gmail.  
2. The background script finds or opens a Gmail tab and injects `gmailScanner.js`.  
3. The scanner:
   - Reads visible rows (sender, subject, snippet, thread link)  
   - Uses keyword rules to classify **trial** vs **subscription**  
   - Tries to parse dates and amounts from nearby text  
   - Builds a list of “trial/subscription” objects (with optional confidence + thread URL)  
4. The background merges results into `chrome.storage.local` and notifies the popup.  
5. The popup renders a table + summary using that stored data.

No Gmail API, no external servers, no analytics.

---

## Permissions

From `manifest.json`:

- `"tabs"` – find/open your Gmail tab  
- `"scripting"` – inject the scanner into Gmail  
- `"storage"` – save detected trials/subs locally  
- `"host_permissions": ["https://mail.google.com/*"]` – allow running on Gmail

No `identity`, no external URLs.

---

## Installation (developer mode)

1. Clone this repo:

   ```bash
   git clone https://github.com/TiltedLunar123/free-trial-tripwire-gmail.git
   cd free-trial-tripwire-gmail
