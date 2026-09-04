# SAB Product Brief

Status: **Discovery proposal — approval required before implementation**

Repository: `leifheaney5/HTS-Substrate-AI-Bridge--SAB-1-`

## 1. Discovery findings

The repository is currently empty on GitHub: no commits, branches, README, source files, tests, deployment configuration, issues, or pull requests are present. No authoritative product requirements, local instructions, security guidance, or acronym definitions are available in the repository.

The supplied project direction describes SAB as a focused GIS-to-AI integration layer for Hawkeye Tech, but the official expansions of **HTS** and **SAB** should remain unclaimed until explicitly documented by the project owner.

Because no authoritative product brief exists, implementation must stop after this discovery document until scope is approved.

## 2. Proposed product purpose

**Recommended scope:** SAB is a reusable GIS-AI bridge that accepts structured geospatial evidence, validates and queries it, performs deterministic spatial analysis, and produces evidence-linked AI summaries and map-ready outputs for explicit human review.

One-sentence product definition:

> SAB connects GIS evidence to constrained, cited, human-reviewed AI workflows without replacing the analyst or duplicating MNTR.

The product should demonstrate how geospatial systems and AI services can interoperate through typed, auditable, reviewable contracts while keeping deterministic analysis separate from generative interpretation.

## 3. Intended users

Primary users:

- Geospatial analysts who need to inspect and query evidence before requesting AI assistance.
- Intelligence or research analysts who need cited explanations derived only from selected spatial evidence.
- Engineers integrating GIS applications with constrained AI services.
- Demonstration and evaluation stakeholders who need a reproducible, inspectable workflow rather than a black-box AI demo.

Secondary users:

- Product and security reviewers evaluating provenance, trust boundaries, failure states, and human-approval controls.

## 4. Relationship to MNTR and Trodden

SAB must remain complementary to the existing product family:

- **MNTR** is the broad analyst platform for sources, events, investigations, timelines, alerts, provenance, and reporting.
- **Trodden** is the specialized imagery-to-evidence workflow for detecting informal paths, temporal persistence, spatial comparison, and movement evidence.
- **SAB** is the bridge layer that lets structured GIS evidence flow into constrained AI workflows and returns cited, map-ready, reviewable outputs.

SAB is **not** a second MNTR, a replacement for Trodden, or a general-purpose GIS platform. Cross-repository integration should be documented as a future contract boundary, not built prematurely.

## 5. Candidate scopes considered

### Option A — Recommended: GIS evidence → deterministic analysis → cited AI explanation

A compact integration console that validates local geospatial evidence, builds a visible query plan, runs deterministic spatial/temporal analysis, then allows an AI provider to explain only the returned evidence with citations and human approval.

Why this is preferred:

- Demonstrates the actual bridge between GIS and AI.
- Has a narrow, testable vertical slice.
- Produces clear case-study artifacts.
- Avoids duplicating MNTR or Trodden.
- Can run entirely with synthetic/local data and a deterministic mock AI provider.

### Option B — Contract-first integration SDK

A schema and adapter package focused almost entirely on typed evidence contracts, provider boundaries, provenance, validation, and replay, with only a minimal CLI/demo UI.

Tradeoff: stronger engineering-library positioning, but less visually compelling as a case study and weaker demonstration of analyst review.

### Option C — Geospatial query workbench

A richer UI emphasizing layer exploration, AOI selection, query construction, and spatial result inspection, with AI explanation as a secondary panel.

Tradeoff: visually useful but risks drifting into a standalone GIS product and duplicating capabilities that belong elsewhere.

**Recommendation: approve Option A.**

## 6. Smallest useful vertical slice

```text
GIS evidence bundle
→ schema validation
→ AOI + structured spatial/temporal query
→ visible validated query plan
→ deterministic spatial result
→ evidence-linked AI summary
→ map + accessible results table
→ analyst review / edit / approve / reject
→ provenance-aware export + replay
```

A user should be able to:

1. Load a local synthetic GIS evidence bundle.
2. Inspect map layers and metadata.
3. Draw or select an area of interest.
4. Configure a structured spatial and temporal query.
5. Run deterministic operations such as `intersects`, `within`, `near`, `buffer`, count-by-layer, time-window filtering, source comparison, and before/after change summary.
6. Inspect returned features in both a map and accessible table.
7. Ask a natural-language question about only the returned evidence.
8. See the natural-language request converted into a visible validated query plan.
9. Generate an AI-assisted answer constrained to the retrieved evidence.
10. Inspect citations to source IDs, feature IDs, timestamps, and derived calculations.
11. See explicit missing, stale, uncertain, partial, and unavailable-source states.
12. Edit, reject, or approve the answer before export.
13. Export and replay the complete run.

