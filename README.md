# Punchy — Setup Guide

Punchy is a Pebble time clock app that tracks your punch in/out times, calculates pay — including overtime, tax withholding, and periodic deductions — and can export and restore your data via a personal Google Sheet.

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
|---|---|
| Select on the calendar | Open the punch menu |
| **Hold** Select on the calendar | Jump straight to editing whichever punch (In/Out) is next needed |
| Up / Down on calendar | Browse previous days |
| Back on a past day | Jump back to today |
| Punch In | Set your start time |
| Punch Out | Set your end time |
| Notes | Add a voice note to the day |
| More | Access additional options |
| Up / Down on time picker | Adjust time in your configured snap increment (1, 5, or 15 min) |
| **Hold** Select on a time picker | Switch to precise hour → minute editing |
| Select to confirm | Save the punch |
| Back | Go back one screen (or one editing stage, if in hour/minute mode) |

From the **More** menu:

- **Pay Dashboard** — today, week, month, and year earnings with reg/OT breakdown, and a **NET** figure if you've set up tax or deductions
- **Clear Day** — wipe a day's punch data
- **Colors** — change the watch color scheme
- **Settings** — pay rate, break time, overtime rules, snap settings, pay modifiers, app timeout, and more

---

## New: Precise Time Editing (Hold to Edit)

Every time picker (Punch In, Punch Out, Default In, Default Out) now supports two levels of adjustment:

1. **Normal mode** — Up/Down move in your configured snap increment (see Snap Settings below). This is the default when you open any picker.
2. **Hold Select** to switch into **hour/minute mode** — Up/Down first adjust the **hour**, then press Select once to lock it in and move to adjusting the **minute**. Press Select again to confirm and save.

Use **Back** while in hour/minute mode to step back one stage instead of leaving the screen — handy if you overshoot.

Holding **Select** on the calendar screen also jumps you straight into the correct picker: Punch Out if you're already punched in, otherwise Punch In.

---

## New: Snap Settings

Instead of a single "round to 15 minutes" toggle, Snap now has independent settings for Punch In and Punch Out:

| Setting | Description |
|---|---|
| Snap In | When enabled, opening Punch In (with no time already set for today) pre-fills the current time, rounded |
| Snap In Interval | Rounds to the nearest 1, 5, or 15 minutes |
| Snap Out | Same idea, for Punch Out |
| Snap Out Interval | Rounds to the nearest 1, 5, or 15 minutes |

Your chosen interval also becomes the **normal step size** for Up/Down on that picker — so a 5-minute Snap Out interval means Punch Out now moves in 5-minute steps by default, not the old fixed 15.

Snap only ever applies to **today**, and only when there's no time already saved for that punch — editing a past day or a time you've already set always starts from the saved value or your default.

Found under **Settings → Snap Settings** on the watch, and in the **Punch Times** section on the phone.

---

## New: Pay Modifiers (Deductions & Tax)

Found under **Settings → Pay Modifiers** on the watch, and in its own **Pay Modifiers** section on the phone.

### Periodic Deduction

A fixed dollar amount — for things like health insurance — subtracted from your pay totals.

| Setting | Description |
|---|---|
| Deduction Amount | Dollar amount per period, e.g. $45.00 |
| Deduction Frequency | Weekly or Monthly |

- **Weekly** deductions are subtracted once from the Week total, and scaled automatically for Month/Year based on how many weeks actually fall in that window.
- **Monthly** deductions are subtracted once from the Month total, and ×12 for the Year total.
- Deductions never apply to "Today," since a single day isn't a full pay period.
- Set the amount to **$0.00** to turn this off.

### Tax Settings

Estimates take-home pay by withholding a percentage from your gross earnings. Two modes:

**Flat mode** (default) — one percentage applied to all your pay, on every view including Today. Simplest option, and the right choice if you're outside the US or your country/region uses a flat tax rate.

**Tiered mode** — a 3-bracket progressive system, similar to how many countries (including the US) structure income tax: your rate increases as your weekly gross pay increases, so a heavy-overtime week gets taxed at a higher rate than a normal week.

| Setting | Description |
|---|---|
| Tax Mode | Flat or Tiered |
| Tier 1 Rate | In Flat mode, this is your *only* rate. In Tiered mode, it's your base rate up to Tier 1 Max |
| Tier 1 Max/Wk | Weekly gross pay ceiling for Tier 1 (Tiered mode only) |
| Tier 2 Rate | Rate applied between Tier 1 Max and Tier 2 Max (Tiered mode only) |
| Tier 2 Max/Wk | Weekly gross pay ceiling for Tier 2 (Tiered mode only) |
| Tier 3 Rate | Rate applied to anything above Tier 2 Max (Tiered mode only) |

**How to set your rates:** rather than trying to reconstruct official tax brackets, look at 2-3 of your actual pay stubs — a light week, a normal week, and a heavy-overtime week — and use the withholding percentage you actually see on each as your tier rates. This automatically blends in federal, state, local, and FICA withholding without needing separate settings for each, since whatever you build the app already knows about your own paycheck.

