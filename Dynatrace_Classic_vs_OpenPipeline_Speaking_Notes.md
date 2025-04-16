
# Dynatrace Pipeline Presentation Notes

## Introduction

Before we dive into how Dynatrace OpenPipeline works today, let’s take a quick step back. It’s important to understand how things worked until recently – through what we call the **Classic Pipeline**. This gives us context on why OpenPipeline was built and what problems it solves.

---

## Slide 1: Dynatrace Classic Pipeline

### Speaking Notes

This slide shows how logs and metrics were handled in the classic Dynatrace pipeline.

- First, **all logs were ingested into a single, default bucket** — there was no dynamic routing or fine-grained control at the point of ingestion.
- Once inside, logs would go through processing rules — what we now call **Logs Classic**. These rules parsed and extracted data but were not as flexible as what we can do now.
- From these logs, metrics could be extracted using predefined mechanisms — known as **Metrics Classic**. These typically had static prefixes like `builtin`, `ext`, or `calc`, and you didn’t have as much control over granularity or naming.
- Finally, events could be generated based on certain patterns or thresholds detected in the logs.

While this worked well, it had limitations: there was a lack of routing flexibility, limited transformation capability, and no real visibility into how your data moved through the pipeline.

### Transition Line to Next Slide

To address these limitations, Dynatrace introduced a powerful, modern alternative — something that gives us complete control over the entire data journey, from ingestion to analysis. That’s where **OpenPipeline** comes in.

---

## Slide 2: OpenPipeline Flow

### Introduction

This is the new and improved pipeline — the **OpenPipeline** — and it completely transforms how we handle logs, metrics, events, and more. Let me walk you through how it works.

### Speaking Notes

- On the left, you see a variety of ingest sources:
  - Data from **OneAgent**,
  - APIs and custom endpoints,
  - And different types of telemetry: logs, metrics, topology, spans, and events.

- This data flows into a **routing layer**, where we can now dynamically route data based on metadata like account ID, cluster ID, or log group. This flexibility was not available in the classic model.

- Once routed, each data stream goes through a **processing pipeline** — where you can:
  - Apply custom processing logic,
  - Extract structured metrics or fields,
  - Enforce permission rules,
  - And control how and where the data is stored.

- All this data ultimately lands in **Grail**, Dynatrace’s data lakehouse, where it becomes immediately queryable and ready for analysis.

This means faster insights, greater control, and a much more scalable observability platform.