## 7. Data sources and ownership assumptions

Initial demo data should be **synthetic, deterministic, local, and distributable with the repository**.

The first version should not require external accounts, paid GIS providers, private datasets, or network access.

Each evidence item should carry, where applicable:

- Stable feature ID.
- Source ID and source type.
- Geometry and geometry type.
- Observation and ingestion timestamps.
- Spatial precision or uncertainty.
- License or usage metadata.
- Original source reference.
- Observed attributes.
- Derived attributes.
- Confidence or quality metadata.
- Processing status.
- Provenance links.
- Synthetic/demo status.

Missing values must remain distinct from zero values. Observed, calculated, inferred, and generated fields must remain distinguishable.

The contract should be versioned and support GeoJSON or another standard map-ready representation, machine-readable query results, human-readable evidence summaries, deterministic fixtures, and full export/replay.

## 8. Proposed architecture

Use the smallest maintainable architecture that preserves strong boundaries between:

1. **Evidence ingestion** — reads local evidence bundles.
2. **Schema validation** — enforces versioned typed contracts and geometry constraints.
3. **Geospatial query planning** — converts UI or natural-language intent into a restricted, visible query plan.
4. **Deterministic spatial analysis** — executes only supported spatial and temporal operators.
5. **AI provider adapters** — interface for deterministic mock provider first, optional external providers later.
6. **Prompt construction** — supplies only query-selected evidence.
7. **AI output validation** — validates structured output and rejects unsupported claims.
8. **Provenance/citations** — traces every material statement to evidence or deterministic calculations.
9. **Map-ready serialization** — returns stable output for the map and accessible table.
10. **Human review state** — draft, edited, approved, rejected.
11. **Export/replay** — preserves input bundle version, query plan, deterministic results, AI output, citations, and review state.

Future MNTR/Trodden integration should occur only through versioned contracts. The future boundary must document what each system sends to SAB, what SAB returns, which fields are source facts vs derived vs AI-generated, which outputs require approval, and how provenance survives the round trip.

## 9. AI trust model

All AI output is untrusted until reviewed.

The AI layer must:

- Receive only evidence selected by a validated query.
- Never directly execute arbitrary SQL, shell commands, URLs, or provider requests.
- Never invent features, coordinates, dates, sources, measurements, or outcomes.
- Return schema-validated structured output.
- Cite evidence for every material statement.
- Separate observed facts, deterministic calculations, interpretation, and recommendations.
- State when evidence is insufficient, stale, partial, or unavailable.
- Never output person-level tracking, targeting, threat, criminality, hostility, intent, or risk conclusions.
- Never authorize irreversible or privileged external actions.
- Preserve explicit human edit/approve/reject control.

A deterministic mock provider is required so the complete demo works without credentials or external network access.

## 10. Trust boundaries

Treat the following as separate trust zones:

- User-supplied evidence bundle.
- Schema validator.
- Query planner.
- Deterministic geospatial engine.
- AI prompt boundary.
- AI provider response.
- Output validator.
- Review/approval state.
- Exported artifact.

The AI provider must never be treated as an authority on the evidence or permitted to bypass the deterministic query layer.

## 11. Security and privacy risks

The implementation should explicitly address:

- Malformed GeoJSON and invalid geometries.
- Excessive geometry size/complexity.
- Oversized requests.
- Query and provider timeouts.
- Prompt injection embedded in evidence attributes.
- Arbitrary URL fetching or remote-content dereferencing.
- Arbitrary code/SQL/shell execution.
- Secret leakage to the frontend or logs.
- Sensitive data in application logs.
- Unsupported AI claims.
- Missing or forged citations.
- Confusion between synthetic and live data.
- Unreviewed AI actions.
- Stale, partial, or unavailable evidence.

Controls should include input/schema validation, request-size limits, geometry bounds/complexity limits, query/provider timeouts, rate limits where appropriate, safe error messages, server-side secret handling, no credentials in frontend bundles, audit records, and explicit human approval.

The public case-study demo must use synthetic or explicitly authorized data only.

## 12. Explicit non-goals

SAB is not:

