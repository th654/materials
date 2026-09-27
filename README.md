# CPW Material Selections (materials.cpoolworks.com)

Rep-facing material selection form. Moved off Viktor Spaces Sept 2026.

## How it works
1. Rep signs in with their name and @cpoolworks.com email, gets a 6 digit code by email, and stays signed in on that device for 30 days. No admin setup: new reps sign themselves up. After 30 days they confirm their email again, so when a CPW mailbox is shut off, access lapses on its own. Instant cutoff: set Active to No on the Reps tab of the log sheet.
2. Rep fills the form with the client. Progress autosaves on the device.
3. Submit posts to the **CPW Material Selections Proxy** (Google Apps Script, owned by info@cpoolworks.com).
4. The proxy checks the rep's session (rep name/email come from the verified session), logs a full copy to the **Material Selections Log** sheet (info@ Drive), and forwards to the Viktor webhook `/cpw/material-selections-submit`.
5. Viktor does the rest: selection sheet, DocuSign, Drive filing, ClickUp, team notice.
6. If Viktor rejects or is down, the rep sees an error (nothing is lost), and info@ gets an email with the log row to resend (`resendRow(n)` in the script).

## Files
- `index.html` built app (React, single file). Do not hand edit.
- `config.js` the proxy /exec URL. Edit this if the Apps Script is redeployed to a new URL.
- `CNAME` GitHub Pages custom domain.
- `materials-source.zip` app source + Apps Script. Catalog edits: change `app/src/lib/catalog.ts`, `npm install && npm run build`, upload the new `dist/index.html`.

## Secrets
The Viktor webhook token lives only in the Apps Script Script Properties (`VIKTOR_WEBHOOK_URL`). Sign in codes and session tokens are stored as SHA-256 hashes.
