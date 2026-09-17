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
| Georgia | `region-us-ga` | 24 | 108 KB | [`packs-0.7.0`](https://github.com/bpace/botbot-packs/releases/tag/packs-0.7.0) |
| Arizona | `region-us-az` | 24 | 104 KB | [`packs-0.8.0`](https://github.com/bpace/botbot-packs/releases/tag/packs-0.8.0) |
| California | `region-us-ca` | 24 | 108 KB | [`packs-0.9.0`](https://github.com/bpace/botbot-packs/releases/tag/packs-0.9.0) |

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

## Licence

The pack files in every release are licensed under
[Creative Commons Attribution-ShareAlike 4.0 International](LICENSE)
(CC BY-SA 4.0). The Wikipedia extracts inside them are CC BY-SA, and share-alike
is the strictest term among the sources, so it governs the whole file. The other
components are compatible with it:

| Component | Source licence | Within the pack |
|---|---|---|
| Taxon names, families, GBIF keys | GBIF Backbone — CC BY 4.0 | CC BY-SA 4.0 |
| USDA symbols and state membership | USDA NRCS PLANTS — CC0 1.0 | CC BY-SA 4.0 |
| Descriptions | English Wikipedia — CC BY-SA 4.0 | CC BY-SA 4.0 |
| Taxon embeddings | Output of BioCLIP (MIT); no licence attaches to model output | CC BY-SA 4.0 |
| Occurrence grid | Derived from GBIF occurrence records; per-record licences (CC0, CC BY, CC BY-NC) are counted in a non-shipping build snapshot | CC BY-SA 4.0 |

If you redistribute a pack, or anything derived from one, keep the `attributions`
table intact and license the result under CC BY-SA 4.0.

## Model and tooling

Embeddings are produced by the BioCLIP ViT-B/16 text tower using the 80-template
OpenAI ImageNet prompt ensemble. A pack is only compatible with an app build whose
`model_id` and `embedding_dim` match its `pack_meta`.

- **BioCLIP** — MIT — <https://huggingface.co/imageomics/bioclip> ·
  <https://github.com/Imageomics/bioclip> ·
  Stevens et al., *BioCLIP: A Vision Foundation Model for the Tree of Life*,
  CVPR 2024, <https://arxiv.org/abs/2311.18803>
- **open_clip** — MIT — <https://github.com/mlfoundations/open_clip> — loads the
  model and supplies the `OPENAI_IMAGENET_TEMPLATES` prompt set
- **OpenAI CLIP** — MIT — <https://github.com/openai/CLIP> — origin of the 80
  ImageNet prompt templates
- **SQLite** — public domain — <https://www.sqlite.org/copyright.html> — pack
  container format
