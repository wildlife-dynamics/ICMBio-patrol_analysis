# Patrol Analysis Report

**Relatório Bimestral** for Parque Nacional do Iguaçu (ICMBio). It pulls patrol tracks, threat events, camera-trap deployments, and photographic records from EarthRanger for a reporting period, and produces both an interactive dashboard and a Word (`.docx`) report.

## Dashboard

**Stat cards**

| Card | Shows |
|---|---|
| Número de patrulhas | Count of distinct patrols in the period |
| Quilômetros percorridos | Total distance covered across all patrol tracks |
| Participantes/operadores | Count of distinct patrol leaders (see [Known limitations](#known-limitations)) |

**Maps** — each gets its own full-width row

| Map | Shows |
|---|---|
| Mapa de trilhas de patrulha | Patrol tracks, colored by patrol type (or ID) |
| Mapa de densidade de tempo | Time-density heatmap of patrol coverage |
| Mapa de eventos | Event locations, colored by event type (or category/ID) |

Both maps optionally overlay an EarthRanger spatial feature layer and/or a locally-uploaded GeoJSON/GeoPackage/GeoParquet file (**Spatial Features** / **Local Spatial Features**).

**Tables** — sortable, filterable, downloadable

| Table | Contents |
|---|---|
| Patrulhas incluídas | One row per patrol: início, fim, distância, tempo, tipo |
| Eventos observados | Event counts by type |
| Ameaças | Threat event details (caça, pesca ilegal, exploração vegetal, outras ameaças) |
| Atropelamentos | Roadkill event details (weather, location, monitoring method, team) |
| Armadilhas fotográficas | Camera-trap events: sector, date, event type, responsável, coordinates |
| Resumo por setor | Event counts and centroid per park sector |

**Chart**

| Chart | Shows |
|---|---|
| Eventos de patrulha por mês | Monthly event counts, stacked by event type |

## Event types

| Event type slug | Used for |
|---|---|
| `caceria`, `pesca_ilegal`, `explorao_vegetal`, `outras_ameaas` | Ameaças table |
| `atropelamento_v2` | Atropelamentos table (has its own distinct set of fields — not merged into Ameaças) |
| `armadilhas_fotogrficas`, `retirada_armadilhas_fotogrficas` | Armadilhas fotográficas table |

## Report (.docx)

`Patrol Report` renders the dashboard's content into the Relatório Bimestral template (`resources/templates/relatorio_bimestral_template.docx`, built by `scripts/build_relatorio_bimestral_template.py`). A period with zero events of a given type (e.g. no roadkill incidents) renders that section empty rather than failing the whole report. Photo attachments can be skipped entirely via **Skip Photo Attachments**, if a faster run without images is preferred.

## Known limitations

- **Participantes/operadores** currently counts distinct patrol leaders — EarthRanger has no separate support-crew ("equipe de apoio") list, so this may need to become manually editable (open PRD question, see the `TODO` above `total_participants` in `spec.yaml`).
- **Map Viewport** (advanced) controls how tightly every map zooms and frames its data — adjust it if maps consistently look too zoomed in or out for how they're actually displayed.

## Requirements

[pixi](https://pixi.sh) is required for environment and dependency management. You will also need an EarthRanger connection configured for the `parnaiguacu` data source.

## Getting started

```bash
make compile   # builds the ecoscope-workflows-ext-icmbio task package and compiles spec.yaml
make run       # runs the workflow against param.yaml (real EarthRanger data, no mock IO)
make open      # opens the generated HTML outputs in your browser
```

Run `make help` to see every available target (`recompile`, `refresh`, `clean`, `all`).

Results (dashboard HTML/PNG, the `.docx` report, and `result.json`) are written to `/tmp/icmbio-patrol-analysis/output` by default — override with `OUTPUT_DIR=...`.

## Repo layout

| Path | Purpose |
|---|---|
| `spec.yaml` | The DAG — hand-authored, compiled via `wt-compiler` |
| `param.yaml` | Default run configuration (real EarthRanger data source, time range, filters) |
| `layout.json` | Dashboard widget grid |
| `metadata.yaml` | Workflow name/author/description |
| `test-cases.yaml` | Named test case(s) against real ICMBio park data |
| `dev/` | `recompile.sh` (full compile) and `regenerate_rjsf.sh` (fast, schema-only regen) |
| `resources/templates/` | The `.docx` report template |
| `scripts/` | `build_relatorio_bimestral_template.py` — builds the template from the original mockup |
| `ecoscope-workflows-patrol-analysis-workflow/` | Compiled workflow package (generated — never hand-edit) |
