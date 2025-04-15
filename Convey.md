# ✅ What the Term “OpenPipeline” Conveys

This table outlines the dual meaning of the term **OpenPipeline** in the Dynatrace platform — highlighting its openness, flexibility, and streaming architecture.

| **Component** | **Interpretation**                 | **What It Conveys** |
|---------------|------------------------------------|----------------------|
| **Open**      | **Open Ingestion & Standards**     | Accepts data from any source — OneAgent, OpenTelemetry, APIs, Fluent Bit, Cribl, and more. It’s vendor-agnostic. |
|               | **Open Access to Configuration**   | Fully configurable through UI or API: parsing, masking, routing, metric extraction, and more. |
|               | **Open for Extension**             | Extensible architecture — new processors, data types, and transformations can be added. |
|               | **Transparent Processing**         | Real-time preview, validation, and debug support before applying changes. No "black box." |
|               | **Community-Aware**                | Built to work seamlessly with open standards like OpenTelemetry and cloud-native formats. |
| **Pipeline**  | **Stream-Based Data Processing**   | Data flows through well-defined, real-time stages: `Processing → Metric Extraction → Data Extraction → Permissions → Storage`. |
|               | **Composable & Modular**           | You can define team-specific pipelines, reuse processors, and modularize data flows. |
|               | **Ingest-Time Transformation**     | Enables transformation and normalization *before* data reaches storage — like a live ETL pipeline. |
|               | **Governance-Friendly**            | Enforces access control, masking, cost attribution, and routing logic *as data arrives*. |
