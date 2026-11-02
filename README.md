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
  the queries.
- [`proposals/`](proposals/) - drafts for new Wikidata property proposals.
- [`data/`](data/) - crosswalk tables against PlantMetWiki, WikiPathways,
  ChEBI, Plant Ontology, Rhea, EC.

## Status

Started: initial SPARQL queries written and run against the live Wikidata
Query Service; first gap numbers recorded in
[`analysis/gap-analysis.md`](analysis/gap-analysis.md). See that file for
current findings, including confirmed modelling issues (P527 overloading,
inconsistent compound class hierarchy, lack of taxon-specificity in
pathway/gene data).
