# Project Pelagos: The Open Specification for Managed Data Volumes

**Pelagos** (from the Greek *pelagos*, meaning "the open or deep sea") is an open-source specification and reference implementation dedicated to defining a universal framework for **Data Volumes**.

In the modern data ecosystem, technologies like Apache Iceberg and Delta Lake have standardized structured tabular data. However, the vast majority of data, including unstructured collections of logs, images, and binary files, remains unmanaged, often turning object storage into **Data Swamps**. Pelagos bridges this gap by treating the **Volume** as a first-class citizen.

---

## Proposal Feedback

Please review and comment on the proposal draft here:
[Google Doc (comment access)](https://docs.google.com/document/d/1K-yMaUtEuRzH5ruuST8g57hWypiDZ18Rt1XE5inN8JU/edit?tab=t.0)

---

## The Core Problem

Pelagos addresses three fundamental gaps in modern object storage:

- **The Access Gap:** A lack of standardized REST mechanisms for requesting scoped, temporary access to collections of objects.
- **The Context Gap:** Raw storage paths lack critical metadata regarding ownership, data classification, and AI readiness.
- **The Standard Gap:** There is no shared specification for discovering and accessing volumes across different tools (for example, Spark, AI agents, and governance engines).

---

## Key Architectural Pillars

### 1. Control Plane-Native Authority

Unlike other formats that store metadata sidecars in the storage bucket, Pelagos centralizes volume state within the **Control Plane**.

- **Sole Source of Truth:** The Control Plane is the only authority for volume layout and version history.
- **Pure Data Plane:** Storage buckets contain only raw data, reducing attack surface and improving operational efficiency.
- **Atomic Transactions:** Supports multi-writer optimistic concurrency and atomic multi-volume commits.

### 2. Zero-Trust Security (Credential Vending)

Pelagos acts as a credential broker, issuing short-lived, scoped tokens through cloud-native services (AWS STS, GCP Workload Identity, and others).

- **Snapshot-Isolated Scoping:** Access is cryptographically or policy-bound to the exact files in a specific snapshot.
- **Access Profiles:** Supports multiple enforcement styles, including **ABAC (Tag-Based)**, **Manifest-ARN**, **Pre-Signed URLs**, and a **Pelagos Proxy (Gateway)**.

### 3. AI Agent Readiness (The Cognitive Layer)

Pelagos moves beyond simple tags to provide machine-actionable context for autonomous systems.

- **Pre-processing State:** Standardized flags for `pii_scrubbed`, `deduplicated`, and `normalized`.
- **Embedding Compatibility:** Structured context for vector dimensionality and model lineage.
- **Safety Notarization:** Auditable safety gates including bias reviews and synthetic content ratios.

### 4. Storage Decoupling and Lifecycle

- **Infrastructure Agility:** Migrate data across regions or providers without changing application code.
- **Tier Transparency:** Logical identifiers remain constant even as data moves between `hot`, `warm`, and `archive` tiers.
- **Automatic Maintenance:** Internal orchestration for compaction and orphaned file garbage collection.

---

## Specification vs. Implementation

- **The Specification:** A vendor-neutral REST API defining the Storage Plan JSON schema, credential vending handshakes, and metadata envelopes.
- **Rust Reference Server:** A high-performance implementation that serves as the living standard for the Pelagos protocol.
- **Polyglot SDKs:** Language-agnostic client libraries (initially Rust, Python, Java, and Go) to enforce snapshot boundaries and manage credential refresh.

---

## Comparison: Pelagos vs. Apache Iceberg

Pelagos is architecturally distinct and is not intended as an extension of Iceberg.

| Feature | Apache Iceberg | Project Pelagos |
| :--- | :--- | :--- |
| **Primary Primitive** | Table (Structured) | Volume (Format-agnostic) |
| **Metadata Location** | Distributed (Data Bucket) | Centralized (Control Plane) |
| **Security Focus** | Prefix-level/IAM | Snapshot-Isolated Manifests |
| **AI Integration** | Out of scope | Core protocol concern |

---

## Governance

Project Pelagos is currently in its initial proposal phase and is being developed with the intent to be transferred to the **Apache Software Foundation (ASF)** to ensure long-term, vendor-neutral governance.

---

## Roadmap

- [ ] Finalize OpenAPI specification for the Volume REST API.
- [ ] Release alpha version of the Rust reference server.
- [ ] Publish Python and Go SDKs.
- [ ] Define a global federation protocol for oceans of control planes.
