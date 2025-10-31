# HubSpot Crawler

A comprehensive Node.js audit tool for analyzing HubSpot workflows, custom properties, and integrations.

## Features

- **Workflow Analysis**: Lists all workflows with enrollment counts, runs, errors, and type (contact/deal/company)
- **Custom Properties Audit**: Fetches and categorizes custom properties for contacts (customers), deals, and companies (partners)
- **Unused Fields Detection**: Identifies fields with no updates in the last 90 days
- **NetSuite Integration Scan**: Detects dependencies and references to NetSuite integration
- **Usage Scoring**: Scores workflows and fields by usage patterns
- **Consolidation Recommendations**: Suggests opportunities to consolidate or archive resources
- **JSON Report Output**: Generates detailed audit reports in JSON format

## Installation

1. Clone the repository:
```bash
git clone https://github.com/DelGuapo/hubspotcrawler.git
cd hubspotcrawler
```

2. Install dependencies:
```bash
npm install
```

3. Configure your HubSpot API key:
```bash
cp .env.example .env
# Edit .env and add your HubSpot API key
```

## Configuration

Create a `.env` file in the root directory with your HubSpot API key:

```
HUBSPOT_API_KEY=your_hubspot_api_key_here
```

To get your HubSpot API key:
1. Log in to your HubSpot account
2. Go to Settings > Integrations > Private Apps
3. Create a new private app or use an existing one
4. Copy the access token

### Required Scopes

Your HubSpot API key needs the following scopes:
- `crm.objects.contacts.read`
- `crm.objects.deals.read`
- `crm.objects.companies.read`
- `automation`
- `crm.schemas.custom.read`

## Usage

Run the audit:

```bash
npm start
```

Or:

```bash
node index.js
```

The script will:
1. Connect to your HubSpot account
2. Analyze all workflows
3. Fetch custom properties for contacts, deals, and companies
4. Detect unused fields (90+ days without updates)
5. Scan for NetSuite integration dependencies
6. Generate usage scores and recommendations
7. Save a detailed report to `audit-report.json`

## Output

The script generates a JSON report (`audit-report.json`) with the following structure:

```json
{
  "timestamp": "2024-10-31T20:00:00.000Z",
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

### Report Sections

- **workflows**: Complete list of workflows with metadata, enrollment counts, and usage scores
- **properties**: Custom properties categorized by object type (contact/deal/company)
- **unusedFields**: Fields that haven't been updated in 90+ days
- **netsuiteIntegration**: NetSuite-related workflows and properties
- **recommendations**: Actionable suggestions for consolidation and cleanup

## Mock Mode

If the script cannot access the HubSpot API (due to permissions or network issues), it will automatically generate mock data for testing purposes. This allows you to see the report structure without live data.

## Development

The main script is in `index.js` and includes:
- HubSpot API client initialization
- Workflow fetching and analysis
- Property categorization
- Usage detection algorithms
- Scoring system
- Recommendation engine

## Troubleshooting

**Error: HUBSPOT_API_KEY not found**
- Make sure you've created a `.env` file with your API key
- Verify the key is correct and has proper permissions

**API Access Limited**
- The script will fall back to mock data if API access is restricted
- Check your HubSpot plan and API scope permissions

**Network Errors**
- Verify your internet connection
- Check if your firewall allows connections to HubSpot's API

## License

ISC

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
