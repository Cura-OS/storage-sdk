# @curaos/storage-sdk

Typed client for `storage-service` - REST operations and event wire-types,
**generated from the service's contracts**. No hand-written HTTP or Kafka
plumbing; the SDK is the only client code a consumer needs.

- REST client + request/response types ← `storage-service/specs/storage.tsp`
  (TypeSpec → OpenAPI 3.1 → [`@hey-api/openapi-ts`](https://heyapi.dev)).
- Event payload + header types ← `storage-service/specs/storage.asyncapi.yaml`
  (AsyncAPI 3.0.0 → `@asyncapi/parser` → `json-schema-to-typescript`).

> Scaffolded by `bun run gen:sdk storage` (the reusable SDK recipe, #308). Run
> `bun run generate` to fill `src/rest/**` + `src/events.gen.ts` from the
> contracts, then wire the service-specific re-exports in `src/index.ts` and the
> assertions in `test/smoke.test.ts`.

## Installation

```sh
bun add @curaos/storage-sdk
```

The package publishes to the CuraOS Verdaccio registry; `.npmrc` already scopes
`@curaos:registry=http://localhost:4873`.

## Regenerating from the contract

One command re-runs the whole chain (service spec compile → REST client → event
types) from the committed contracts:

```sh
bun run generate
```

The generated output under `src/` is committed and guarded: `test/drift.test.ts`
fails if the committed SDK is not byte-identical to a fresh regeneration - a
contract change that was not re-run through `bun run generate`, or a generator
version bump, is caught in CI.

## Commands

```sh
bunx turbo run typecheck --filter=@curaos/storage-sdk
bunx turbo run test      --filter=@curaos/storage-sdk
bunx turbo run build     --filter=@curaos/storage-sdk
```
