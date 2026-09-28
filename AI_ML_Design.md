# AI/ML Design

## Delivered preview

The current submission does not call an AI provider. It uses a deterministic mock report so the preview is safe to open and test without an API key, network access, or patient data. The sample note and all displayed report values are synthetic.

## Intended production approach

The build specification calls for an existing large language model accessed through a provider adapter. No model is trained for this project. The default provider is OpenAI, with the model selected through `AI_MODEL`. Automated tests should use a deterministic mock provider and must never call the network.

The provider contract is:

```text
generateReport(sourceText, options) -> raw JSON string
```

The production prompt must instruct the model to extract only information explicitly present in the delimited source text, use `null` or `[]` for absent values, identify missing information, report conflicts, and return one JSON object without markdown or commentary.

## Workflow

1. Validate exactly one input: text or file.
2. Extract text directly, from a digital PDF, or with OCR for scanned PDFs and images.
3. Clean line endings and repeated whitespace, then apply the configured character limit.
4. Send the cleaned source to the provider with temperature zero.
5. Parse and validate the response against the strict report schema.
6. Retry once with validation errors if JSON or schema validation fails.
7. Run deterministic post-checks for values that do not appear in the source text.
8. Save only validated completed reports; save metadata-only failed rows for invalid AI output.

## Uncertainty and safety

The design reduces risk through nullable schema fields, explicit prompt rules, a hallucination guard, OCR quality warnings, missing-information checks, and a persistent UI disclaimer. These controls reduce risk but cannot guarantee correctness. The tool is a document review support tool, not a diagnostic tool, and every output must be verified by a qualified healthcare professional.

## Limitations

Expected limitations include poor image quality, handwriting, terminology errors, provider latency or rate limits, single-instance SQLite scaling, and the absence of authentication and role-based access control. A live provider run and production model name must be recorded after deployment rather than invented in advance.
