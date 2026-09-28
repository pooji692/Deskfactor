# Technical Decisions

## Deterministic preview instead of live AI

The delivered preview uses a deterministic mock analysis. This keeps the artifact runnable without Node.js, an API key, or network access and makes the behavior reproducible for review. The trade-off is that it demonstrates the workflow rather than real extraction quality.

## Synthetic data only

All note content, names, identifiers, measurements, and reports are synthetic. This avoids exposing protected health information while keeping the interface realistic enough for evaluation.

## Single-file browser delivery

The preview is packaged as `index.html` with embedded CSS and JavaScript. This is the smallest dependable artifact for the current environment and can be opened directly. The trade-off is that it does not provide server-side OCR, persistence, API security, or live provider integration.

## Planned React and Express split

The production target remains React + Vite for the client and Node.js + Express for the API. This keeps secrets and document-processing dependencies on the server, allows a provider-neutral AI adapter, and supports a separately deployable Vercel frontend and Render backend.

## Schema-first AI output

The production implementation should use Zod with strict top-level keys. Schema validation provides a clear contract between an unreliable text generator and the rest of the application. The trade-off is that valid-but-incomplete content can still require human review.

## OCR fallback

Digital PDF text extraction should be attempted first. OCR should be used only when extracted text is too short, with a warning recorded for scanned-PDF processing and low confidence. This avoids unnecessary OCR cost and latency while supporting image-only documents.

## SQLite persistence

SQLite is appropriate for a small single-instance submission because it is simple, local, and easy to inspect. The production deployment needs a persistent disk; otherwise reports disappear on redeploy. A larger multi-instance deployment would need a managed database.

## No source-text retention by default

The production configuration should default to `RETAIN_SOURCE_TEXT=false`. Reports store validated structured output and processing metadata, while source text is not retained unless the owner explicitly enables it for a controlled environment.
