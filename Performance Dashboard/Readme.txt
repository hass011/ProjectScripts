# SQL Sentry Live Feed Dashboard

A comprehensive real-time monitoring dashboard for SQL Sentry that provides live alerts, blocking sessions, performance metrics, backup monitoring, and integration with PagerDuty, PRTG, and Rubrik.

## 🌟 Features

- **Real-Time Alert Monitoring**: Server-Sent Events (SSE) stream for live SQL Sentry alerts
- **Blocking Sessions Tracking**: Monitor blocking chains and long-running queries
- **Performance Metrics**: CPU, memory, disk I/O, and wait statistics
- **Backup Monitoring**: Track backup status across all servers
- **DBCC Restore Testing**: Monitor automated DBCC CHECKDB validation
- **Multi-Server Support**: Query multiple SQL Sentry instances simultaneously
- **External Integrations**:
  - **PagerDuty**: On-call schedules and incident management
  - **PRTG**: Network monitoring sensor data
  - **Rubrik**: Backup compliance and SLA monitoring

## 📋 Prerequisites

- **Python 3.7+** (tested with Python 3.8+)
- **SQL Server ODBC Driver 17** or later
- **Network Access** to:
  - SQL Sentry database servers
  - PagerDuty API (if using integration)
  - PRTG server (if using integration)
  - Rubrik clusters (if using integration)

## ⚡ Quick Start (Simplest Setup)

**For personal use, follow these 3 steps:**

1. **Install dependencies:**
   ```bash
   pip install flask flask-cors pyodbc requests urllib3
   ```

2. **Edit configuration** in `app_livefeed.py` (lines 38-42):
   ```python
   SQL_SERVER = os.getenv('SQL_SERVER', 'your-server-name')
   SQL_DATABASE = os.getenv('SQL_DATABASE', 'SentryOne')
   SQL_USERNAME = os.getenv('SQL_USERNAME', 'your-username')
   SQL_PASSWORD = os.getenv('SQL_PASSWORD', 'your-password')
   ```

3. **Run the dashboard:**
   ```bash
   cd C:\Users\hassan.hassan\CascadeProjects\sql-sentry-mcp\dashboard
   python app_livefeed.py
   ```

4. **Open in browser:** `http://localhost:5001/`

**That's it!** No need for `.env` files or environment variables unless you need them.

---

## 🚀 Installation

### 1. Clone or Download the Project

```bash
cd C:\Users\hassan.hassan\CascadeProjects\sql-sentry-mcp\dashboard
```

### 2. Install Python Dependencies

```bash
pip install flask flask-cors pyodbc requests urllib3
```

Or create a `requirements.txt` file:

```text
flask>=2.3.0
flask-cors>=4.0.0
pyodbc>=4.0.39
requests>=2.31.0
urllib3>=2.0.0
```

Then install:

```bash
pip install -r requirements.txt
```

### 3. Install SQL Server ODBC Driver

