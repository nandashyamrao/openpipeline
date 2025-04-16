# ✔️ Status Correction Lambda – Explained

## 🔧 What It Does
Cleans up and standardizes the `status` field in AWS Lambda logs using the `loglevel` field when `status` is missing or incorrect.

---

## 📝 Example Log (raw)

```json
{
  "aws.log_group": "/aws/lambda/...",
  "loglevel": "ERROR",
  "message": "Lambda execution failed due to timeout"
}
```

---

## ⚙️ Rule Logic (DQL Processor)

```dql
// This processor is designed to fix the ERROR status

parse content, 'json:json'
fieldsAdd correctedStatus = json[loglevel]
fieldsAdd status = if(isnotnull(correctedStatus), correctedStatus, else: status)
fieldsAdd status = if(isnotnull(correctedStatus), correctedStatus, else: loglevel)
fieldsRemove correctedStatus
```

---

## 🎯 Matching Condition

```dql
matchesPhrase(aws.log_group, "/aws/lambda/")
```

---

## 📊 Result

For every log record from Lambda:
- If `loglevel` exists, its value will be used as the unified `status`
- If `status` already exists, it is preserved unless `correctedStatus` is valid
- Temporary field `correctedStatus` is removed

---

## ✅ Final Output (cleaned log)

```json
{
  "aws.log_group": "/aws/lambda/...",
  "status": "ERROR",
  "message": "Lambda execution failed due to timeout"
}
```