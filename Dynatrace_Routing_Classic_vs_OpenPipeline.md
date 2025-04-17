
# Dynatrace Log Routing: Classic vs. OpenPipeline

## Classic Dynatrace (Pre-Grail): All Logs → Default Bucket

In the Classic log monitoring model:

- **No routing logic** was available.
- **All logs**—from various AWS accounts, clusters, or applications—were sent to a **single, default storage bucket**.
- Separation had to be done manually using:
  - Log filters
  - Tagging strategies
  - Dashboard-level segmentation

**Analogy**:  
> Like dumping all incoming mail into a single mailbox and expecting every department to search for their own letters.

---

## OpenPipeline: Smart Routing Before Storage

OpenPipeline introduces **routing logic**—both **static** and **dynamic**—to sort logs into dedicated pipelines **before** storing them in Grail.

### Log Flow in OpenPipeline:

1. **Log Ingested** from OneAgent, API, or custom sources.
2. **Routing Stage**:
   - Static Routing: Matches fixed values (e.g., specific AWS account ID).
   - Dynamic Routing: Matches metadata patterns (e.g., log group contains "cloudtrail").
3. **Pipeline Processing** (transformation, enrichment, metric extraction).
4. **Storage in Grail** under designated pipeline.

---

## Static vs. Dynamic Routing

| Feature         | Static Routing                                | Dynamic Routing                                  |
|-----------------|------------------------------------------------|--------------------------------------------------|
| Match Type      | Exact value match                              | Pattern-based, logic-driven                      |
| Example         | `matchesValue(aws.account.id, "123456789012")` | `matchesPhrase(category, "security")` OR logic   |
| Flexibility     | Low                                            | High                                             |
| Use Case        | Known accounts or systems                      | Multiple clusters, categories, environments      |
| Common Pattern  | Catch-all using `true`                         | Conditional branching with multiple criteria     |

---

## Example Log Routing Table

| Log ID | aws.account.id | log.source   | category   | Routed Pipeline     | Routing Type |
|--------|----------------|--------------|------------|---------------------|--------------|
| Log A  | 123456789012    | system-logs  | General    | Finance             | Static       |
| Log B  | 998877665544    | auth-logs    | Security   | Security            | Dynamic      |
| Log C  | 444455556666    | app-logs     | Application| Classic (Catch-all) | Default      |

---

## Comparison Summary

| Feature                   | Classic (Old Way)             | OpenPipeline (New Way)                   |
|---------------------------|-------------------------------|-------------------------------------------|
| Log destination           | Default, single log store     | Routed pipelines with custom destinations |
| Separation of concerns    | Manual via tags/filters       | Automatic via routing rules               |
| Multi-tenancy             | Not supported                 | Fully supported via dedicated pipelines   |
| Metadata-based routing    | Not available                 | Powerful logic-based routing              |
| Storage engine            | Classic store                 | Grail (next-gen log engine)               |

---

## Conclusion

With OpenPipeline, Dynatrace offers **powerful and precise control** over how logs are routed, processed, and stored—unlike the older Classic model, which treated all logs the same. This upgrade enables multi-team observability, better security segregation, and smarter automation.

