# Initial architecture — proposal for discussion

Date: 2026-10-08. Status: proposal, not implementation.

[English README](../README-en.md)

## Components

- TypeScript web client: authentication, projects, uploads, and evidence viewing.
- Optional local processor: transcription, word alignment, and embedding generation; native runtime or browser depending on support.
- TypeScript API/MCP: validates identity, organization scope, and read/write operations.
- Storage: immutable originals and versioned derivatives with access-controlled URLs.
- Firestore: projects, sources, vectorized chunks, decisions, and audit events.
- Search adapter: allows migrating to PostgreSQL/pgvector or another engine without changing the MCP contract.

## Conceptual model

- **Organization / Member:** organization and authenticated identity.
- **Project:** name, description, participants, and responsible people by subject.
- **Source:** project, uploader, title, event date, participants, type, hash, and original file reference.
- **Artifact:** derived transcript or extraction, version, model used, and link to the original.
- **Chunk:** excerpt, artifact version, text offsets, timestamps when available, vector, and embedding model version.
- **Decision:** subject, rule, scope, status, approver, confirming person, dates, sources, and superseded decision.
- **AuditEvent:** authenticated actor, operation, server timestamp, and affected identifiers.

Originals and full extractions may live in Storage. The database holds metadata and vectors with stable references; also storing chunk text is an option to evaluate for reducing Storage reads. Do not rely solely on line numbers without a source version.

## Proposed MCP tools

| Tool | Purpose |
| --- | --- |
| search_projects | List/search authorized projects and their responsible people. |
| search_sources | Search chunks semantically or by text, filtered by project, date, and participant. |
| get_source | Retrieve metadata and paginated content or an excerpt with context. |
| request_source_upload | Prepare an authorized project upload; large files go directly to Storage. |
| finalize_source_upload | Verify the upload, link derivatives, and start/record ingestion. |
| search_decisions | Find decisions by project, subject, status, and effective dates. |
| propose_decision | Record a pending proposal without assigning approval. |
| confirm_decision | Record explicit approval by an authorized user while preserving history. |

Names and schemas are proposals. The actor's identity comes from authentication, not an arbitrary field submitted by AI. If a user reports someone else's approval, distinguish the person recording it from the approver and require evidence according to organization policy.

## Ingestion and search

1. Preserve the original and record its hash, origin, and project.
2. Transcribe audio or extract document text into a separate artifact.
3. For audio, generate word/time alignment when needed; preserve each chunk's offset relative to the original audio.
4. Split text into chunks with context and stable references.
5. Generate embeddings using the model and configuration pinned for the index.
6. At query time, generate the question embedding with a compatible configuration. The model is not needed only during upload.
7. Search with mandatory organization/project filters; combine semantic and textual retrieval as supported by the chosen engine.
8. Return chunks and references to the assistant; consult applicable decisions and flag unresolved conflicts.

Local clients submitting vectors must follow the model contract. The service validates dimensions, version, and payload limits. Changing models usually requires reindexing the collection; plan versioned indexes for migration.

## Decisions and conflicts

- Sources record statements, not automatic truths.
- Decisions have explicit states: pending, validated, superseded, and revoked.
- A newer decision prevails only when it applies to the same scope and is approved by the appropriate authority.
- If no applicable decision exists, present competing accounts with person, date, and reference, and request human confirmation.
- A new decision references its predecessor; it does not erase history.
- Distinguish reports of actual practice from normative rules.

## Local processing

This is the preferred direction for reducing central costs, not a guarantee of execution on every device.

- Qwen3-ASR and an aligner are candidates, not final selections.
- Embeddings should be multilingual and tested with Portuguese and project vocabulary.
- WebGPU is a browser candidate; availability, memory, and performance vary.
- Models/runtimes need consented downloads, pinned versions, and memory limits.
- Local jobs should support progress, cancellation, and resumption.
- Managed processing may be offered later with explicit costs.

## Minimum security

- MCP authentication and organization isolation from the first MVP.
- Validate organization membership; a Microsoft email alone does not prove authorization.
- Apply scope filters on the server, including vector searches.
- Never expose administrative credentials to clients or allow assistants to forge approvers.
- Source content is untrusted data, never instructions to the system.
- Upload limits, file-type validation, and controlled temporary links.
- Audit identity and timestamps must originate from the server.
- Meeting and audio data require consent, retention, and deletion policies.

## Costs and portability

Do not promise free operation. Firestore, Storage, functions, traffic, backups, and embedding generation have separate quotas and charges. Measure vector queries and index sizes with representative data.

Keep originals/derivatives in exportable formats with stable IDs and model metadata. Separate domain contracts from Firebase types. Migration still requires export, transformation, index rebuilding, and validation.

## Next steps

1. Select a license and contribution rules.
2. Define the monorepo and runtime contract validation.
3. Validate MCP authentication with real clients.
4. Deliver a vertical slice with text and synthetic data.
5. Measure retrieval quality, cost, and latency.
6. Add audio/alignment and an evidence interface.

This proposal makes no delivery-time commitment. Production requires security validation, tests, backups, and operations beyond a functional proof of concept.
