# Microsoft Fabric Data Engineering Learning Guide

## 1. Microsoft Fabric Architecture and Core Concepts

### ✅ Key Concepts
- **Fabric Architecture Layers**:
  - **OneLake**: Unified, logical data lake built on top of Delta Lake. It acts as a single storage layer accessible by all compute engines and workspaces.
  - **Compute Engines**:
    - **Spark**: Big data processing engine ideal for transformation and ML workloads.
    - **SQL**: Traditional T-SQL interface used in Warehouse and Lakehouse for structured analytics.
    - **KQL (Kusto Query Language)**: Optimized for telemetry, logs, and time-series data.
  - **Fabric Items**:
    - **Lakehouse**: Hybrid storage supporting file-based and tabular formats, useful for medallion architecture (bronze/silver/gold layers).
    - **Warehouse**: Fully managed SQL environment optimized for relational modeling and integration with Power BI.
    - **Eventhouse**: Purpose-built for storing and analyzing streaming data using KQL.
    - **KQL Database**: Logical container for structured time-series and telemetry data using Kusto Engine.

### 📘 Learn
- **OneLake Concepts**:
  - Supports shortcutting and data virtualization.
  - Accessed via semantic models or compute engines.
- **Delta Format Fundamentals**:
  - ACID Transactions: Guarantees data consistency.
  - Schema Enforcement: Prevents incompatible writes.
  - Time Travel: Allows querying previous versions.
- **Fabric SKUs**:
  - F-Series capacity determines resource allocation.
  - Shared across items like pipelines, lakehouses, and notebooks.
- **Workspace Design**:
  - Assign capacity to workspaces.
  - Support multi-user collaboration.
  - Role-based access and deployment structure.

---

## 2. Data Ingestion Solutions

### ✅ Key Concepts
- **Data Ingestion Tools**:
  - **Pipelines**: Orchestrated workflows for batch and scheduled data movement.
  - **Eventstreams**: Event-based ingestion from Event Hubs, IoT Hub, Kafka.
  - **Dataflows Gen2**: GUI-driven ingestion with Power Query for shaping and combining data.
  - **Notebooks**: Custom ingestion logic using PySpark, Scala, or SQL.

### 📘 Learn
- **Real-Time vs Batch**:
  - *Real-time*: Low-latency, high-velocity data (IoT, telemetry).
  - *Batch*: Periodic ingestion (daily/weekly), structured.
- **On-Premises Data Ingestion**:
  - Use Data Gateway with Pipelines for secure access.
  - Configure linked services and datasets.
- **Change Data Capture (CDC)**:
  - Supported in Azure SQL and Synapse.
  - Enables incremental ingestion in Pipelines, Eventstreams.
- **Change Detection Patterns**:
  - Detecting deltas using timestamps or hashes.
  - Useful for large datasets where full reload is costly.

---

## 3. Data Transformation and Orchestration

### ✅ Key Concepts
- **Transformation with Spark**:
  - Use Notebooks with PySpark/Scala for scalable processing.
  - Supports distributed joins, aggregations, window functions.

- **Data Pipelines**:
  - Control Flow Elements: `If`, `Switch`, `Wait`, `ForEach`, `Until`.
  - Parameterization: Dynamic expressions using system variables.
  - Dependency Handling: Triggers between notebook activities.

### 📘 Learn
- **Eventstream Windowing Functions**:
  - **Tumbling**: Fixed, non-overlapping windows.
  - **Hopping**: Overlapping sliding windows.
  - **Sliding**: Triggered on every event.
- **Streaming Transformations**:
  - Aggregations: `Avg`, `Count`, `Min`, `Max`.
  - Filters and projections.
- **Lakehouse Orchestration**:
  - Chain Notebooks and Pipelines.
  - Automate bronze → silver → gold table creation.

---

## 4. Data Modeling and Storage

### ✅ Key Concepts
- **Modeling Approaches**:
  - **Star Schema**: Central fact table surrounded by dimension tables.
  - **Keys**:
    - *Surrogate Key*: Artificial, often an identity column.
    - *Natural Key*: Derived from business logic (e.g., CustomerID).

- **Slowly Changing Dimensions (SCD)**:
  - **Type 2**: Preserve historical records using `ValidFrom`/`ValidTo`.
  - **Hashing**: Detect changes by comparing hash of all attribute columns.

### 📘 Learn
- **Schema Evolution**:
  - Delta Tables: Supports column additions, data type evolution.
  - KQL Tables: Update policies can adapt data shapes.
- **Delta Table Operations**:
  - `OPTIMIZE`: Merge small files, improve query performance.
  - `VACUUM`: Clean obsolete files from deleted versions.
  - `V-Order`: Reorder data for Verti-Scan engines like Power BI.
- **Table Properties**:
  - Time travel history, file count, row group size.

---

## 5. Security and Access Control

