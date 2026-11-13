# CPW Material Selections (materials.cpoolworks.com)

Rep-facing material selection form. Moved off Viktor Spaces Sept 2026.

## How it works
1. Rep signs in with their name and @cpoolworks.com email, gets a 6 digit code by email, and stays signed in on that device for 30 days. No admin setup: new reps sign themselves up. After 30 days they confirm their email again, so when a CPW mailbox is shut off, access lapses on its own. Instant cutoff: set Active to No on the Reps tab of the log sheet.
2. Rep fills the form with the client. Progress autosaves on the device.
3. Submit posts to the **CPW Material Selections Proxy** (Google Apps Script, owned by info@cpoolworks.com).
4. The proxy checks the rep's session (rep name/email come from the verified session), logs a full copy to the **Material Selections Log** sheet (info@ Drive), and forwards to Claire (Script Property `VIKTOR_WEBHOOK_URL`, now pointed at Claire).
5. Claire does the rest: selection sheet, parks the DocuSign for Thomas to approve, GHL note, ClickUp.
6. If Claire rejects or is down, the rep sees an error (nothing is lost), and info@ gets an email with the log row to resend (`resendRow(n)` in the script).

## Live pricing (Nov 2026)
When the rep picks a client, the page asks the proxy for that job's price sheet (`prices` action). Claire reads the sold **Estimate Review** PDF from the client's Drive folder (Documents / Construction / Materials & Financials) and returns every option's difference against it, client price only. The page then prices each pick on its own: interior finish, coping, waterline tile (Aquabella and NPT, with deco tile by ratio), the pool deck, and any LMN item the rep adds. No Estimate Review on file means no prices; the form works exactly as before.

- Prices never live in this repo or on the page. They come from Claire after the rep signs in.
- The numbers on screen are a display. Claire prices the final picks again from the Estimate Review before any change order.
- Catalog changes that affect price (travertine colors, finish colors, tile lists) follow LMN and the vendor price sheets in Claire's price book.

## Files
- `index.html` built app (React, single file). Do not hand edit.
- `config.js` the proxy /exec URL. Edit this if the Apps Script is redeployed to a new URL.
- `CNAME` GitHub Pages custom domain.
- `materials-source.zip` app source + Apps Script. Catalog edits: change `app/src/lib/catalog.ts`, `npm install && npm run build`, upload the new `dist/index.html`.

## Secrets
The Viktor webhook token lives only in the Apps Script Script Properties (`VIKTOR_WEBHOOK_URL`). Sign in codes and session tokens are stored as SHA-256 hashes.
