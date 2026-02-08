# Overview
In this lab, you will create your first Azure Function using Visual Studio Code and deploy it to Azure. This hands-on exercise introduces you to the complete serverless development workflow: from creating a local project to testing.

# Running the Project Locally

## Prerequisites

- Python 3.12
- VS Code (or any IDE)
- Azure Functions Core Tools v4
- Node.js (required for Core Tools)

## 1. Clone the GitHub Repo
git clone https://github.com/mend0214/CST8917-Lab1


## 2. Install dependencies using:

```bash
pip install -r requirements.txt
```

## 3. Configure Environment Variables
```json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "python",
    "AzureWebJobsStorage": "UseDevelopmentStorage=true",
    "DATABASE_CONNECTION_STRING": "<Your-Cosmos-DB-Connection-String>"
  }
}
```
## 4. Run the Project Locally
1. Open the project in VS Code

2. Press F5 (Start Debugging) or run:
```bash
func start
```
3. Verify that the functions are running in the terminal:
```bash
Functions:
  TextAnalyzer: http://localhost:7071/api/TextAnalyzer
  GetAnalysisHistory: [GET] http://localhost:7071/api/GetAnalysisHistory
```

## Test the Functions
### Test the Text Analyzer

**Using curl:**
```bash
curl "http://localhost:7071/api/TextAnalyzer?text=Serverless%20computing%20is%20amazing.%20It%20scales%20automatically."
```

**Using browser:**
```
http://localhost:7071/api/TextAnalyzer?text=Serverless computing is amazing. It scales automatically.
```
**Expected Response:**
```json
{
  "analysis": {
    "wordCount": 7,
    "characterCount": 56,
    "characterCountNoSpaces": 49,
    "sentenceCount": 2,
    "paragraphCount": 1,
    "averageWordLength": 7.0,
    "longestWord": "automatically.",
    "readingTimeMinutes": 0.0
  },
  "metadata": {
    "analyzedAt": "2026-01-28T15:30:00.000000",
    "textPreview": "Serverless computing is amazing. It scales automatically."
  }
}
```
**Verify in Azure Portal that the data was stored in your database**

### Test the History Endpoint
1. Run several text analyses to populate your database
2. Call the history endpoint: ```http://localhost:7071/api/GetAnalysisHistory```
3. Verify you receive the stored results
4. Test the limit parameter: ```http://localhost:7071/api/GetAnalysisHistory?limit=5```

**Example Response:**
```json
{
  "count": 2,
  "results": [
    {
      "id": "abc123-...",
      "analysis": { "wordCount": 7, ... },
      "metadata": { "analyzedAt": "2026-01-28T15:30:00", ... }
    },
    {
      "id": "def456-...",
      "analysis": { "wordCount": 12, ... },
      "metadata": { "analyzedAt": "2026-01-28T14:20:00", ... }
    }
  ]
}
```

## Notes
1. Make sure to install dependencies
2. Create your Cosmos DB, and name your database and container as ```analysis``` and ```results``` respectively.
3. Configure your Cosmos DB database connection string in ```local.settings.json```