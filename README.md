# BotBot Knowledge Packs

Public distribution point for the Regional Knowledge Packs used by
[BotBot](https://github.com/bpace/botbot), an offline plant-identification app.

This repository holds no application source. It exists so packs can be fetched
without authentication; the app itself is private.

## What a pack is

One read-only SQLite file per region, downloaded on request by the user and never
automatically. Each pack carries, for the taxa of one US state:

- `taxa` — scientific name, family, common names, GBIF taxon key
- `taxon_embeddings` — precomputed BioCLIP ViT-B/16 text-tower vectors, fp16, 512-d
- `knowledge` — description, care, toxicity, each with its provenance
- `occurrences` — coarse ~50 km GBIF occurrence grid
- `attributions` — the notices reproduced below

Toxicity text appears only where a cited source exists. Where there is none the
field is null and the app says so, rather than guessing.

## Available packs

| Region | Pack | Taxa | Size | Release |
|---|---|---:|---:|---|
| Florida | `region-us-fl` | 24 | 104 KB | [`packs-0.1.0`](https://github.com/bpace/botbot-packs/releases/tag/packs-0.1.0) |
| Connecticut | `region-us-ct` | 24 | 108 KB | [`packs-0.2.0`](https://github.com/bpace/botbot-packs/releases/tag/packs-0.2.0) |
| Massachusetts | `region-us-ma` | 24 | 104 KB | [`packs-0.3.0`](https://github.com/bpace/botbot-packs/releases/tag/packs-0.3.0) |
| New York | `region-us-ny` | 24 | 104 KB | [`packs-0.4.0`](https://github.com/bpace/botbot-packs/releases/tag/packs-0.4.0) |
| New Jersey | `region-us-nj` | 24 | 104 KB | [`packs-0.5.0`](https://github.com/bpace/botbot-packs/releases/tag/packs-0.5.0) |
| Vermont | `region-us-vt` | 24 | 104 KB | [`packs-0.6.0`](https://github.com/bpace/botbot-packs/releases/tag/packs-0.6.0) |

Each release asset is verified by SHA-256 on the device before it is installed; a
mismatch aborts the install.

## Attribution

Packs contain material from the sources below, and each pack reproduces these
notices in its own `attributions` table, which the app surfaces under
Settings → Data attributions.

- **USDA NRCS PLANTS Database** — CC0 1.0 — <https://plants.sc.egov.usda.gov/downloads>
  Every Taxon was checked against the relevant USDA NRCS State Plants List.
- **GBIF Backbone Taxonomy** — CC BY 4.0 — <https://doi.org/10.15468/39omei>
  Names, families, and taxon keys were resolved against GBIF.
- **Wikipedia** — CC BY-SA 4.0 — <https://en.wikipedia.org/>
  Available cited descriptions are English Wikipedia extracts.

Wikipedia-derived text is CC BY-SA 4.0, so redistribution of these packs carries
the share-alike obligation.

## Model

Embeddings are produced by the BioCLIP ViT-B/16 text tower (MIT licence) using the
80-template OpenAI ImageNet prompt ensemble. A pack is only compatible with an app
build whose `model_id` and `embedding_dim` match its `pack_meta`.
