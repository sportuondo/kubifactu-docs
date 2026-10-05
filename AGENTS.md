# KubiFACTU API Docs Agent Guide

## Purpose

This repository is the canonical documentation site for the KubiFACTU API. It is a Mintlify documentation project for the Laravel application in:

`/Users/sendoaportuondo/Developer/qbikode/kubifactu/kubifactu-source`

Treat this docs repository, not the Laravel repository, as the canonical home for API documentation and OpenAPI specifications.

## First Places To Read

- `docs.json`: Mintlify site configuration, navigation, branding, API playground settings, and OpenAPI sources. This project uses `docs.json`, not `mint.json`.
- `api-reference/openapi/openapi.yaml`: canonical OpenAPI spec for the main KubiFACTU API.
- `api-reference/openapi/openapi-kubibai.yaml`: canonical OpenAPI spec for the KubiBAI-compatible endpoints.
- `api-reference/openapi/openapi-common.yaml`: canonical OpenAPI spec for auxiliary/common endpoints.
- Root MDX pages: `introduction.mdx`, `basic-concepts.mdx`, `quickstart.mdx`, `faq.mdx`, `toproduction.mdx`, `kubibai-differences.mdx`, and `changelog.mdx`.
- Manual examples: `api-reference/facturas/examples/*.mdx`.
- Shared snippets: `snippets/*.mdx`, especially `snippets/client-default-headers.mdx`.

The `essentials/` directory and some starter pages are Mintlify template material. Do not treat them as product style or product truth unless the task explicitly concerns Mintlify examples.

## Relationship With The Laravel App

Use the Laravel project to understand runtime behavior, validation, resources, enums, and tests. Do not use any OpenAPI files from the Laravel project as source of truth.

Useful Laravel files:

- `routes/api.php`: API surface and middleware groups.
- `bootstrap/app.php`: middleware aliases and API middleware groups.
- `app/Http/Controllers/Api/**`: endpoint behavior.
- `app/Http/Requests/Api/**`: validation rules and request shape.
- `app/Http/Resources/**`: response shape.
- `app/Models/**`: enum values and domain language.
- `tests/Feature/Http/Controllers/Api/**`, `tests/Feature/Api/**`, and `tests/Feature/Support/**`: expected behavior and edge cases.
- `docs/*.md` in the Laravel repo: internal domain notes, not customer-facing copy.

When behavior and docs disagree, inspect the Laravel code and tests, then update this docs repository. Do not move API documentation back into the source repo.

## Product Context

KubiFACTU is a REST API for generating, sending, querying, correcting, resubmitting, and cancelling Veri*Factu invoice records for AEAT compliance. It also handles QR data, chaining/traceability per SIF and client company, deferred submissions, callbacks, owner-managed client companies, certificates, authorization documents, and identification-document checks.

Important domain terms:

- Prefer `KubiFACTU` for the product name.
- Prefer `Veri*Factu` for the AEAT system.
- Use `AEAT`, `API-KEY`, `SIF`, `registro de facturación`, `subsanación`, `rectificativa`, `anulación`, `empresa cliente`, and `intermediario` consistently.
- Documentation is written in Spanish for API integrators. Keep copy precise, direct, and implementation-oriented.

## API Documentation Rules

- The OpenAPI YAML files in `api-reference/openapi/` are canonical. Keep them internally consistent with the Laravel behavior.
- Keep production and test servers aligned across all specs:
  - `https://api.kubifactu.com`
  - `https://devapi.kubifactu.com`
- Authentication uses API keys in headers:
  - client/company endpoints: `X-Qbikode-ClientApiKey`
  - owner/intermediary endpoints: `X-Qbikode-UserApiKey`
- Apply the right security scheme to each endpoint. Invoice and identification endpoints generally use client auth; client company, SIF management, and Veri*Factu health owner operations generally use owner auth.
- When documenting request fields, check Laravel `FormRequest` classes for validation, normalization, defaults, and conditional rules.
- When documenting responses, check Laravel `JsonResource` classes and response macros.
- When documenting enum values, verify the corresponding PHP enum/model in `app/Models`.
- For examples, prefer realistic Spanish business text and stable placeholder UUIDs/API keys. Do not include real secrets, certificates, customer data, or production identifiers.
- Do not duplicate endpoint behavior in prose when the OpenAPI schema can express it. Use prose for workflow, warnings, regulatory nuance, and integration guidance.

## Mintlify Conventions

- Add pages to `docs.json` navigation when they should be visible.
- Use MDX frontmatter with at least `title`; include `description` for guide pages.
- Use Mintlify components already present in the repo: `CodeGroup`, `Warning`, `Tip`, `Info`, `Note`, and `Update`.
- Reuse snippets instead of copying repeated content, especially HTTP headers.
- Manual invoice examples follow this pattern:
  - import `ClientDefaultHeaders` from `/snippets/client-default-headers.mdx`
  - `## Encabezados HTTP`
  - `<ClientDefaultHeaders />`
  - `## Cuerpo JSON de la petición`
  - `<CodeGroup>` with a JSON example
- OpenAPI-generated endpoint pages are controlled by `docs.json` and the YAML specs. Do not edit generated output directories as if they were source.
- Mintlify supports OpenAPI 3.x and internal `$ref` references inside the same OpenAPI document. Avoid cross-file `$ref` unless Mintlify support has been verified.

## Commands And Verification

This repo has no meaningful local `package.json` scripts; `package-lock.json` is effectively empty. Node is pinned in `.nvmrc`.

This project uses the `mint` CLI (`npm install -g mint@latest`); do not use the legacy `mintlify` command. Useful commands:

```bash
nvm use
mint dev
mint dev --port 3333
mint dev --no-open
mint broken-links
mint validate
```

`mint validate` runs a strict build validation (it exits on warnings or errors) and also checks the OpenAPI specs; `mint openapi-check` is deprecated. `mint` is installed under Herd's nvm, so non-interactive shells may not find it on `PATH`; run it through an interactive shell (`zsh -ic 'mint validate'`).

Before finishing substantive docs changes, validate what is practical in the current environment. At minimum, inspect diffs carefully. For API reference changes, run `mint validate` if the CLI is available.

## Common Pitfalls

- Do not follow README references to `mint.json`; this repo uses `docs.json`.
- Do not use `essentials/` as a model for customer-facing KubiFACTU content.
- Do not rely on stale duplicated OpenAPI files from the Laravel app.
- Watch for inconsistent branding such as `KubiFactu`, `Kubifactu`, `VeriFactu`, or `Verifactu`; normalize intentionally.
- Be careful with OpenAPI paths that include query strings. Standard OpenAPI paths should not embed query parameters in the path; prefer path plus `parameters`.
- Quickstart and examples may contain old local URLs or content types. Prefer public API hosts and `application/json` unless there is a deliberate reason.
- Do not create new dependencies or a local Node project just to run Mintlify unless explicitly asked.

## Working Style

- For non-trivial documentation work, first identify whether the task changes product guides, OpenAPI reference, examples, or all three.
- If the task concerns API behavior, inspect the Laravel source and tests before editing docs.
- If the task concerns Mintlify behavior, consult current Mintlify documentation rather than relying on memory.
- Keep changes narrow and reviewable. Do not clean up unrelated starter files, backups, or branding inconsistencies unless the task asks for that cleanup.
- Prefer concrete examples over abstract explanation, but keep examples legally and technically safe.

## Other

- Use subagents if you think they might be helpful for the task.
- When you need to ask me questions, use `request_user_input` and list the answer you think is best as the first option.