Tiered mode calculates tax week-by-week even when you're viewing Month or Year, so a big month's total pay doesn't get incorrectly pushed into your top bracket — each individual week is taxed at its own rate and the results are summed.

Once either Tier 1 Rate (or Flat Rate) or a Deduction Amount is set above zero, the Pay Dashboard's "TOTAL" row becomes **NET**, reflecting your estimated take-home pay. Leave everything at 0 (the default) and nothing changes from how Punchy always worked.

### Apply Current Wage to Past Year

Found in the phone app's **Pay** section, right under Hourly Rate: a toggle called **"Apply This Rate to Past Year."**

Normally, each punch entry keeps the pay rate that was active when you made it — so a raise doesn't retroactively change old entries. If you'd rather have your current rate applied everywhere, toggle this on and tap Save. The watch will overwrite the stored rate on every existing punch entry (up to 365 days back) with whatever your current Hourly Rate is set to.

**This cannot be undone**, so use it deliberately. You do **not** need this for tax or deduction changes — those are calculated fresh from live settings every time you look at the dashboard, so they already apply to all your history automatically.

---

## New: App Timeout

Found under **Settings → App Timeout**, and on the phone.

Automatically returns to the watch face after a chosen period of inactivity (Off, or 1-30 minutes). Any button press resets the countdown. Useful if you tend to leave the app open.

---

## Phone Settings

Tap the gear icon next to Punchy in the Rebble app to open settings. Changes sync to your watch when you tap **Save to Watch**.

### Pay
| Setting | Description |
|---|---|
| Hourly Rate | Your base pay in cents (1950 = $19.50/hr) |
| Apply This Rate to Past Year | One-way overwrite of stored pay rate on all past entries — see above |
| OT Multiplier | Overtime pay rate (150 = 1.50x, time and a half) |
| OT Starts After | Hours worked before overtime kicks in |
| OT Calculation Mode | Daily (OT each day past threshold), Weekly (OT only after 40 hrs/week), or Both |

### Punch Times
| Setting | Description |
|---|---|
| Default Punch In / Out | Pre-fills the picker — type as "8:00 AM" or "4:30 PM" |
| Snap In / Snap In Interval | See Snap Settings above |
| Snap Out / Snap Out Interval | See Snap Settings above |
| Default Break Time | Break minutes automatically deducted from total hours, in 5-minute steps (0-120) |
| App Timeout | See above |

### Pay Modifiers
| Setting | Description |
|---|---|
| Periodic Deduction / Frequency | See above |
| Tax Mode, Tier Rates & Thresholds | See above |

### Colors
| Setting | Description |
|---|---|
| Background | Black or white watch background |
| Text Color | Main text color |
| Label Color | Secondary/label text color |

### Data
| Setting | Description |
|---|---|
| Export Punch Data | Sends your punch history and notes to Google Sheets |
| Restore from Google Sheets | Reads your punch history and notes back from your sheet and sends it to the watch |
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
  var data  = JSON.parse(e.postData.contents);
  var rows  = data.rows;
  var notes = data.notes || [];

  // Ensure header in row 1
  if (sheet.getLastRow() === 0) {
    sheet.appendRow(['Date', 'Day', 'Punch In', 'Punch Out', 'Hours', 'Pay', 'Rate', 'Exported', 'Notes']);
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
    var note  = notes[i] || '';
    sheet.appendRow([
      month + ' ' + date,
      day,
      inOut[0] || '',
      inOut[1] || '',
      hours,
      pay,
      rate,
      new Date().toLocaleString(),
      note
    ]);
  }

  return ContentService
    .createTextOutput('OK')
    .setMimeType(ContentService.MimeType.TEXT);
}

function doGet(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var last  = sheet.getLastRow();

  if (last <= 1) {
    return ContentService
      .createTextOutput(JSON.stringify({ rows: [] }))
      .setMimeType(ContentService.MimeType.TEXT);
  }

  // Read 9 columns: Date, Day, In, Out, Hours, Pay, Rate, Exported, Notes
  var values = sheet.getRange(2, 1, last - 1, 9).getDisplayValues();

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
6. Copy the **Web app URL** — it looks like: `https://script.google.com/macros/s/ABC123.../exec`

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

The restore reads punch times exactly as they appear in your sheet, so times are always correct regardless of timezone.

---

## Tips

- Up to 365 days of punch history are stored on the watch at any time
- Pay rates are stored per entry — changing your rate later won't affect old punches, unless you use **Apply This Rate to Past Year**
- Tax and deduction settings apply live to *all* history automatically — no need to reapply anything after changing them
- Overtime is split automatically in the Pay Dashboard based on your OT Calculation Mode and threshold
- Snap In / Snap Out are useful if you punch at roughly the same time each day — they pre-select the nearest interval so you rarely need to scroll
- Hold Select on the calendar to jump straight into the picker you need next
- Hold Select on any time picker for precise hour/minute control
- Voice notes are saved per day and visible on the calendar screen

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

**My Pay Dashboard still says "TOTAL," not "NET"**
That's expected until you set a nonzero Tax rate or Deduction amount — the feature is off by default and won't change anything unless you configure it.

---

Punchy is a personal project. Your data stays on your watch and your own Google account — it is never sent to any third-party server.
