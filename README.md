# plant-knowledge-wiki
Plant Knowledge Wiki: Linking LOTUS Compound-Taxon Data to Plant Biosynthetic
Pathways in Wikidata.

This project is being developed as part of the [Wikidata WikiProject Ontology Course - Plant Knowledge Wiki](https://www.wikidata.org/wiki/Wikidata:WikiProject_Ontology/Ontology_Course/Projects_Offering_3/Plant_Knowledge_Wiki), which is part of the project of the Third offering of the course.

## Why

LOTUS has added many referenced compound-taxon links to Wikidata (`found in
taxon`, P703), but the biosynthetic context - which pathway, which enzyme,
which gene, in which genome - is mostly missing or inconsistent, especially
for plants. Biosynthesis is also taxon-specific (e.g. caffeine is made by
different enzymes in coffee, tea, and cacao), so a useful model needs to
carry organism (and ideally tissue/evidence) context, not just a bare link.

The chain this project is trying to fill in:

| Link | Property | Status |
|---|---|---|
| compound -> taxon | `found in taxon` (P703) | covered by LOTUS |
| compound -> pathway | - | missing/indirect |
| pathway -> enzyme | `has part(s)` (P527) | sparse for plants |
| enzyme -> gene | `encoded by` (P702) | limited for plants |
| gene -> species | P703 | not stated |
| gene -> genome | P644/P645 + P659 | rare for plants |

## Repository layout

- [`queries/`](queries/) - reusable SPARQL queries (coverage/gap queries,
  pilot queries) for the Wikidata Query Service.
- [`analysis/`](analysis/) - gap analysis write-up and findings from running
  the queries, plus a [course-connections summary](analysis/course-connections-summary.md)
  tying findings to specific weeks of the Ontology Course.
- [`notebooks/`](notebooks/) - Jupyter notebook(s) that re-run the queries
  and chart the results (visualizations, added from course reviewer
  feedback). See [Running the notebooks](#running-the-notebooks) below.
- [`proposals/`](proposals/) - the [modelling pattern](proposals/modelling-pattern.md)
  (with worked examples) and a
  [chain-representation reflection](proposals/pathway-representation-proposal.md)
  (a PlantMetWiki pathway ID property sketch, plus a lighter-weight
  role-qualifier option - not submitted or implemented; see that file's
  status note).
- [`data/`](data/) - crosswalk tables against PlantMetWiki, WikiPathways,
  ChEBI, Plant Ontology, Rhea, EC (not started yet).

## Running the notebooks

```
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter nbconvert --to notebook --execute --inplace notebooks/gap_visualizations.ipynb
# or: jupyter notebook notebooks/gap_visualizations.ipynb
```

The notebook re-runs the `.rq` files in `queries/` live against the
Wikidata Query Service, so its charts reflect current data, not a cached
snapshot.

## Status

All five proposal deliverables (plus the reviewer-suggested
visualizations) have a first draft:

1. **SPARQL queries** - 7 queries in [`queries/`](queries/) covering the
   full compound -> pathway -> enzyme -> gene -> genome chain across 6
   species.
2. **Gap analysis report** - [`analysis/gap-analysis.md`](analysis/gap-analysis.md):
   coverage figures, confirmed modelling issues (P527 overloading, two
   incompatible pathway conventions, zero qualifier usage, non-taxon-
   specific pathway/gene data), and an empirically-confirmed case study
   (caffeine in coffee/tea/cacao).
3. **Modelling pattern** - [`proposals/modelling-pattern.md`](proposals/modelling-pattern.md):
   5 design decisions and 3 worked examples. Example A's Phase 1 is now
   **live**: [caffeine biosynthesis (Q142235637)](https://www.wikidata.org/wiki/Q142235637) -
   see that file's "What's design-only, and what's now live" section.
4. **Property proposal / chain-representation reflection** -
   [`proposals/pathway-representation-proposal.md`](proposals/pathway-representation-proposal.md):
   a PlantMetWiki pathway ID property sketch, plus a lighter-weight
   role-qualifier alternative that needs no new property at all. By
   project decision, **not submitted** to Wikidata:Property proposal and
   no community discussion has been opened.
5. **Pilot** (optional) - **Phase 1 implemented** (the caffeine
   biosynthesis pathway item above). Phase 2 (per-species enzyme/gene
   data) remains open - see the modelling pattern document and the
   project's Call to Action.
6. **Visualizations** (added from reviewer feedback) - [`notebooks/gap_visualizations.ipynb`](notebooks/gap_visualizations.ipynb).

Still open: the crosswalk to PlantMetWiki/ChEBI/Plant Ontology/Rhea/EC
(Objective 2, [`data/`](data/) is still empty) and federated queries
against PlantMetWiki/WikiPathways directly (as opposed to querying
Wikidata's local copies of their IDs).
