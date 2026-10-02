# E-invoicing backend

Express and MongoDB backend for a 2024 UNSW team capstone. Raghav Agarwal implemented the application. This repository is historical project source; current production availability is not asserted.

## Implemented structure

PDF upload → Veryfi extraction → JSON-to-UBL XML conversion → MongoDB/GridFS storage. A separate validation path calls an external UBL validator and builds reports. Routes also cover authentication, profile updates, file sharing and email history.

- `backend/pdfConverter/`: extraction orchestration, UBL construction and conversion records.
- `backend/ublValidator/`: external validation and result retrieval.
- `backend/auth/` and `backend/middleware/`: login, registration, reset and authentication checks.
- `backend/shared/`: reporting, access sharing and email helpers.
- `backend/tests/`: five Jest test files covering authentication, password resets, file retrieval and email history.

These folders demonstrate implementation scope. Their presence does not establish security certification, universal invoice compatibility or measured coverage.

## Local setup

Use a supported Node.js release and a disposable MongoDB database. Install from the backend folder:

```sh
cd backend
npm ci --ignore-scripts
```

Create an untracked `.env` from `.env.example`, supplying your own local values. `MONGO_URI`, `TOKEN_KEY` and a port are required for core configuration. Extraction, validation and email paths additionally require provider credentials. No historical credentials are supplied.

`server.js` currently hardcodes the historical hosted frontend origin in two CORS locations. Password-reset URLs also target that frontend. Configure these deliberately for your local origin before using a browser client. Do not point tests at a live database.

```sh
npm start
npm run lint
npm test -- --runInBand
npm run coverage -- --runInBand
```

The scripts above exist in `package.json`. Integration tests import the database connector and create/delete records; they require a disposable test database and safe mail/provider configuration. They were not run against historical services during documentation review. Coverage thresholds are configuration, not measured coverage. A Dockerfile exists; deployment configuration is historical and is not a verified live demo.

## Engineering limitations and next work

- Isolate external services and database configuration so tests run reproducibly without live integrations.
- Add focused fixtures for invalid PDFs, incomplete extraction and malformed UBL.
- Verify access control on every document route and validate ownership server-side.
- Standardise environment configuration and remove deployment-specific URLs.
- Publish actual test/coverage output only after a clean, isolated run.

A sanitised case study can describe the architecture without distributing the original capstone report or credentials.