If not already installed, download and install:
- **Windows**: [Microsoft ODBC Driver 17 for SQL Server](https://docs.microsoft.com/en-us/sql/connect/odbc/download-odbc-driver-for-sql-server)
- **Linux**: Follow [Microsoft's Linux installation guide](https://docs.microsoft.com/en-us/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server)

## ⚙️ Configuration

### Option 1: Direct Code Modification (Simplest - Recommended for Single User)

**If you're the only user and not using version control (Git), this is the easiest approach.**

Open `app_livefeed.py` and modify lines 38-55:

```python
# Configuration
SQL_SERVER = os.getenv('SQL_SERVER', 'your-server-name')      # Change this
SQL_DATABASE = os.getenv('SQL_DATABASE', 'SentryOne')          # Change if needed
SQL_USERNAME = os.getenv('SQL_USERNAME', 'your-username')      # Change this
SQL_PASSWORD = os.getenv('SQL_PASSWORD', 'your-password')      # Change this

# PagerDuty Configuration (Optional)
PD_API_KEY = os.getenv('PAGERDUTY_USER_API_KEY', 'REPLACE_ME')  # Change if using PagerDuty
PD_API_HOST = os.getenv('PAGERDUTY_API_HOST', '')

# PRTG Configuration (Optional)
PRTG_SERVER   = os.getenv('PRTG_SERVER', 'https://your-prtg-server')
PRTG_USERNAME = os.getenv('PRTG_USERNAME', 'your-prtg-username')
PRTG_PASSHASH = os.getenv('PRTG_PASSHASH', 'your-prtg-passhash')

# Rubrik Configuration (Optional)
RUBRIK_SERVERS       = os.getenv('RUBRIK_SERVERS', '').split(',')
RUBRIK_CLIENT_ID     = os.getenv('RUBRIK_CLIENT_ID', 'your-client-id')
RUBRIK_CLIENT_SECRET = os.getenv('RUBRIK_CLIENT_SECRET', 'your-client-secret')
```

**That's it!** Just save the file and run:
```bash
python app_livefeed.py
```

### Option 2: Environment File (.env) - Recommended for Teams/Production

**Use this approach if:**
- You're using Git/version control (keeps passwords out of repository)
- Multiple people will use the dashboard with different credentials
- You need different settings for dev/test/prod environments

Create a `.env` file in the dashboard directory:

```bash
cd C:\Users\hassan.hassan\CascadeProjects\sql-sentry-mcp\dashboard
```

Create `.env` file with the following content:

```env
# SQL Sentry Database Configuration
SQL_SERVER=elkcrpsql02
SQL_DATABASE=SentryOne
SQL_USERNAME=Windsirf
SQL_PASSWORD=Cday321

# PagerDuty Integration (Optional)
PAGERDUTY_USER_API_KEY=your_pagerduty_api_key_here
PAGERDUTY_API_HOST=https://api.pagerduty.com

# PRTG Integration (Optional)
PRTG_SERVER=https://prtg.corp.payroc.com
PRTG_USERNAME=svc-prtg-api-dbas
PRTG_PASSHASH=2547364615

# Rubrik Integration (Optional)
RUBRIK_SERVERS=10.40.12.155,10.44.12.155
RUBRIK_CLIENT_ID=client|019b273e-0554-7d73-922c-ce924876bbf8
RUBRIK_CLIENT_SECRET=your_rubrik_client_secret_here
```

**Important:** Add `.env` to your `.gitignore` file:
```bash
echo .env >> .gitignore
```

### Option 3: Windows Environment Variables

**Use this for system-wide configuration (all users on the machine).**

**Using Command Prompt:**

```cmd
set SQL_SERVER=elkcrpsql02
set SQL_DATABASE=SentryOne
set SQL_USERNAME=Windsirf
set SQL_PASSWORD=Cday321
set PAGERDUTY_USER_API_KEY=your_api_key
```

**Using PowerShell:**

```powershell
$env:SQL_SERVER="elkcrpsql02"
$env:SQL_DATABASE="SentryOne"
$env:SQL_USERNAME="Windsirf"
$env:SQL_PASSWORD="Cday321"
$env:PAGERDUTY_USER_API_KEY="your_api_key"
```

### Configuration Priority

The application reads configuration in this order (highest priority first):
1. **System environment variables** (if set)
2. **`.env` file** (if exists)
3. **Default values in code** (the fallback values in `app_livefeed.py`)

### Configuration Parameters

#### Required Settings

| Variable | Description | Default | Example |
|----------|-------------|---------|---------|
| `SQL_SERVER` | SQL Sentry database server | `elkcrpsql02` | `your-sql-server.domain.com` |
| `SQL_DATABASE` | SQL Sentry database name | `SentryOne` | `SentryOne` |
| `SQL_USERNAME` | Database username | `Windsirf` | `sql_sentry_reader` |
| `SQL_PASSWORD` | Database password | *(required)* | `YourSecurePassword123` |

#### Optional Integrations

**PagerDuty:**
- `PAGERDUTY_USER_API_KEY`: Your PagerDuty user API token (leave as `REPLACE_ME` for mock mode)
- `PAGERDUTY_API_HOST`: PagerDuty API endpoint (default: `https://api.pagerduty.com`)

**PRTG:**
- `PRTG_SERVER`: PRTG server URL
- `PRTG_USERNAME`: PRTG API username
- `PRTG_PASSHASH`: PRTG passhash for authentication

**Rubrik:**
- `RUBRIK_SERVERS`: Comma-separated list of Rubrik cluster IPs
- `RUBRIK_CLIENT_ID`: Rubrik service account client ID
- `RUBRIK_CLIENT_SECRET`: Rubrik service account secret

## 🎯 Usage

### Starting the Dashboard

1. **Navigate to the dashboard directory:**

```bash
cd C:\Users\hassan.hassan\CascadeProjects\sql-sentry-mcp\dashboard
```

2. **Run the application:**

```bash
python app_livefeed.py
```

3. **Expected Output:**

```
================================================================================
SQL Sentry Live Feed Dashboard
================================================================================
SQL Sentry: elkcrpsql02/SentryOne
PagerDuty: Configured (or MOCK MODE if not configured)
================================================================================
 * Serving Flask app 'app_livefeed'
 * Debug mode: on
 * Running on http://0.0.0.0:5001
```

4. **Access the dashboard:**

Open your browser and navigate to:
- **Main Dashboard**: `http://localhost:5001/`
- **Blocking Feed**: `http://localhost:5001/blocking`

### Available Pages

#### 1. Live Feed Dashboard (`/`)
- Real-time alert stream
- Blocking sessions monitor
- Performance metrics overview
- Backup status summary

#### 2. Blocking Feed (`/blocking`)
- Dedicated blocking chain visualization
- Long-running query tracking
- Kill session functionality
- Historical blocking data

## 📊 API Endpoints

The dashboard exposes several REST API endpoints:

### Alert & Event Endpoints

```
GET /api/feed/stream
Server-Sent Events stream for real-time alerts and blocking

GET /api/alerts/recent?hours=24
Get recent alerts from the last N hours

GET /api/alerts/stats?days=7
Get alert statistics summary

GET /api/alerts/timeline?days=30
Get alert timeline data for charting

GET /api/alerts/top-objects?days=7
Get objects with most alerts

GET /api/alerts/history?page=1&limit=50&server=&severity=&days=7
Paginated alert history with filters
```

### Blocking & Performance Endpoints

```
GET /api/blocking/current
Get current blocking sessions

GET /api/blocking/history?hours=24
Get historical blocking data

GET /api/blocking/kill?spid=123&server=ServerName
Kill a blocking session (use with caution!)

GET /api/performance/summary?hours=24&server=all
Get performance metrics summary

GET /api/performance/timeseries?hours=24&server=all&metric=cpu
Get time-series performance data
```

### Backup Monitoring Endpoints

```
GET /api/backups/summary?days=7&server=all
Get backup summary statistics

GET /api/backups/databases?days=7&server=all
Get per-database backup status

GET /api/backups/history?days=7&server=all&db=&type=
Get backup history with filters

GET /api/backups/failures?days=7&server=all
Get recent backup failures
```

### DBCC Restore Testing Endpoints

```
GET /dbcc
DBCC dashboard UI

GET /dbcc/api/summary?days=30&server=all
Get DBCC test summary statistics

GET /dbcc/api/databases?days=30&server=all
Get per-database DBCC test results

GET /dbcc/api/history?days=30&server=all&page=1&limit=50
Get DBCC test history with pagination

GET /dbcc/api/distinct_databases?days=90&server=all
Get list of databases being tested
```

### External Integration Endpoints

```
GET /api/pagerduty/oncall
Get current on-call schedule from PagerDuty

GET /api/prtg/sensors?server=
Get PRTG sensor data for a server

GET /api/rubrik/summary
Get Rubrik backup compliance summary

GET /api/rubrik/hosts
Get Rubrik protected hosts list
```

### Server Management Endpoints

```
GET /api/servers
Get list of monitored SQL Sentry servers

GET /api/servers/health?server=all
Get health status of all servers

GET /api/servers/test-connection?server=ServerName
Test connection to a specific server
```

## 🔧 Advanced Configuration

### Multi-Server Setup

The application can query multiple SQL Sentry databases simultaneously. Servers are configured in the code:

```python
# In app_livefeed.py, modify the server list:
DBCC_SERVERS = {
    'Primary': {'server': 'sql01', 'database': 'SentryOne'},
    'DR': {'server': 'sql02', 'database': 'SentryOne'},
    'Development': {'server': 'sqldev01', 'database': 'SentryOne'}
}
```

### Adjusting Update Intervals

The live feed uses Server-Sent Events with a default 5-second update interval:

```python
# In app_livefeed.py, modify the sleep time:
time.sleep(5)  # Change to your desired interval (seconds)
```

### Template Customization

HTML templates are located in the `templates/` directory:
- `templates/livefeed.html` - Main dashboard
- `templates/blocking_feed.html` - Blocking sessions page

CSS and JavaScript are embedded in the templates for easy customization.

## 🐛 Troubleshooting

### Connection Issues

**Problem**: `pyodbc.OperationalError: Unable to connect to SQL Server`

**Solutions**:
```bash
# Verify SQL Server is accessible
ping elkcrpsql02

# Test with SQL Server Management Studio (SSMS)
# Server: elkcrpsql02
# Database: SentryOne
# Authentication: SQL Server Authentication

# Check ODBC driver installation
odbcinst -j   # Linux
# or check: Control Panel > Administrative Tools > ODBC Data Sources (Windows)

# Verify credentials
# - If using .env: notepad .env
# - If modified code: Check lines 38-42 in app_livefeed.py
```

**Problem**: `[Microsoft][ODBC Driver 17 for SQL Server][SQL Server]Login failed`

**Solutions**:
- **If you modified the code directly:** Check lines 38-42 in `app_livefeed.py` for correct username/password
- **If using .env file:** Verify username and password in `.env` file
- Ensure SQL Server authentication is enabled
- Check if user has read permissions on SentryOne database
- Try connecting with SQL Server Management Studio (SSMS) to verify credentials

### PagerDuty Integration Issues

**Problem**: PagerDuty showing "MOCK MODE"

**Solution**:
```env
# Set your actual API key in .env:
PAGERDUTY_USER_API_KEY=u+aBcDeFgHiJkLmNoPqRsTuV
```

Get your API key from: https://support.pagerduty.com/docs/generating-api-keys

### Port Already in Use

**Problem**: `OSError: [Errno 98] Address already in use`

**Solutions**:
```bash
# Option 1: Kill existing process (Windows)
netstat -ano | findstr :5001
taskkill /PID <PID> /F

# Option 2: Change port in app_livefeed.py
# Line 3719: app.run(host='0.0.0.0', port=5002, debug=True)
```

### Template Not Found

**Problem**: `jinja2.exceptions.TemplateNotFound: livefeed.html`

**Solution**:
```bash
# Ensure templates directory exists:
cd C:\Users\hassan.hassan\CascadeProjects\sql-sentry-mcp\dashboard
dir templates

# Templates should be in: dashboard/templates/
# - livefeed.html
# - blocking_feed.html
```

### SSL Certificate Warnings (Rubrik Integration)

**Expected Behavior**: Rubrik uses self-signed certificates, warnings are suppressed:
```python
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
```

This is normal for internal Rubrik clusters with self-signed certificates.

## 🔒 Security Best Practices

### For Personal/Single-User Setups

If you're the only user and modified the code directly:
- ✅ This is fine for local/personal use
- ⚠️ **Do NOT commit the file to Git** if it contains real passwords
- ⚠️ **Do NOT share the file** with others without removing passwords first

### For Team/Production Deployments

### 1. Use .env Files and Protect Them

```bash
# Add to .gitignore
echo .env >> .gitignore

# Set proper file permissions (Linux)
chmod 600 .env

# On Windows, use file properties to restrict access
```

### 2. Use Read-Only Database Accounts

Grant only SELECT permissions to the SQL Sentry user:

```sql
USE SentryOne;
CREATE LOGIN sql_sentry_reader WITH PASSWORD = 'SecurePassword123!';
CREATE USER sql_sentry_reader FOR LOGIN sql_sentry_reader;
ALTER ROLE db_datareader ADD MEMBER sql_sentry_reader;
-- Grant execute on specific procedures if needed
```

### 3. Enable HTTPS in Production

For production deployments, use a reverse proxy (IIS, nginx, Apache) with SSL:

```bash
# Example nginx configuration
server {
    listen 443 ssl;
    server_name sentry-dashboard.yourdomain.com;
    
    ssl_certificate /path/to/cert.pem;
    ssl_certificate_key /path/to/key.pem;
    
    location / {
        proxy_pass http://localhost:5001;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

### 4. Implement Authentication

Add authentication middleware for production use:

```python
from flask import session, redirect, url_for
from functools import wraps

def login_required(f):
    @wraps(f)
    def decorated_function(*args, **kwargs):
        if 'user' not in session:
            return redirect(url_for('login'))
        return f(*args, **kwargs)
    return decorated_function
```

## 📦 Running as a Windows Service

To run the dashboard as a Windows service, use `nssm` (Non-Sucking Service Manager):

### 1. Download and Install NSSM

```bash
# Download from: https://nssm.cc/download
# Extract to: C:\nssm
```

### 2. Install Service

```cmd
cd C:\nssm\win64
nssm install SQLSentryDashboard "C:\Python39\python.exe" "C:\Users\hassan.hassan\CascadeProjects\sql-sentry-mcp\dashboard\app_livefeed.py"
nssm set SQLSentryDashboard AppDirectory "C:\Users\hassan.hassan\CascadeProjects\sql-sentry-mcp\dashboard"
nssm set SQLSentryDashboard DisplayName "SQL Sentry Live Feed Dashboard"
nssm set SQLSentryDashboard Description "Real-time monitoring dashboard for SQL Sentry"
nssm set SQLSentryDashboard Start SERVICE_AUTO_START
```

### 3. Start Service

```cmd
nssm start SQLSentryDashboard

# Or use Services.msc
services.msc
```

## 🔄 Updating the Application

```bash
# 1. Stop the application (if running as service)
nssm stop SQLSentryDashboard

# 2. Pull latest changes
cd C:\Users\hassan.hassan\CascadeProjects\sql-sentry-mcp\dashboard
git pull

# 3. Update dependencies (if needed)
pip install -r requirements.txt --upgrade

# 4. Restart the application
nssm start SQLSentryDashboard
```

## 📈 Monitoring & Logs

### Application Logs

The application outputs to stdout/stderr:

```bash
# Run with output redirection
python app_livefeed.py > logs/app.log 2>&1
```

### Performance Monitoring

Monitor the dashboard itself:
- CPU usage: Task Manager → Python process
- Memory usage: Should be ~50-200MB under normal load
- Database connections: Max 10-20 concurrent connections

### Health Check Endpoint

```bash
# Add a health check endpoint (future enhancement)
GET /api/health
Response: {"status": "healthy", "uptime": "5 days", "connections": 12}
```

## 🧪 Testing

### Manual Testing

```bash
# Test database connection
python -c "import pyodbc; conn = pyodbc.connect('DRIVER={ODBC Driver 17 for SQL Server};SERVER=elkcrpsql02;DATABASE=SentryOne;UID=Windsirf;PWD=Cday321'); print('✓ Connected')"

# Test API endpoints
curl http://localhost:5001/api/servers
curl http://localhost:5001/api/alerts/recent?hours=1
curl http://localhost:5001/api/blocking/current
```

### Automated Testing Script

Create `test_dashboard.py`:

```python
import requests
import sys

base_url = "http://localhost:5001"
endpoints = [
    "/api/servers",
    "/api/alerts/recent?hours=1",
    "/api/blocking/current",
    "/api/backups/summary?days=1",
]

print("Testing SQL Sentry Dashboard...")
for endpoint in endpoints:
    try:
        response = requests.get(f"{base_url}{endpoint}", timeout=10)
        status = "✓" if response.status_code == 200 else "✗"
        print(f"{status} {endpoint} - Status: {response.status_code}")
    except Exception as e:
        print(f"✗ {endpoint} - Error: {e}")
        sys.exit(1)

print("\n✓ All tests passed!")
```

## 📚 Additional Resources

- **SQL Sentry Documentation**: https://www.sentryone.com/docs
- **Flask Documentation**: https://flask.palletsprojects.com/
- **pyodbc Documentation**: https://github.com/mkleehammer/pyodbc/wiki
- **PagerDuty API**: https://developer.pagerduty.com/api-reference/
- **Rubrik API**: https://github.com/rubrikinc/rubrik-sdk-for-python

## 🤝 Support

For issues or questions:
- **Internal Support**: Contact hassan.hassan@yourdomain.com
- **SQL Sentry Issues**: Check SQL Sentry database connectivity and permissions
- **Application Issues**: Check application logs and error messages

## 📋 Quick Reference Commands

```bash
# Start the dashboard
cd C:\Users\hassan.hassan\CascadeProjects\sql-sentry-mcp\dashboard
python app_livefeed.py

# Access main dashboard
http://localhost:5001/

# Access blocking feed
http://localhost:5001/blocking

# View recent alerts (API)
http://localhost:5001/api/alerts/recent?hours=24

# View current blocking (API)
http://localhost:5001/api/blocking/current

# View backup summary (API)
http://localhost:5001/api/backups/summary?days=7

# Test database connection
python -c "import pyodbc; pyodbc.connect('DRIVER={ODBC Driver 17 for SQL Server};SERVER=elkcrpsql02;DATABASE=SentryOne;UID=Windsirf;PWD=Cday321')"
```

## 📝 License

[Specify your license here]

---

**Version**: 1.0  
**Last Updated**: 2024  
**Port**: 5001  
**Framework**: Flask 2.x + pyodbc
