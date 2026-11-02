# Gap analysis

Working notes for Objectives 1 (domain survey) and 3 (gap & issue analysis).
Each entry records a concrete, re-run-able finding from the Wikidata Query
Service, not a general claim. Source queries live in [`../queries/`](../queries/).

## Coverage: compound -> pathway (Arabidopsis thaliana, Q158695)

Query: [`02_coverage_lotus_compound_to_pathway_gap.rq`](../queries/02_coverage_lotus_compound_to_pathway_gap.rq)
Run 2026-11-02.

| Metric | Count | % of total |
|---|---|---|
| Compounds found in taxon (P703) | 1284 | 100% |
| ...with *any* P527 ("has part(s)") backlink | 225 | 17.5% |
| ...with a backlink to a genuine WikiPathways-sourced pathway (P527 + P2410) | 118 | 9.2% |

**Reading:** even using the most generous existing property (P527, not a
dedicated compound->pathway property), over 90% of compounds known to occur
in Arabidopsis have no pathway context in Wikidata at all. This is the
baseline gap number for Objective 5's property-proposal justification.

## Modelling issue: P527 ("has part(s)") is overloaded

Query: [`01_arabidopsis_lotus_pathway_gene.rq`](../queries/01_arabidopsis_lotus_pathway_gene.rq)

Dropping the P2410 filter and querying P527 backlinks on a compound returns
a mix of:
- genuine metabolic pathways (e.g. *TCA cycle*, Q27436670)
- plain composition/part-whole statements with no biosynthetic meaning at
  all (e.g. *ammonia* is "part of" *ammonium cation*, *ammonia solution*,
  *metal ammine complex*)

P527 is being used both for "this pathway has this metabolite as a step"
and for ordinary chemical part-whole relationships. Any coverage query or
modelling pattern that uses P527 as the compound->pathway link needs the
P2410 (WikiPathways ID) guard (or an equivalent instance-of-pathway check)
to avoid counting noise as coverage.

## Modelling issue: natural product class hierarchy is inconsistent

*Ammonia* (Q4087), a compound returned by a plain `found in taxon` query,
is `instance of` "type of chemical entity" (Q113145171) - not "chemical
compound" (Q11173) or any single consistent compound class. A query or
pattern that assumes a uniform `P31` value for LOTUS-sourced compounds will
silently drop rows. Needs a wider survey across more taxa to see how
widespread this is (candidate for Objective 1).

## Modelling issue: pathway data is not taxon-specific

For the genuine WikiPathways pathways linked to Arabidopsis compounds
(e.g. WP78 "TCA cycle", WP497 "urea cycle and metabolism of amino groups",
WP716 "Vitamin A and carotenoid metabolism"), the genes attached via P527
are human gene symbols (SUCLA2, SDHA, OGDH, SDHB, SDHC, SDHD, DLD, DLST,
IDH2/3A/3B/3G, MDH2, FH, CS, ACO2, SUCLG1/2, ...), not Arabidopsis genes.

Restricting the gene side of the query to genes with `found in taxon`
(P703) = Arabidopsis thaliana returns **zero rows** (confirmed by running
query 01 - 200/200 result rows, 0 with `?gene` bound).

**Reading:** this is a direct, queryable demonstration of the project's
central motivation. A compound's "found in taxon" link and a pathway's
"has part" link to a gene currently compose into a false implication -
that this plant's biosynthesis uses that (human) gene - because neither
link carries taxon-qualification on the pathway/gene side. This is exactly
the gap the qualifier pattern (Objective 4) and the caffeine example on
the project page are about: biosynthesis is taxon-specific, and Wikidata's
current pathway/gene data mostly isn't.

## Still open / not yet checked

- `stated in` (P248) does **not** point to Q104225190 ("LOTUS, natural
  product database") on any P703 statement, and `based on heuristic`
  (P887) doesn't either (both checked, 0 results). There is currently no
  found way to query "which P703 statements came specifically from the
  LOTUS import" versus any other source of the same property. Revisit
  during the full domain survey (Objective 1) - may need to check edit
  history / dump metadata rather than live statement RDF.
- Crosswalk to PlantMetWiki, ChEBI, Plant Ontology, Rhea, EC (Objective 2) -
  not started.
- Federated queries against PlantMetWiki/WikiPathways (as opposed to just
  querying Wikidata's local copies of WikiPathways IDs) - not started.
- This analysis only covers one taxon (Arabidopsis thaliana) so far. Numbers
  will shift for other species; worth re-running for a small taxon sample
  before quoting gap percentages anywhere final (e.g. the property proposal).
