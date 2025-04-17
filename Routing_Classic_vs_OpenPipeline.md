
# Dynatrace Routing: Classic vs. OpenPipeline

---

## **Classic Dynatrace (Pre-Grail): All Logs → Default Bucket**

- In the Classic log monitoring, there was **no concept of routing rules**.
- **All logs**—whether from 10 different AWS accounts or 5 clusters—were sent to a **single central bucket** (think of it like a “default inbox”).
- If you wanted separation (e.g., logs for Finance vs. Security), you had to:
  - Tag them manually  
  - Filter with searches in the Logs UI  
  - Or create custom dashboards per team

**Analogy:**  
All mail (logs) was dumped into **one giant mailbox**. Each team had to **dig through it** to find what they cared about.

---

## **OpenPipeline: Smart Routing Before It Hits the Bucket**

Now with **OpenPipeline**, Dynatrace gives you control over **routing before logs are stored**.

You create multiple **pipelines**, each acting like a **separate mailbox**. Routing rules (**static or dynamic**) decide which log goes where.

### **Here’s the Flow:**
1. **Ingest:** A log comes in from AWS, K8s, or a custom app.  
2. **Routing stage:**
   - **Static rule?**  
     Matches fixed AWS account ID → **Finance pipeline**  
   - **Dynamic rule?**  
     Matches log group = `cloudtrail` → **Security pipeline**
3. **Processing in the pipeline**  
4. **Stored in Grail** → now isolated, enriched, secured for that use case.

---

## **Comparison Table**

| **Feature**               | **Classic (Old Way)**             | **OpenPipeline (New Way)**                   |
|---------------------------|-----------------------------------|----------------------------------------------|
| **Log destination**       | Default, single log store         | Routed pipelines with custom destinations    |
| **Separation of concerns**| Manual via tags/filters           | Automatic via routing rules                  |
| **Multi-tenancy**         | Not supported                     | Fully supported via dedicated pipelines      |
| **Metadata-based routing**| Not available                     | Powerful logic-based routing                 |
| **Storage engine**        | Classic store                     | Grail (next-gen log engine)                  |

---

## **Benefit of OpenPipeline Routing Over Classic**
- More efficient
- More secure
- Enables per-team or per-use-case observability
- Reduces noise
- Supports enterprise-grade multi-account strategies
