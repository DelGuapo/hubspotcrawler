# HubSpot Crawler

A comprehensive Node.js audit tool for analyzing HubSpot workflows, custom properties, and integrations.

> **For Non-Technical Users**: This tool helps you understand what's happening in your HubSpot account. It creates a detailed report showing which workflows are active, which custom fields you're using, and which ones might not be needed anymore. No programming knowledge required to use it!

## What Does This Tool Do?

- **Workflow Analysis**: Shows all your automated workflows with how many contacts/deals/companies are enrolled, how many times they've run, and if there are any errors
- **Custom Properties Audit**: Lists all your custom fields for contacts (customers), deals, and companies (partners)
- **Unused Fields Detection**: Finds fields that haven't been updated recently (you can set how many days to check)
- **NetSuite Integration Scan**: Identifies any connections to NetSuite (if you use it)
- **Usage Scoring**: Rates your workflows and fields based on how much they're being used
- **Consolidation Recommendations**: Suggests which workflows or fields you might want to combine or remove
- **JSON Report Output**: Creates a detailed report file you can open and review

## Prerequisites (What You Need)

Before you start, make sure you have:

1. **Node.js installed** (version 18 or higher)
   - Don't have it? Download from [nodejs.org](https://nodejs.org/)
   - To check if you have it: Open a terminal/command prompt and type `node --version`

2. **A HubSpot account** with access to create Private Apps
   - You'll need admin permissions to create an API key

3. **Basic familiarity with the command line/terminal**
   - Don't worry! We'll walk you through each step

## Step-by-Step Installation

### Step 1: Download the Tool

Open your terminal (Mac/Linux) or Command Prompt (Windows) and run:

```bash
git clone https://github.com/DelGuapo/hubspotcrawler.git
cd hubspotcrawler
```

**Don't have git?** You can download the code:
1. Go to https://github.com/DelGuapo/hubspotcrawler
2. Click the green "Code" button
3. Click "Download ZIP"
4. Extract the ZIP file
5. Open terminal/command prompt and navigate to the extracted folder

### Step 2: Install Dependencies

In the terminal, while in the hubspotcrawler folder, run:

```bash
npm install
```

This downloads the necessary libraries. Wait for it to complete (it might take a minute).

### Step 3: Get Your HubSpot API Key

You need a special key to let this tool access your HubSpot data:

1. **Log in to your HubSpot account**
2. **Click the Settings icon** (gear icon in the top right)
3. **In the left sidebar**, scroll down and click **"Integrations"**
4. **Click "Private Apps"**
5. **Click "Create a private app"** (or use an existing one)
6. **Give it a name** like "Audit Tool"
7. **Click the "Scopes" tab** and enable these permissions:
   - `crm.objects.contacts.read` - Read contacts
   - `crm.objects.deals.read` - Read deals
   - `crm.objects.companies.read` - Read companies
   - `automation` - Read workflows
   - `crm.schemas.custom.read` - Read custom properties
8. **Click "Create app"**
9. **Copy the access token** that appears (it looks like a long string of random characters)

### Step 4: Create Your Configuration File

1. In the hubspotcrawler folder, **make a copy** of `.env.example`:
   - **Mac/Linux**: Run `cp .env.example .env`
   - **Windows**: Run `copy .env.example .env`

2. **Open the `.env` file** with any text editor (Notepad, TextEdit, VS Code, etc.)

3. **Paste your API key** after the equals sign:
   ```
   HUBSPOT_API_KEY=your_actual_api_key_here
   ```

4. **Save the file**

⚠️ **Important**: Never share your `.env` file or API key with anyone!

## How to Use the Tool

### Basic Usage (Recommended for First-Time Users)

Simply run:

```bash
node index.js
```

Or:

```bash
npm start
```

The tool will:
1. Connect to your HubSpot account
2. Analyze all your workflows
3. Check all your custom properties
4. Look for fields that haven't been updated in 90 days
5. Scan for NetSuite integration references
6. Generate recommendations
7. Save everything to a file called `audit-report.json`

You'll see progress messages as it works. When it's done, look for the `audit-report.json` file in the same folder.

### Advanced Usage (Custom Options)

**Get help and see all options:**
```bash
node index.js --help
```

**Check for fields unused in the last 30 days** (great for finding recent changes):
```bash
node index.js --days 30
```

**Check for fields unused in the last 180 days** (long-term trends):
```bash
node index.js --days 180
```

**Save the report with a custom name:**
```bash
node index.js --output my-audit-2024.json
```

**Combine options:**
```bash
node index.js --days 60 --output quarterly-audit.json
```

### Understanding the Command Line Options

- `--days <number>` - How many days back to check for field usage
  - Lower numbers (like 30) show recent activity
  - Higher numbers (like 180) show long-term trends
  - Default is 90 days if you don't specify

