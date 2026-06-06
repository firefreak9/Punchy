# Punchy — Setup Guide

**Punchy** is a Pebble Time 2 time clock app that tracks your punch in/out times, calculates pay, and can export your data to a personal Google Sheet.

---

## What You Need

- A Pebble Time 2 watch
- The Pebble app on your phone (iOS or Android)
- A Google account (for spreadsheet export — optional)

---

## Installing Punchy

1. Download the `.pbw` file or download it from the Pebble app store
2. Open it on your phone — the Rebble app will offer to install it
3. Once installed, open **Punchy** on your watch

---

## Basic Use

| Button | Action |
|--------|--------|
| **Select** on the calendar | Open the punch menu |
| **Punch In** | Set your start time |
| **Punch Out** | Set your end time |
| **Up / Down** | Adjust the time in 15-minute steps |
| **Select** to confirm | Save the punch |
| **Back** | Go back one screen |

From the **More** menu you can access:
- **Notes** — add a voice note to the day
- **Pay Dashboard** — see today, week, month, and year earnings
- **Clear Day** — wipe a day's data
- **Settings** — adjust colors, pay rate, break time, overtime, and default punch times

---

## Phone Settings

Tap the **gear icon** next to Punchy in the Rebble app to open settings. Changes sync to your watch when you tap **Save to Watch**.

| Setting | Description |
|---------|-------------|
| Hourly Rate | Your base pay in cents (1950 = $19.50/hr) |
| OT Multiplier | Overtime rate (1.50x = time and a half) |
| OT Starts After | How many hours before overtime kicks in |
| Default Punch In | Pre-fills the punch in time picker |
| Default Punch Out | Pre-fills the punch out time picker |
| Default Break Time | Break minutes deducted from total hours |
| Background | Black or white watch background |
| Text Color | Main text color |
| Label Color | Secondary/label text color |

---

## Exporting Your Data

Punchy can send your punch history to a personal Google Sheet. Your data goes directly to your own Google account — nobody else can see it.

### Step 1 — Create Your Google Sheet

1. Go to [sheets.google.com](https://sheets.google.com)
2. Click **Blank** to create a new spreadsheet
3. Name it **Punchy** (or anything you like)

### Step 2 — Add the Export Script

1. In your spreadsheet, click **Extensions → Apps Script**
2. Delete all the existing code in the editor
3. Paste the following code:

```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var data = JSON.parse(e.postData.contents);
  var rows = data.rows;

  // Add header row if sheet is empty
  if (sheet.getLastRow() === 0) {
    sheet.appendRow(['Date', 'Day', 'Punch In', 'Punch Out', 'Hours', 'Pay', 'Rate', 'Exported']);
  }

  for (var i = 0; i < rows.length; i++) {
    var parts = rows[i].split(/\s+/);
    var day   = parts[0] || '';
    var month = parts[1] || '';
    var date  = parts[2] || '';
    var times = parts[3] || '';
    var hours = parts[4] || '';
    var pay   = parts[5] || '';
    var rate  = (parts[6] || '').replace('@','');
    var inOut = times.split('-');
    sheet.appendRow([
      month + ' ' + date,
      day,
      inOut[0] || '',
      inOut[1] || '',
      hours,
      pay,
      rate,
      new Date().toLocaleString()
    ]);
  }

  return ContentService
    .createTextOutput('OK')
    .setMimeType(ContentService.MimeType.TEXT);
}
```

4. Click **Save** (the floppy disk icon or Ctrl+S)

### Step 3 — Deploy the Script

1. Click **Deploy → New deployment**
2. Click the gear icon next to "Type" and select **Web app**
3. Fill in the settings:
   - **Description:** Punchy Export (or anything)
   - **Execute as:** Me
   - **Who has access:** Anyone
4. Click **Deploy**
5. If prompted, click **Authorize access** and follow the Google sign-in steps
6. Copy the **Web app URL** — it looks like:
   `https://script.google.com/macros/s/ABC123.../exec`

### Step 4 — Connect to Punchy

1. Open the Rebble app on your phone
2. Tap the gear icon next to Punchy
3. Scroll to the **Export** section
4. Paste your Web app URL into the **Google Sheets URL** field
5. Tap **Save to Watch**

The URL is saved — you only need to do this once.

### Step 5 — Export Your Data

1. Open Punchy settings on your phone
2. Under **Export**, change the dropdown to **Tap Save to export**
3. Tap **Save to Watch**
4. Wait a few seconds while the watch sends your data
5. Your spreadsheet will update automatically

Your sheet will have one row per punch day with date, day of week, punch in time, punch out time, hours worked, pay earned, hourly rate, and the export timestamp.

---

## Tips

- **Historical data** — Punchy stores up to 365 days of punch history on the watch
- **Pay rates** — each punch entry stores the rate that was active at the time, so changing your rate later won't affect old entries
- **Overtime** — the Pay Dashboard splits earnings into regular and OT automatically based on your threshold setting
- **Re-exporting** — running export again will add new rows; it doesn't overwrite existing ones. You can delete duplicates in the sheet if needed.

---

## Troubleshooting

**Settings aren't syncing to the watch**
Make sure Punchy is open and active on the watch when you tap Save in the phone settings.

**Export shows 0 entries**
Open Punchy on your watch first, then trigger the export from the phone settings.

**Google Sheet isn't updating**
Check that the Apps Script was deployed with "Who has access: Anyone" — if it's set to "Only myself" it will reject requests from the app.

**The URL field resets**
The URL is restored automatically when you open settings. If it's gone, just paste it again and tap Save.

---

*Punchy is a personal project built for Pebble Time 2. Data stays on your device and your own Google account.*
