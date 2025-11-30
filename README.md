# Free-Trial Tripwire for Gmail

Chrome extension that scans your Gmail for **free trials** and **upcoming subscription charges** so you can cancel before you get billed.

> Early dev / beta. Expect rough edges while this gets built out.

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
- Shows a simple dashboard so you can:
  - Scan for “at-risk” trials and subs in one place  
  - Click through to open the thread in Gmail

*(Some functionality is work-in-progress; see roadmap.)*

---

## Status

- **Version:** 0.1.x (developer preview)
- **Audience:** Developers / power users comfortable loading unpacked extensions
- **Not ready** for non-technical users yet

If you find bugs or false positives/negatives, please open an issue with:
- A redacted screenshot of the email
- What you expected vs what you saw in the extension

---

## How it works (high level)

1. You click the extension while you’re in Gmail.
2. The extension injects a content script into your Gmail tab.
3. It scans visible messages using simple rules / patterns for:
   - Trial / renewal phrasing (`"free trial"`, `"renews"`, `"subscription"`, etc.)
   - Dates near those phrases
   - Currency/amount patterns
4. It builds a list of “trials / subs” and sends that back to the popup UI.
5. The popup renders a table where each row is one detected trial or subscription.

All parsing and detection runs **locally in your browser**.

---

## Installation (developer mode)

1. Download or clone this repo:

   ```bash
   git clone https://github.com/TiltedLunar123/free-trial-tripwire-gmail.git
   cd free-trial-tripwire-gmail