- `--output <filename>` - Name your report file
  - Useful if you want to keep multiple reports
  - Default is `audit-report.json`

- `--help` or `-h` - Show help information

## Understanding Your Report

After running the tool, you'll get a file called `audit-report.json`. Here's what's in it:

### Opening the Report

You can open this file with:
- Any text editor (Notepad, TextEdit, VS Code)
- Online JSON viewers (search "JSON viewer online")
- Import it into Excel or Google Sheets

### Report Structure

```json
{
  "timestamp": "2024-10-31T20:00:00.000Z",
  "configuration": {
    "unusedFieldsDays": 90
  },
  "workflows": [...],
  "properties": {
    "contact": [...],
    "deal": [...],
    "company": [...]
  },
  "unusedFields": [...],
  "netsuiteIntegration": {
    "dependencies": [...]
  },
  "recommendations": {
    "workflows": [...],
    "fields": [...]
  }
}
```

### What Each Section Means

- **timestamp**: When the audit was run
- **configuration**: Settings used (like how many days for unused fields)
- **workflows**: All your automated workflows with:
  - How many contacts/deals/companies are enrolled
  - How many times they've run
  - If there are errors
  - A usage score (higher = more actively used)
- **properties**: All your custom fields organized by type
  - Contact properties (for customer information)
  - Deal properties (for sales deals)
  - Company properties (for partner/account information)
- **unusedFields**: Fields that haven't been updated in X days (based on your --days setting)
  - Shows which fields might be safe to archive or remove
- **netsuiteIntegration**: Any workflows or fields related to NetSuite
- **recommendations**: Suggestions for:
  - Consolidating similar workflows
  - Removing or archiving unused fields
  - Cleaning up duplicate properties

## Common Questions (FAQ)

### Q: Do I need to know programming to use this?
**A:** No! Just follow the installation steps and run the basic command. The tool does everything automatically.

### Q: Will this change anything in my HubSpot account?
**A:** No! This tool only reads data. It doesn't modify or delete anything.

### Q: How long does it take to run?
**A:** Usually 1-5 minutes, depending on how much data you have in HubSpot.

### Q: What if I get an error about my API key?
**A:** Double-check that:
1. You copied the entire API key (no spaces)
2. Your `.env` file is saved in the hubspotcrawler folder
3. The file is named exactly `.env` (not `.env.txt`)

### Q: Why would I want to check different day ranges (like 30 vs 180)?
**A:** Different ranges show different insights:
- **30 days**: Recent activity, good for month-over-month comparisons
- **60 days**: Quarterly trends
- **90 days** (default): Good balance for finding truly unused fields
- **180+ days**: Long-term analysis, very conservative

### Q: Can I run this multiple times?
**A:** Yes! Run it as often as you like. Each run creates a new report (or overwrites the previous one unless you use --output with different names).

### Q: What should I do with the recommendations?
**A:** Review them with your team. The tool suggests what *might* be consolidated or removed, but you know your business best. Use the recommendations as a starting point for discussion.

## Troubleshooting

### "Error: HUBSPOT_API_KEY not found"
- **Solution**: Make sure you created the `.env` file and added your API key. The file must be in the same folder as `index.js`.

### "Cannot find module..."
- **Solution**: Run `npm install` again to install all required packages.

### "API Access Limited" or seeing mock data
- **Solution**: Check that your API key has all the required permissions (scopes) listed in Step 3 above.

### "Network Errors"
- **Solution**: 
  - Check your internet connection
  - Make sure your firewall isn't blocking the connection
  - If you're on a corporate network, you might need to contact IT

### The report file is hard to read
- **Solution**: Try these options:
  - Use an online JSON viewer (search "json viewer online")
  - Copy the content into Excel/Google Sheets
  - Use a code editor like VS Code (it formats JSON nicely)

## Getting Help

If you're stuck:
1. Run `node index.js --help` to see usage options
2. Check the Troubleshooting section above
3. Make sure you followed all installation steps
4. Look for error messages - they often tell you exactly what's wrong

## For Developers

### Mock Mode

If the script cannot access the HubSpot API (due to permissions or network issues), it will automatically generate mock data for testing purposes. This allows you to see the report structure without live data.

### Technical Details

The main script is in `index.js` and includes:
- HubSpot API client initialization
- Workflow fetching and analysis
- Property categorization
- Usage detection algorithms
- Scoring system
- Recommendation engine
- Command-line argument parsing

### API Details

The tool uses the official `@hubspot/api-client` SDK and requires these API scopes:
- `crm.objects.contacts.read` - Read contacts
- `crm.objects.deals.read` - Read deals
- `crm.objects.companies.read` - Read companies
- `automation` - Read workflows
- `crm.schemas.custom.read` - Read custom property definitions

## License

ISC

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
