# Modelling pattern: compound -> pathway -> enzyme -> gene -> genome

Deliverable 3 ("a recommended modelling pattern, with worked examples, for
linking natural products to pathways, enzymes, genes and genomes"), with
Deliverable 5's optional pilot folded in as worked examples (design-level
only - see "What's a design, not an edit" at the end).

Grounded in the findings in [`../analysis/gap-analysis.md`](../analysis/gap-analysis.md)
and in concepts from the Ontology Course (`../slides_course/`), cited
inline below.

## The five design decisions

### 1. One pathway item per biosynthetic route; qualify the *link*, not the pathway

Don't create a separate "caffeine biosynthesis in coffee" /
"...in tea" / "...in cacao" item. Keep one "caffeine biosynthesis"
pathway item and put `found in taxon` (P703) on each compound/gene
*participation statement*, not on the pathway entity itself. This is
exactly how Objective 4 already specifies the qualifier ("found in taxon
as a qualifier, to state in which organism a compound-pathway... link
holds"), and it avoids the combinatorial explosion of near-duplicate
pathway items the course material warns about when deciding "where to
state" information (Week 3).

### 2. Type new pathway items as `instance of` "biosynthetic pathway" (Q70696303), not the generic "biological pathway" (Q4915012)

Found during this project's gap analysis: Q70696303 "biosynthetic
pathway" already exists, is correctly `subclass of` "biological pathway"
and "metabolic pathway" - but has **zero** items using it. Every real
pathway item (including every genuine WikiPathways-sourced one found in
queries 02-07) uses the generic parent class directly. This is a direct
instance of the course's Week 1/2 point that instance-of/subclass-of
choices matter and the Week 6 "lack of guidance for using
classes/properties" problem category: a more precise class sits unused
because nothing tells editors it's there. Using it going forward costs
nothing and, for the first time, makes "give me biosynthesis pathways
specifically" a directly answerable SPARQL query instead of needing the
indirect P2410/P527 heuristics this project's own queries had to build.

### 3. Don't put a bare compound directly behind `has part(s)` (P527) as the compound -> pathway link

P527 is confirmed overloaded (gap analysis: P527 backlinks on a compound
mix real pathway membership with plain chemical composition facts like
"ammonia is part of ammonia solution"). Reserve P527 for what WikiPathways
pathways already use it for (listing a pathway's reaction
participants/parts). The compound -> pathway *biosynthesis* link should
use the new property proposed in Objective 5 (see
[`property-proposal-plantmetwiki-id.md`](property-proposal-plantmetwiki-id.md)
for the identifier side; the relational compound -> pathway property
itself is the larger, harder property proposal this project flags as
future work rather than resolves here - see "What's left" below).

### 4. Qualifier set for every organism-specific participation statement

| Qualifier | Property | Required? |
|---|---|---|
| Organism | `found in taxon` (P703) | Required |
| Tissue/organ | `anatomical location` (P927) | Optional, when known |
| Evidence type | `determination method or standard` (P459) | Optional, e.g. to mark experimentally-validated vs predicted |
| Source | `stated in` (P248) | Required |

Gap analysis found **zero** existing qualifier usage on the relevant
statements (query 05) - this pattern isn't competing with inconsistent
prior practice, it's the first qualifier convention for this part of the
graph.

### 5. Enzyme -> gene and gene -> genome: reuse existing properties; this is a data gap, not a modelling gap

`encoded by` (P702) for enzyme -> gene, and `genomic start`/`genomic end`
(P644/P645) qualified by `genomic assembly` (P659) for gene -> genome are
already well-formed, established properties (used extensively for human
and model-animal genes). No new property is needed for these two legs.
What's missing is data: queries 06-07 found 0/28 Arabidopsis genes with a
genomic position, 0 genes at all for the other 5 species checked, and
effectively zero genuine enzyme-typed pathway participants. Filling this
is plain data entry once sources exist, not a design problem.

## Worked example A: caffeine biosynthesis (coffee, tea, cacao)

This is the project's own headline motivating example, and gap analysis
confirmed (query 04) that **zero** plant-side caffeine biosynthesis data
exists in Wikidata today - the only genuine pathway linked to caffeine is
tagged to archaic *Homo sapiens* (human caffeine catabolism), and no item
for "caffeine biosynthesis" exists at all (checked via `wbsearchentities`,
2026-11-21 - only scientific articles about it exist, not a pathway item).

Illustrative statements under this pattern (none of this has been created
on Wikidata - see "What's a design, not an edit" below):

```
Item: "caffeine biosynthesis"  [NEW ITEM]
  instance of -> biosynthetic pathway (Q70696303)

Item: theobromine (Q206844, existing compound)
  [new property from Objective 5] participates in -> caffeine biosynthesis
    qualifier: found in taxon -> Coffea arabica (Q47685)
    qualifier: stated in -> [source publication per species]

  [same statement, different taxon qualifier - same pathway item, same
   compound, different organism and potentially different enzyme/gene
   downstream]
  [new property] participates in -> caffeine biosynthesis
    qualifier: found in taxon -> Camellia sinensis (Q101815)
    qualifier: stated in -> [source publication]

  [new property] participates in -> caffeine biosynthesis
    qualifier: found in taxon -> Theobroma cacao (Q42385)
    qualifier: stated in -> [source publication]
```

The point this example makes concrete: the *compound* and *pathway* nodes
are shared across all three species (correctly - it's the same
biosynthetic route conceptually), but each species' *participation
statement* carries its own taxon qualifier and, at the enzyme/gene level
(not shown - would need verified gene identities per species, which this
project has not sourced), would diverge per the project's own claim that
different enzymes catalyze this in each plant.

## Worked example B: a real PlantMetWiki pathway with no Wikidata counterpart yet

Verified live against PlantMetWiki's SPARQL endpoint (2026-11-21, see
[`property-proposal-plantmetwiki-id.md`](property-proposal-plantmetwiki-id.md)):
`PC1001` = "nicotine biosynthesis", one of PlantMetWiki's 1,162 curated
plant metabolic pathways. Searching Wikidata (`wbsearchentities`) finds no
"nicotine biosynthesis" pathway item either - this is a second, concrete,
verified case of the same gap as Example A, not a hypothetical one. Once
the PlantMetWiki ID property (Objective 5) exists, a new "nicotine
biosynthesis" item under this pattern would carry:

