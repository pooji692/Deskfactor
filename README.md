# AI Clinical Document Reviewer

This submission contains a self-contained browser preview of the requested AI Clinical Document Reviewer workflow. Open `index.html` directly to review the UI, load the synthetic sample note, run an analysis, inspect structured sections, and exercise report history actions.

Documentation:

- [Architecture diagram](docs/architecture.md)
- [AI/ML design](docs/AI_ML_Design.md)
- [Technical decisions](docs/Technical_Decisions.md)

The attached `CODEX_BUILD_SPEC.md` was treated as the product specification. Its “Items the human must do” section remains a human deployment checklist: an AI provider key, GitHub repository, Vercel project, Render service, screenshots, and final live-AI review require the owner’s accounts and approval.

## Preview

The standalone preview uses deterministic synthetic data so it never sends note contents or files to a service. It demonstrates the requested review layout, disclaimer, empty/loading/success states, sample loading, file selection state, flagged sections, and report history.

## Runtime note

This environment did not contain Node.js, npm, or a deploy connector, so the full React + Vite / Express runtime could not be installed, started, tested, or deployed honestly from this session. No public URL is claimed. The preview is the verified submission artifact available here.

## Deployment checklist

1. Install Node 20+.
2. Implement or restore the React frontend and Express backend from the supplied build spec.
3. Run backend and frontend tests with `npm test` in each package.
4. Deploy `frontend` to Vercel and `backend` to Render with a persistent disk and the environment variables from the spec.
5. Replace the placeholder deployment URLs in the project README after checking health, text, PDF, image, refresh/history, unsupported-file, and CORS flows.

Synthetic-data disclaimer: AI output may contain errors and must be verified by a qualified healthcare professional.
