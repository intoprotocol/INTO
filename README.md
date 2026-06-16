# INTO Protocol / INTACT Layer

INTO Protocol is a Filecoin-backed infrastructure protocol for AI-generated work integrity.

This repository contains the Phase 1 MVP: **INTACT Layer**, a lightweight proof, trace, evidence backup, and verification layer for AI Agent workflows.

The core flow is:

```text
AI workflow trace -> trusted proof package -> Filecoin-backed evidence backup -> verification
```

INTACT Layer is not an AI Agent platform, chatbot product, enterprise dashboard, generic storage wrapper, billing system, token system, or RWA module. Its purpose is narrower and more foundational: when an AI Agent produces an output, INTACT Layer records enough structured evidence to prove what happened later.

## Why This Exists

AI Agents increasingly generate reports, retrieve documents, call tools, operate workflows, and make decisions on behalf of users. But most AI-generated work today lacks a reliable proof layer.

Often the final output exists, while the process behind it is unclear:

- What task was executed?
- Which AI Agent produced the output?
- What input, files, or data sources were referenced?
- What workflow events happened?
- What output was generated?
- Which hashes prove the input, output, evidence, and trace?
- Was the evidence backed up to Filecoin-backed storage?
- Can another system verify the proof later?

INTO Protocol answers these questions by creating tamper-evident proof records for AI-generated work.

## What This MVP Includes

- Next.js + TypeScript project scaffold
- Tenant API key authentication for integration APIs
- Data models for tenants, agents, tasks, workflow events, file references, output records, proof packages, evidence bundles, storage records, and verification records
- Task creation API
- Workflow event submission API
- File reference submission API
- Output metadata submission API
- Proof package generator
- Deterministic SHA-256 hashing for inputs, outputs, files, events, workflow traces, proof packages, and evidence bundles
- Storage connector abstraction
- Mock Filecoin-backed storage connector for local development
- Verification API
- Public verification page
- Basic proof report view
- Demo workflow showing an AI Agent task generating a proof package

## Phase 1 Scope

This repository focuses only on the INTACT Layer MVP:

- Standardized AI workflow trace schema
- Trace and metadata submission APIs
- Trusted output proof package generation
- Filecoin-backed evidence backup abstraction
- Storage metadata recording
- Verification API
- Public verification page
- Basic proof report template
- Demo integration workflow

Out of scope for this phase:

- Enterprise dashboard
- RWA evidence module
- Billing or usage metering
- Token system
- Full AI Agent framework
- Chatbot interface
- Advanced permission management
- Private deployment system
- Compliance SaaS suite

## Architecture

```mermaid
flowchart LR
  A["AI Agent App"] --> B["INTACT Trace Submission API"]
  B --> C["Workflow Trace Normalizer"]
  C --> D["Hashing + Proof Package Service"]
  D --> E["Evidence Bundle Builder"]
  E --> F["Storage Connector"]
  F --> G["Storage Metadata Registry"]
  G --> H["Verification API + Public Verifier"]
```

The current implementation uses a local JSON data store under `.intact-data/` so the MVP can run without provisioning infrastructure. The service layer is separated from API handlers so persistence can later move to PostgreSQL/Prisma.

The storage layer is also abstracted. Local development uses a mock Filecoin-backed connector that writes evidence bundles locally and returns Filecoin-style storage metadata such as storage URI, CID, PieceCID, retrieval URL, and retrieval status.

## Repository Structure

```text
src/app/                         Next.js App Router pages and API routes
src/app/api/v1/                  INTACT Layer API endpoints
src/app/verify/                  Public proof verification page
src/app/proofs/[proofId]/report  Human-readable proof report
src/lib/intact/                  Core models, services, hashing, proof, storage, verification
src/lib/intact/storage/          Storage connector abstraction and mock Filecoin connector
docs/                            API and architecture notes
examples/                        Demo workflow payload
prisma/                          Intended relational schema for PostgreSQL migration
tests/                           Hashing and proof flow tests
```


## Privacy Principles

INTACT Layer is designed for privacy-aware AI workflow verification:

- Store hashes when raw data is not needed
- Avoid storing plaintext sensitive content by default
- Support evidence file upload separately from metadata
- Keep raw payload storage optional
- Do not expose private task data on public verification pages
- Public verification should prove integrity, not leak customer data

## North Star

The MVP succeeds if an AI application developer can submit an AI Agent workflow trace, generate a trusted proof package, back up the evidence through Filecoin-backed storage, and share a public proof link that downstream users can verify.

INTO Protocol turns AI-generated work into verifiable proof records backed by Filecoin storage.