```
Item: "nicotine biosynthesis"  [NEW ITEM]
  instance of -> biosynthetic pathway (Q70696303)
  PlantMetWiki pathway ID -> PC1001
```

with compound/enzyme/gene participation statements following the same
qualifier pattern as Example A.

## Worked example C: glucosinolate biosynthesis - a modelling collision, not a clean slate

Unlike A and B, an item already exists here: "glucosinolate biosynthetic
process" (Q22274797), imported from Gene Ontology (GO:0019761). But it's
`instance of` "biological process" - a different class entirely from the
WikiPathways-style "biological pathway" convention - has `has part(s)` to
the compound (glucosinolate) but no taxon, no genes, no enzymes. This is
the two-pathway-conventions collision documented in the gap analysis.

**Recommendation for this case specifically:** don't retrofit the GO item
(it correctly represents the generic, organism-independent biological
process GO intends). Instead, create a separate, WikiPathways-style
"glucosinolate biosynthesis (Arabidopsis thaliana)" item typed `instance
of biosynthetic pathway` and linked to the GO item via a suitable
"facet of" / corresponds-to relation (existing Wikidata practice for
linking GO process terms to specific modelled pathways - needs a short
look at how other WikiPathways items already do this, not resolved here),
rather than inventing a third convention. This keeps rule 1 above
("qualify the link, not the pathway") compatible with GO's genuinely
organism-independent process terms.

## Connections to the course material (Week references)

- **Instance-of vs subclass-of discipline (Weeks 1-2):** decision 2 above
  (use the specific unused subclass) and the Example C collision are both
  direct applications.
- **"Where to state information" / avoiding combinatorial explosion
  (Week 3):** decision 1 (qualify the link, don't fork the pathway item).
- **Chemistry WikiProject's 2024 instance->subclass migration, "still
  messed up" for genes/proteins (Week 5):** precedent that this exact kind
  of class-assignment fix (decision 2) is both normal practice and still
  incomplete elsewhere in Wikidata - this project's fix is small, not
  unusual.
- **Mereology/"has part" flagged as unfinished (Week 7):** decision 3
  (stop overloading P527) continues this acknowledged open problem.
- **Property-proposal workflow (Week 2):** followed directly in
  [`property-proposal-plantmetwiki-id.md`](property-proposal-plantmetwiki-id.md).

## What's a design, not an edit

Nothing in this document has been written to live Wikidata. Creating the
"caffeine biosynthesis" or "nicotine biosynthesis" items, adding the new
compound -> pathway property's statements, or editing the GO glucosinolate
item are all real, public, permanent actions on a shared knowledge base -
appropriately done deliberately, under a project participant's own
account, likely after the property proposal (Deliverable 4) is actually
approved (the new property doesn't exist yet, so Examples A and B's
`participates in` statements can't be created as shown until it does).
This document satisfies Deliverable 3 (the pattern + worked examples) and
sketches what Deliverable 5's optional pilot would look like; actually
carrying out Deliverable 5 is a follow-on action, not completed here.

## What's left

- The compound -> pathway *relational* property itself (as opposed to the
  PlantMetWiki *identifier* property) is still an open property proposal -
  harder to scope than an external ID, and not drafted in this round.
- Verified gene/enzyme identities per species for the caffeine example
  (would need literature sourcing, not done here - avoided asserting
  specific gene names without verification).
- The GO-term/WikiPathways-style "corresponds to" linking convention
  mentioned in Example C needs a short survey of how other Wikidata items
  already do this before standardizing it here.
