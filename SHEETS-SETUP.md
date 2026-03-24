# Google Sheets Lead Capture — Setup Guide
## For both AI Readiness and EU AI Compliance assessments

---

## Step 1: Create or open your Google Sheet

Open the workbook you use for AI Readiness leads. Add a new sheet tab
called `EU AI Compliance` with these column headers in row 1:

```
A: Timestamp
B: Source
C: Name
D: Email
E: Company
F: Role
G: Total Score
H: High Risk Domains
I: Pillar Scores
J: Q1 (AI Systems)
K: Q2 (Risk Classification)
L: Q3 (Domains)
M: Q4 (Governance)
N: Q5 (Responsibility)
O: Q6 (Risk Assessment)
P: Q7 (Transparency)
Q: Q8 (Human Override)
R: Q9 (Bias)
S: Q10 (Documentation)
T: Q11 (Data Governance)
U: Q12 (Monitoring)
```

## Step 2: Create the Google Apps Script

1. In your Google Sheet, go to **Extensions → Apps Script**
2. Delete any existing code and paste this:

```javascript
function doPost(e) {
  try {
    var data = JSON.parse(e.postData.contents);
    var ss = SpreadsheetApp.getActiveSpreadsheet();
    
    // Route to correct sheet based on source
    var sheetName = data.source === 'eu-ai-compliance' 
      ? 'EU AI Compliance' 
      : 'AI Readiness';
    
    var sheet = ss.getSheetByName(sheetName);
    if (!sheet) {
      sheet = ss.insertSheet(sheetName);
    }
    
    if (data.source === 'eu-ai-compliance') {
      sheet.appendRow([
        data.timestamp,
        data.source,
        data.name,
        data.email,
        data.company,
        data.role,
        data.totalScore,
        data.hasHighRisk ? 'YES' : 'NO',
        data.pillarScores,
        data.q1, data.q2, data.q3,
        data.q4, data.q5, data.q6,
        data.q7, data.q8, data.q9,
        data.q10, data.q11, data.q12
      ]);
    } else {
      // AI Readiness format — adapt columns to match your existing sheet
      sheet.appendRow([
        data.timestamp,
        data.source,
        data.name,
        data.email,
        data.company,
        data.role,
        data.totalScore,
        JSON.stringify(data.answers || {})
      ]);
    }
    
    // Optional: send email notification
    MailApp.sendEmail({
      to: 'solutions@koios-analytics.com',
      subject: '🎯 New ' + sheetName + ' Lead: ' + data.name + ' (' + data.company + ')',
      body: 'Name: ' + data.name + '\n' +
            'Email: ' + data.email + '\n' +
            'Company: ' + data.company + '\n' +
            'Role: ' + data.role + '\n' +
            'Score: ' + data.totalScore + '/100\n' +
            (data.hasHighRisk ? '⚠️ HIGH RISK DOMAINS DETECTED\n' : '') +
            '\nPillar Scores: ' + (data.pillarScores || 'N/A') + '\n' +
            '\nView full details in the spreadsheet.'
    });
    
    return ContentService.createTextOutput(
      JSON.stringify({ status: 'ok' })
    ).setMimeType(ContentService.MimeType.JSON);
    
  } catch (error) {
    return ContentService.createTextOutput(
      JSON.stringify({ status: 'error', message: error.toString() })
    ).setMimeType(ContentService.MimeType.JSON);
  }
}
```

3. Save (Ctrl+S), name it "Lead Capture"

## Step 3: Deploy as Web App

1. Click **Deploy → New deployment**
2. Type: **Web app**
3. Settings:
   - Description: "Koios lead capture"
   - Execute as: **Me**
   - Who has access: **Anyone**
4. Click **Deploy**
5. Authorize when prompted (review permissions, click Allow)
6. Copy the **Web app URL** — it looks like:
   `https://script.google.com/macros/s/AKfycb.../exec`

## Step 4: Add the URL to your assessment pages

### EU AI Compliance page (eu-ai-compliance.html):
Find this line:
```javascript
const GOOGLE_SCRIPT_URL = 'YOUR_GOOGLE_APPS_SCRIPT_URL';
```
Replace with your actual URL:
```javascript
const GOOGLE_SCRIPT_URL = 'https://script.google.com/macros/s/AKfycb.../exec';
```

### AI Readiness page (ai-readiness-assessment.html):
Add the same lead capture pattern to the AI Readiness page's form
submission function. The `source` field will be `'ai-readiness'` so
the script routes it to the correct sheet tab.

## Step 5: Test

1. Fill out the EU AI compliance assessment
2. Submit with test details
3. Check your Google Sheet — a new row should appear
4. Check your email for the notification

## How It Works

```
User fills assessment → Clicks "Get My Report"
  → Results displayed immediately (no delay)
  → In background: POST to Google Apps Script
    → Script writes to Google Sheet
    → Script sends email notification
    → User never sees any of this
```

The `mode: 'no-cors'` in the fetch means the request is fire-and-forget.
If it fails (network issue, script error), the user still sees their
results — the lead capture is a silent side-effect, not a blocker.

## Reusing for AI Readiness

The same Google Apps Script handles both assessments — it routes based
on the `source` field. Just add the same fetch() pattern to your
AI Readiness page's submit handler with `source: 'ai-readiness'`.