### ✅ Key Concepts
- **Security Controls**:
  - *Row-Level Security*: Restrict rows based on user identity.
  - *Column-Level Security*: Hide sensitive columns.
  - *Dynamic Data Masking*: Obfuscate column values at query time.

- **Git Integration**:
  - Source control for notebooks, pipelines, lakehouses.
  - Enables CI/CD via YAML pipelines.

### 📘 Learn
- **Access Models**:
  - Workspace Roles: Admin, Member, Contributor, Viewer.
  - Object Permissions: Dataset, Notebook, Table-level security.
- **Least Privilege Principle**:
  - Users should have only the permissions needed.
  - Auditing and governance through activity logs.
- **DevOps Best Practices**:
  - Use Git branches per environment.
  - Validate and test pipeline changes before promoting.

---

## 6. Real-Time Intelligence and Streaming

### ✅ Key Concepts
- **Eventstream Operators**:
  - `Aggregate`, `Group By`, `Join`, `Project`, `Filter`, `Union`.
  - Visual flow editor for defining logic.

- **Eventhouse Capabilities**:
  - Fast ingestion with short retention.
  - Optionally sync with OneLake for long-term storage.

- **Streaming Architectures**:
  - *Eventstream*: Declarative, visual, and user-friendly.
  - *Streaming Dataflow*: Drag-and-drop for non-coders.
  - *Spark Streaming*: Custom code, large-scale processing.

### 📘 Learn
- **KQL Basics**:
  - Time-series joins, summarization, filtering.
  - Query optimization for large telemetry datasets.
- **Materialized Views**:
  - Pre-aggregated summaries for near real-time insights.
  - Scheduled refresh and KQL functions.

---

## 7. Monitoring, Performance, and Troubleshooting

### ✅ Key Concepts
- **Monitoring Options**:
  - SQL DMV Views: `sys.dm_exec_requests`, `sys.dm_exec_sessions`.
  - Query Insights Tables:
    - `long_running_queries`: Detect expensive queries.
    - `frequently_run_queries`: Optimization candidates.

- **Performance Tuning**:
  - *Direct Lake Mode*: Power BI queries data directly from OneLake.
  - *Delta Tuning*: Avoid small files, use V-Order for faster scans.

### 📘 Learn
- **Pipeline Monitoring Hub**:
  - View success/failure of activities.
  - Diagnose performance bottlenecks.
- **Deployment Pipelines**:
  - Promote artifacts across dev → test → prod.
  - Limitation: No Git branch merge; environments must be managed manually.
- **Metrics and Alerts**:
  - Fabric Admin Portal: Monitor workspace usage.
  - Set alerts for capacity thresholds, job failures.

---

## 8. Advanced Delta Lake Optimization

### ✅ Key Concepts
- **V-Order Write Optimization**:
  - Enables improved scan performance by reordering columns.
  - Enhances compression and query performance in Power BI and SQL engines.
  - Fully compatible with open-source Parquet readers.
- **Z-Order vs V-Order**:
  - *Z-Order*: Optimizes file layout for multi-column filtering.
  - *V-Order*: Targets Fabric compute engines for better columnar access.
- **Maintenance Commands**:
  - `OPTIMIZE`: Compacts small files.
  - `VACUUM`: Removes obsolete file versions based on retention period.
  
### 📘 Learn
- Enable/disable V-Order depending on workload.
- Combine `OPTIMIZE` with strategic partitioning.
- Monitor delta table performance using file count and read latency metrics.

---

## 9. Change Tracking and Slowly Changing Dimensions (Advanced)

### ✅ Key Concepts
- **Hashing for Change Detection**:
  - Use hashing functions (e.g., `hashbytes`) across multiple columns.
  - Efficient for tracking changes in wide tables (50+ columns).
- **CDC with Streaming**:
  - Integrate Eventstream and Delta Lake to support real-time SCD patterns.
  - Combine with watermarks or windowing to limit data volume.
- **SCD Types Overview**:
  - *Type 0*: No changes allowed (read-only).
  - *Type 1*: Overwrites previous values (no history).
  - *Type 2*: Adds new rows with versioned history.

### 📘 Learn
- Implement SCD Type 2 using MERGE in Spark/SQL.
- Apply hash comparisons in pipelines to reduce unnecessary updates.
- Handle schema drift using flexible delta schema evolution.

---

## 10. Deployment Pipelines and Governance

### ✅ Key Concepts
- **Deployment Pipelines**:
  - Automate promotion of items (e.g., semantic models, lakehouses).
  - Manage configurations across dev → test → prod stages.
- **Known Limitations**:
  - No Git branch merge or full semantic diff.
  - No rollback or conflict resolution at item level.
- **Governance Tools**:
  - Endorsements: *Certified* and *Promoted* labels.
  - Sensitivity labels and data classification for DLP.

### 📘 Learn
- Design workspace structure to mirror environments.
- Parameterize pipelines to switch between configurations.
- Integrate with Azure DevOps or GitHub Actions for CI/CD.
- Use activity logs and audit trails to validate compliance.

---

