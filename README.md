# Fixit Locally

A zero-budget MVP for booking home appliance repair (AC, refrigerator, washing machine, TV, water purifier) in **Tumkur, Karnataka**.

Customers book on a simple landing page. Each booking is saved to a Google Sheet, emailed to the owner, and opened as a ready-to-send WhatsApp message. The owner then forwards the job to an independent technician and earns a commission on each completed job.

**Live site:** https://fixit-star.github.io/Fixit/

---

## How it works

```
Customer fills form on landing page
        |
        +--> Google Apps Script --> new row in Google Sheet + email alert to owner
        |
        +--> WhatsApp opens with booking details --> sent to owner
                                                         |
                                       Owner forwards job to a technician on WhatsApp
```

No app, no server, no paid tools.

## Files

| File | Purpose |
|------|---------|
| `index.html` | The landing page and booking form (single file, no build step) |
| `google-apps-script.gs` | Backend script that saves bookings to Google Sheets and sends email |
| `README.md` | This file |

## Setup

### 1. Google Sheet and script
1. Create a Google Sheet (for example "Fixit Locally Bookings").
2. Copy its **Sheet ID**: the long text in the address bar between `/d/` and `/edit`.
3. In the sheet, open **Extensions > Apps Script**, delete the default code, and paste in `google-apps-script.gs`.
4. Set the two values at the top:
   - `OWNER_EMAIL`: where booking alerts go.
   - `SHEET_ID`: the ID from step 2.
5. Save, choose `testRun` in the function dropdown, and click **Run**. Approve the permissions (**Advanced > Go to project > Allow**). A `Bookings` tab with a test row should appear in the sheet.
6. Click **Deploy > New deployment > Web app** and set:
   - **Execute as:** Me
   - **Who has access:** Anyone
7. Click **Deploy** and copy the **Web app URL** (ends in `/exec`).

### 2. Landing page
Open `index.html` and edit the values near the bottom of the file:

```js
var WA_NUMBER = "91XXXXXXXXXX";   // WhatsApp number with country code, no + or spaces
var SHEET_URL = "https://script.google.com/macros/s/XXXX/exec";  // Web app URL from step 1
```

Also update the brand name, prices, and service areas in the page text to match your business.

### 3. Publish on GitHub Pages
1. Push `index.html` to the `main` branch of this repository.
2. Go to **Settings > Pages**.
3. Set **Source** to **Deploy from a branch**, choose **main** and **/(root)**, and click **Save**.
4. After 1 to 2 minutes the site is live at `https://<username>.github.io/<repo>/`.

## Updating the site
Edit `index.html` on GitHub (pencil icon) and commit. The site updates within a minute or two.

## Updating the script
After any change to the Apps Script code, you must redeploy or the live web app keeps running the old version:
**Deploy > Manage deployments > pencil icon > Version: New version > Deploy**.
The URL stays the same, so the page does not need to change.

## Booking sheet columns

`Time`, `Name`, `Phone`, `Service`, `Area`, `Problem`, `Preferred time`, `Status`, `Technician`, `Job amount`, `Commission`, `Commission received`

The first seven are filled automatically. Use the rest to track dispatch and earnings.

## Business model
- Independent local technicians, not employees.
- Revenue: commission per completed job (suggested 15 to 20%).
- Later: optional monthly subscription for high-volume technicians, after about 50 active technicians.

## Troubleshooting

| Problem | Likely cause and fix |
|---------|----------------------|
| Site shows GitHub 404 | Pages source not set to "Deploy from a branch", or the file is not named `index.html` |
| Bookings not appearing in sheet | `SHEET_URL` is empty or wrong in `index.html`, or the deployment access is not **Anyone**. Open the Web app URL in a private window; it should show "Script function not found: doGet", not a sign-in page |
| Script changes have no effect | The deployment was not redeployed with a **New version** |
| Script cannot find the sheet | `SHEET_ID` is wrong, or the script was deployed from a different Google account than the sheet owner |
| No email alerts | Check `OWNER_EMAIL`, check Spam, and redeploy after changing it |
| Execution errors | Open **Executions** in Apps Script and read the error message |

## Limits
- Free Gmail accounts can send roughly 100 script emails per day.
- The customer must press **Send** in WhatsApp. The sheet and email are the reliable record of every booking.
- The booking form is public, so anyone can submit it. Check bookings before dispatching a technician.

## Roadmap
- QR code posters pointing to the live link
- Social media content plan (Instagram, Facebook, WhatsApp Status)
- Automated WhatsApp messages and online payments
- Booking app or platform and expansion to nearby towns
