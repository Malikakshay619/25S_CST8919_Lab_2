# Flask Login Monitor App with Azure Log Analytics

## Overview
This project implements a Flask-based login monitoring web application, deployed to Azure App Service using Local Git. It integrates with Azure Log Analytics to collect and analyze login attempts.

## Deployment Steps

1. **Azure Resources Setup**
   - Created a resource group `myRG` in Canada Central.
   - Provisioned an App Service Plan `myPlan` using `B1` SKU.
   - Created the Web App `flask-login-akshay1025` with Python 3.10 runtime.
   - Enabled Local Git deployment and configured `gunicorn app:app` as the startup command.

2. **Application Deployment**
   - Cloned the GitHub repository.
   - Updated and pushed code to Azure using Git.
   - Verified deployment via `curl` POST request:
     ```bash
     curl -X POST https://flask-login-akshay1025.azurewebsites.net/login \
       -H "Content-Type: application/json" \
       -d '{"username":"admin", "password":"password123"}'
     ```

3. **Log Analytics Integration**
   - Created a Log Analytics workspace:  
     `DefaultWorkspace-7fac140a-6f9a-437a-80a4-2f4537b0274c-CCAN`
   - Connected the web app’s diagnostic logs (`AppServiceHTTPLogs`) to the workspace using:
     ```bash
     az monitor diagnostic-settings create ...
     ```

4. **Log Query Execution**
   - Queried failed login attempts using Azure CLI:
     ```bash
     az monitor log-analytics query \
       --workspace <workspace-id> \
       --analytics-query "AppServiceAppLogs | where Message has 'Failed login attempt' | project TimeGenerated, Message" \
       --output table
     ```

## Outcome
Deployment and integration were successful. Diagnostic settings are active, and logs are queryable via Azure Monitor for audit and analysis purposes.
