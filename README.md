# Punchy — Setup Guide

**Punchy** is a Pebble time clock app that tracks your punch in/out times, calculates pay, and can export and restore your data via a personal Google Sheet.

---

## Compatible Watches

- Pebble Time 2 (emery)
- Pebble Time Round 2 / Pebble 2 Duo (gabbro)
- Pebble 2 (flint)

---

## What You Need

- A compatible Pebble watch
- The Pebble/Rebble app on your phone (iOS or Android)
- A Google account (for spreadsheet export and restore — optional)

---

## Installing Punchy

1. Download the `.pbw` file or find it in the Rebble app store
2. Open it on your phone — the Rebble app will offer to install it
3. Once installed, open **Punchy** on your watch

---

## Basic Use

| Button | Action |
|--------|--------|
| **Select** on the calendar | Open the punch menu |
| **Up / Down** on calendar | Browse previous days |
| **Back** on a past day | Jump back to today |
| **Punch In** | Set your start time |
| **Punch Out** | Set your end time |
| **Notes** | Add a voice note to the day |
| **More** | Access additional options |
| **Up / Down** on time picker | Adjust time in 15-minute steps |
| **Select** to confirm | Save the punch |
| **Back** | Go back one screen |

From the **More** menu:
- **Pay Dashboard** — today, week, month, and year earnings with reg/OT breakdown
- **Clear Day** — wipe a day's punch data
- **Colors** — change the watch color scheme
- **Settings** — pay rate, break time, overtime rules, default times, and more

---

## Phone Settings

Tap the **gear icon** next to Punchy in the Rebble app to open settings. Changes sync to your watch when you tap **Save to Watch**.

### Pay

| Setting | Description |
|---------|-------------|
| Hourly Rate | Your base pay in cents (1950 = $19.50/hr) |
| OT Multiplier | Overtime pay rate (150 = 1.50x, time and a half) |
| OT Starts After | Hours worked before overtime kicks in |

### Punch Times

| Setting | Description |
|---------|-------------|
| Default Punch In | Pre-fills the punch in time picker |
| Default Punch Out | Pre-fills the punch out time picker |
| Clock Out Nearest 15 Min | When enabled, opening Punch Out pre-selects the current time rounded to the nearest 15 minutes |
| Default Break Time | Break minutes automatically deducted from total hours |

### Colors

| Setting | Description |
|---------|-------------|
| Background | Black or white watch background |
| Text Color | Main text color |
| Label Color | Secondary/label text color |

### Data

| Setting | Description |
|---------|-------------|
| Export Punch Data | Sends your punch history to Google Sheets and emails you a backup link |
| Restore from Google Sheets | Reads your punch history back from your sheet and sends it to the watch |
| Google Sheets URL | Your Apps Script web app URL — paste it here once and it's saved |

---

## Setting Up Google Sheets Export & Restore

Your data goes directly to your own Google account. Nobody else can see it.

### Step 1 — Create Your Google Sheet

1. Go to [sheets.google.com](https://sheets.google.com)
2. Click **Blank** to create a new spreadsheet
3. Name it **Punchy** (or anything you like)

### Step 2 — Add the Script

1. In your spreadsheet, click **Extensions → Apps Script**
2. Delete all existing code in the editor
3. Paste the following:

```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var data = JSON.parse(e.postData.contents);
  var rows = data.rows;

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

function doGet(e) {
  var sheet  = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var last   = sheet.getLastRow();

  if (last <= 1) {
    return ContentService
      .createTextOutput(JSON.stringify({ rows: [] }))
      .setMimeType(ContentService.MimeType.TEXT);
  }

  // getDisplayValues returns plain strings exactly as shown in the sheet
  var values = sheet.getRange(2, 1, last - 1, 7).getDisplayValues();

  return ContentService
    .createTextOutput(JSON.stringify({ rows: values }))
    .setMimeType(ContentService.MimeType.TEXT);
}
```

4. Click **Save** (Ctrl+S)

### Step 3 — Deploy the Script

1. Click **Deploy → New deployment**
2. Click the gear icon next to "Type" and select **Web app**
3. Set the following:
   - **Execute as:** Me
   - **Who has access:** Anyone
4. Click **Deploy**
5. Click **Authorize access** if prompted and follow the Google sign-in steps
6. Copy the **Web app URL** — it looks like:
   `https://script.google.com/macros/s/ABC123.../exec`

> ⚠️ **Important:** If you ever update the script code, you must redeploy. Go to **Deploy → Manage deployments**, click the pencil icon, set version to **New version**, then click **Deploy**. The URL stays the same.

### Step 4 — Connect to Punchy

1. Open Punchy settings on your phone
2. Scroll to the **Data** section
3. Paste your Web app URL into the **Google Sheets URL** field
4. Tap **Save to Watch**

The URL is saved — you only need to do this once.

---

## Exporting Your Data

1. Open Punchy settings on your phone
2. Under **Data**, set **Export Punch Data** to **Tap Save below to export**
3. Tap **Save to Watch**
4. The watch will show an **[ EXPORTING ]** screen with a progress count
5. When done it confirms on the watch, and your sheet updates automatically

Each export appends new rows — it doesn't overwrite existing ones. Entries are added oldest first. If you export multiple times you may get duplicates, which you can delete in the sheet.

---

## Restoring Your Data

If you reinstall Punchy (or switch to the store version from a sideloaded one), you can restore all your punch history from your Google Sheet.

1. Make sure you have exported at least once so your sheet has data
2. Install the new version of Punchy
3. Open Punchy settings on your phone
4. Make sure your **Google Sheets URL** is filled in
5. Set **Restore from Google Sheets** to **Tap Save below to restore**
6. Tap **Save to Watch**
7. The watch shows **[ RESTORING ]** with a live entry count
8. When complete it confirms **RESTORE DONE — N ENTRIES**

> The restore reads punch times exactly as they appear in your sheet, so times are always correct regardless of timezone.

---

## Tips

- **Up to 365 days** of punch history are stored on the watch at any time
- **Pay rates** are stored per entry — changing your rate later won't affect old punches
- **Overtime** is split automatically in the Pay Dashboard based on your threshold setting
- **Clock Out Nearest** is useful if you punch out at roughly the same time each day — it pre-selects the nearest 15-minute mark so you rarely need to scroll
- **Voice notes** are saved per day and visible on the calendar screen

---

## Troubleshooting

**Settings aren't syncing to the watch**
Make sure Punchy is open and active on the watch when you tap Save.

**Export shows 0 entries**
Open Punchy on your watch first, then trigger the export from phone settings.

**Google Sheet isn't updating**
Check that the script is deployed with **Who has access: Anyone**. If set to "Only myself" it will reject the app's requests.

**Restore shows 0 entries**
Make sure you redeployed the script after adding the `doGet` function. Go to Deploy → Manage deployments → edit → New version → Deploy.

**Times restored are wrong**
Make sure your sheet's Punch In and Punch Out columns are formatted as time (showing AM/PM). The restore reads the display value directly from the sheet.

**The URL field resets**
The URL is restored automatically when you open settings. If it's missing, paste it again and tap Save.

---

*Punchy is a personal project. Your data stays on your watch and your own Google account — it is never sent to any third-party server.*
