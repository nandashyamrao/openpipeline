
# Splunk to Postgres Webhook Batch Script

This document explains and contains the Python script that automates:
1. Dispatching a saved Splunk search
2. Waiting for search job completion
3. Fetching results in JSON
4. Sending them in batches to a custom webhook for Postgres insertion

---

## 🔐 Configuration

```python
SPLUNK_API_BASE = "https://splunk.example.com:8089"
SPLUNK_USERNAME = "admin"
SPLUNK_PASSWORD = "your_splunk_password"
SAVED_SEARCH_NAME = "Login Failure Summary"

WEBHOOK_URL = "https://internal-api.insurecorp.com/postgres-webhook"
WEBHOOK_AUTH_TOKEN = "YOUR_INTERNAL_TOKEN"
BATCH_SIZE = 50
```

---

## 🚀 Python Script

```python
import time
import requests
import json

SPLUNK_API_BASE = "https://splunk.example.com:8089"
SPLUNK_USERNAME = "admin"
SPLUNK_PASSWORD = "your_splunk_password"
SAVED_SEARCH_NAME = "Login Failure Summary"

WEBHOOK_URL = "https://internal-api.insurecorp.com/postgres-webhook"
WEBHOOK_AUTH_TOKEN = "YOUR_INTERNAL_TOKEN"
BATCH_SIZE = 50

auth = (SPLUNK_USERNAME, SPLUNK_PASSWORD)
headers = {
    "Content-Type": "application/x-www-form-urlencoded"
}

# Step 1: Dispatch saved search
trigger_url = f"{SPLUNK_API_BASE}/servicesNS/admin/search/saved/searches/{SAVED_SEARCH_NAME}/dispatch"
trigger_response = requests.post(trigger_url, auth=auth, headers=headers, verify=False)
trigger_response.raise_for_status()
sid = trigger_response.json()["sid"]

# Step 2: Wait for search job to finish
results_ready = False
while not results_ready:
    status_url = f"{SPLUNK_API_BASE}/services/search/jobs/{sid}"
    status_response = requests.get(status_url, auth=auth, verify=False)
    status_response.raise_for_status()
    content = status_response.text
    if "<s:key name=\"isDone\">1</s:key>" in content:
        results_ready = True
    else:
        time.sleep(3)

# Step 3: Fetch results
results_url = f"{SPLUNK_API_BASE}/services/search/jobs/{sid}/results?output_mode=json"
results_response = requests.get(results_url, auth=auth, verify=False)
results_response.raise_for_status()
results = results_response.json()["results"]

# Step 4: Send in batches to webhook
headers = {
    "Content-Type": "application/json",
    "Authorization": f"Bearer {WEBHOOK_AUTH_TOKEN}"
}

responses = []
for i in range(0, len(results), BATCH_SIZE):
    batch = results[i:i + BATCH_SIZE]
    payload = {
        "event_type": "batch_login_failures",
        "records": batch,
        "alert_time": time.strftime("%Y-%m-%dT%H:%M:%SZ", time.gmtime()),
        "source": "splunk-nightly-batch"
    }
    response = requests.post(WEBHOOK_URL, headers=headers, json=payload)
    responses.append((response.status_code, response.text))
```

---

## ✅ Notes

- Adjust `BATCH_SIZE` depending on payload limits
- Token-based auth is used in the webhook header
- For large data volumes, consider Splunk export or forwarder + ETL strategy