- A replacement for MNTR.
- A second generic intelligence platform.
- A generic chatbot.
- A full standalone GIS product.
- An autonomous targeting system.
- A person-tracking or surveillance system.
- A threat, hostility, criminality, or intent classifier.
- A system that permits AI to directly execute arbitrary SQL, shell commands, URLs, or external actions.
- A production compliance claim or certified operational system.

## 13. Public claims that must not be made without direct evidence

Do not claim:

- Customer deployment or adoption.
- Production readiness.
- Certified security or regulatory compliance.
- Mission impact or operational effectiveness.
- Productivity improvements.
- Scale, throughput, latency, availability, or uptime unless directly measured and documented.
- Accuracy, false-positive reduction, or analyst-performance improvement unless evaluated with a defined methodology.
- Use of live operational data when the demo uses synthetic data.

All screenshots and measurements used in a case study must come from the running project.

## 14. Proposed demonstration interface

The first UI should be a compact GIS-first integration console containing:

- Map view.
- Layer/evidence list.
- AOI selector.
- Structured query controls.
- Visible generated query plan.
- Results table.
- Evidence detail panel.
- AI explanation panel.
- Citations/provenance panel.
- Uncertainty and limitation messaging.
- Review/edit/approve/reject controls.
- Export/replay controls.
- Persistent synthetic-demo labeling.

Visual direction: dark, technical, precise, calm, high-contrast, evidence-oriented, and restrained. Avoid decorative AI effects and avoid recreating MNTR’s full analyst workspace.

## 15. Proposed implementation files after approval

Exact technology choices should remain minimal and be finalized at implementation time, but the first build is expected to introduce a structure similar to:

```text
README.md
docs/
  PRODUCT_BRIEF.md
  ARCHITECTURE.md
  SECURITY.md
  DATA_CONTRACT.md
  INTEGRATION_BOUNDARY.md
  DEMO_SCRIPT.md
src/
  evidence/
  geo/
  query/
  ai/
  provenance/
  review/
  export/
  ui/
fixtures/
  synthetic-demo/
schemas/
tests/
  unit/
  integration/
  browser/
```

No application files should be created until this brief is approved.

## 16. Test strategy after approval

Unit/integration coverage should include:

- Evidence schema validation.
- Malformed GeoJSON.
- Invalid geometries.
- Oversized/complex AOIs.
- Supported spatial predicates.
- Time-window filtering.
- Missing values.
- Stale/unavailable sources.
- Deterministic calculations.
- Query-plan validation.
- AI output schema validation.
- Unsupported AI claims.
- Citation completeness.
- Provider timeout/failure.
- Authorization boundaries.
- Export/replay behavior.
- Human approval state transitions.

Browser coverage should include loading the demo, selecting an AOI, running a query, inspecting map/table results, asking a question, viewing citations, approving/rejecting an answer, failure/partial/stale/empty states, keyboard navigation, responsive layouts, reduced motion, and automated accessibility checks.

## 17. Case-study evidence plan after approval

Produce only artifacts derived from the running project:

- Architecture diagram.
- Data-flow diagram.
- GIS-to-AI workflow diagram.
- Map/evidence screenshot.
- Validated query-plan screenshot.
- Evidence-linked AI response screenshot.
- Uncertainty/degraded-source screenshot.
- Short demo script.
- Reproducible local setup instructions.
- Example evidence bundle.
- Example exported result.
- Provenance and limitations statement.

Clearly label synthetic data, local demo mode, any live provider data, deterministic calculations, AI-generated text, and human-reviewed content.

## 18. Known risks and open questions

1. What do **HTS** and **SAB** officially stand for, if anything beyond the repository naming convention?
2. Is the recommended Option A scope approved as the canonical product direction?
3. What implementation stack should be preferred if Hawkeye Tech already has a standard frontend/backend stack?
4. Should the first release support only GeoJSON, or GeoJSON plus additional formats such as GeoPackage/CSV-with-coordinates?
5. Which spatial library/runtime is preferred for deterministic operations?
6. Should external AI provider adapters be implemented in v1, or only the deterministic mock provider plus a provider interface?
7. What fields must be compatible with future MNTR and Trodden contracts?
8. Are there Hawkeye Tech visual identity assets or accessibility standards to apply?
9. What license should govern the repository and bundled synthetic fixtures?

## 19. Approval gate

**No application implementation should begin until the project owner approves the product scope and vertical slice above.**

Recommended approval decision:

> Approve Option A: a focused GIS evidence → deterministic analysis → cited AI explanation → human review workflow, with synthetic/local data and a deterministic mock AI provider as the first complete case-study slice.
