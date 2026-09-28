# Architecture

## Delivered preview architecture

The submitted preview is a single static HTML file. It runs entirely in the browser and uses synthetic, deterministic data. No clinical note or uploaded file is sent to an external service.

```mermaid
flowchart LR
  U[User] --> UI[Standalone HTML UI]
  UI --> I[Text input or file selection]
  I --> M[Deterministic mock analysis]
  M --> R[Structured review report]
  R --> V[Report sections and review flags]
  R --> H[In-memory report history]
```

## Target production architecture from the build specification

The intended full-stack implementation is:

```mermaid
flowchart LR
  U[User] --> F[React + Vite UI]
  F -->|REST| B[Express API]
  B --> V[Validation + Multer]
  V --> X{Input type}
  X -->|text| C[Text cleaner]
  X -->|PDF| P[pdf-parse]
  P -->|little text| O[Render pages + Tesseract OCR]
  X -->|image| O
  P --> C
  O --> C
  C --> A[AI provider adapter]
  A --> S[Zod schema validation + hallucination guard]
  S --> D[(SQLite)]
  D --> B
  B --> F
```

The production design separates the browser interface from extraction, AI calls, validation, persistence, and error handling. This prevents document processing and provider credentials from being exposed to the browser.
