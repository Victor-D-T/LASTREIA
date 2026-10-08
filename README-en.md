# LASTREIA

**Project memory, from conversations to decisions.**

[Main README](README.md) · [Portuguese version](README-pt.md)

LASTREIA is a proposed collaborative platform for collecting project sources, searching their content, and recording human decisions with traceability.

Every answer should show where the information came from, who said it, when they said it, and which decision resolves any conflicting accounts. Reports do not automatically become facts; AI suggestions do not automatically become approved decisions.

> Status: concept / initial documentation. No application, MCP server, transcription pipeline, or search functionality has been implemented yet. This repository records the starting point, not a working product.

## The problem

Teams conduct interviews and meetings across multiple projects, but store and synthesize the material in different ways. Information becomes scattered, opinions conflict, and decisions get lost between conversations.

## Proposal

- Organize sources and participants by project.
- Preserve original files, including audio, documents, and PDFs.
- Generate searchable derivatives such as transcripts, chunks, and embeddings.
- Search by meaning and keywords while retaining source references.
- Retrieve excerpts or complete documents through authenticated MCP tools.
- Record explicit decisions: who confirmed, who approved, when, scope, and effective dates.
- Preserve history when a decision is revised or superseded.
- Play audio at the timestamp associated with a transcript excerpt.

## Proposed MVP

1. Sign-in and identification of organization members.
2. Project registration and listing, including participants and responsible people.
3. Upload of original sources and linked derivatives.
4. Semantic chunk search filtered by project.
5. Retrieval of sources or excerpts with context.
6. Decision lookup, proposals, and confirmation with an audit trail.
7. Minimal interface for uploads, projects, and reviewing evidence.

Initially, members of the same organization may share access to projects. Authentication and isolation between organizations remain mandatory, and participants and responsible people must be identified. Granular project/source permissions are a later enhancement.

## Technical direction

- **TypeScript** for backend, frontend, and shared contracts in a monorepo.
- **Firebase** as the initial candidate: Auth, Hosting, Firestore vector search, and Storage for files.
- **Authenticated MCP** as the assistant access interface, without depending on a single LLM model or provider.
- **Local-first processing** for transcription and embedding generation, with managed processing as a future option.
- **Storage and search adapters** to avoid coupling contracts to Firestore.

The final configuration, models, costs, and MCP authentication strategy still need validation. Free quotas do not guarantee zero operating cost.

## Principles

- **Preserved originals:** transcription does not mean deleting or replacing audio.
- **Evidence before conclusions:** answers point to verifiable sources.
- **Explicit human decisions:** AI proposes and flags conflicts; it does not fabricate approvals.
- **Contextual authority:** responsible people and approvers may vary by subject/project.
- **Preserved history:** newer does not automatically mean correct.
- **Privacy:** identity comes from authentication; files do not issue instructions to the system.
- **Portability:** aim to support self-hosting and, later, a managed service.

## Documentation

- [Initial architecture and scope](docs/architecture.md)

## Development

This repository does not contain executable code or installation commands yet. The next step is to define the monorepo structure and build a vertical slice: authenticate, create a project, upload text, search, and record a decision.

## Contributions and licensing

Scope and architecture contributions are welcome. The intention is to develop the project openly; maintainers have not selected a license yet. Do not assume usage rights merely because the repository is public.

Do not submit real transcripts, personal data, tokens, or internal documents in issues, examples, or commits. Use synthetic data.
