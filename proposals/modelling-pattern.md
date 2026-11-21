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

### 3. Disambiguate `has part(s)` (P527) with a role qualifier, rather than assuming a brand-new property is the only fix

P527 is confirmed overloaded (gap analysis: P527 backlinks on a compound
mix real pathway membership with plain chemical composition facts like
"ammonia is part of ammonia solution"). The lightest-weight fix found:
qualify the existing P527 statement with `object of statement has role`
(P3831) - e.g. role = `substrate` (Q12119506) or `reaction intermediate`
(Q7458208) - rather than inventing a new base property. This needs no
property proposal at all, since P3831 and the role items already exist;
it's a usage-convention question, not a schema change.

Whether the chain *also* needs a genuinely new property, stated from the
compound's side (matching the project's original chain-table framing of
"natural product -[produced by]-> pathway"), is a larger, harder design
question this project surfaces but deliberately leaves open. See
[`pathway-representation-proposal.md`](pathway-representation-proposal.md)
for the full reflection on both options, and why this project is not
pursuing either as a live submission right now.

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

## Worked example A: caffeine biosynthesis (coffee, tea, cacao) - implemented (Phase 1)

This is the project's own headline motivating example, and gap analysis
confirmed (query 04) that **zero** plant-side caffeine biosynthesis data
existed in Wikidata before this project - the only genuine pathway linked
to caffeine is tagged to archaic *Homo sapiens* (human caffeine
catabolism), and no item for "caffeine biosynthesis" existed at all
(checked via `wbsearchentities`, 2026-11-21 - only scientific articles
about it existed, not a pathway item).

**Phase 1 is now live:** [caffeine biosynthesis (Q142235637)](https://www.wikidata.org/wiki/Q142235637),
created and verified 2026-11-21, using decision 3's lighter-weight option
(`has part(s)` + a role qualifier, P3831) rather than a new relational
property:

```
Item: "caffeine biosynthesis"  [Q142235637 - LIVE]
  instance of -> biosynthetic pathway (Q70696303)
  has part(s) -> xanthosine (Q408788)
    qualifier: object of statement has role -> reaction intermediate (Q7458208)
    reference: stated in -> Q105208193
  has part(s) -> 7-methylxanthine (Q27121426)
    qualifier: object of statement has role -> reaction intermediate
    reference: stated in -> Q105208193
  has part(s) -> theobromine (Q206844)
    qualifier: object of statement has role -> reaction intermediate
    reference: stated in -> Q105208193
  has part(s) -> caffeine (Q60235)
    qualifier: object of statement has role -> product (Q542929)
    reference: stated in -> Q105208193
```

Deliberately **no `found in taxon` qualifier** on these statements: the
compound-level route (xanthosine -> 7-methylxanthine -> theobromine ->
caffeine) is shared plant biochemistry, not something that differs
between coffee, tea and cacao - the project's own "different enzymes per
species" claim lives at the enzyme/gene level, not here. Adding taxon
claims at this stage, without a per-species source for each, would have
been an overclaim.

**Phase 2, not yet done:** the enzyme/gene level is where species
actually diverge, and exactly where the gap analysis found Wikidata
emptiest (zero genuine enzyme-typed pathway participants across all six
species checked). Completing it means sourcing the real methyltransferase
genes per species from the literature (e.g. coffee's XMT/MXMT/DXMT - a
citable source already exists, Q28139580, "7-Methylxanthine
methyltransferase of coffee plants"), checking whether a Wikidata gene
item exists for each (unlikely per the gap analysis), and creating one
with `found in taxon` (P703) and, where available, genomic position,
linked via `encoded by` (P702) to the enzyme - which is where a
per-statement taxon qualifier like the pattern below would actually apply
and be truthful:

```
enzyme (e.g. theobromine synthase, coffee-specific)  [not yet created]
  encoded by -> gene (e.g. CcDXMT1)  [not yet created]
    qualifier: found in taxon -> Coffea arabica (Q47685)
    qualifier: stated in -> [per-species source, e.g. Q28139580 for coffee]
```

This is literature research per species, not Wikidata editing alone, and
remains open - see the project's Call to Action.

## Worked example B: a real PlantMetWiki pathway with no Wikidata counterpart yet

Verified live against PlantMetWiki's SPARQL endpoint (2026-11-21, see
[`pathway-representation-proposal.md`](pathway-representation-proposal.md)):
`PC1001` = "nicotine biosynthesis", one of PlantMetWiki's 1,162 curated
plant metabolic pathways. Searching Wikidata (`wbsearchentities`) finds no
"nicotine biosynthesis" pathway item either - this is a second, concrete,
verified case of the same gap as Example A, not a hypothetical one. If
the PlantMetWiki pathway ID property sketched in that reflection is ever
created, a new "nicotine biosynthesis" item under this pattern would
carry:

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
- **Property-proposal workflow (Week 2):** the workflow itself (propose
  -> discuss -> "ready" -> create) is documented and followed in
  [`pathway-representation-proposal.md`](pathway-representation-proposal.md),
  even though this project has chosen not to actually submit yet.

## What's design-only, and what's now live

Example A's **Phase 1** (the "caffeine biosynthesis" pathway item and its
four compound statements) has been implemented:
[Q142235637](https://www.wikidata.org/wiki/Q142235637), created and
verified 2026-11-21 - this satisfies Deliverable 5's optional pilot, at
the scope described above.

Everything else remains design-only, deliberately not taken further yet:
Example A's **Phase 2** (per-species enzyme/gene statements), Example B's
"nicotine biosynthesis" item, and Example C's GO-glucosinolate edit. This
is a project decision (see
[`pathway-representation-proposal.md`](pathway-representation-proposal.md)'s
status note on the property proposal specifically), not a blocked or
pending step - each would be a separate, later decision.

## What's left

- The compound -> pathway *relational* property itself (as opposed to the
  PlantMetWiki *identifier* property) is still an open property proposal -
  harder to scope than an external ID, and not drafted in this round.
- Verified gene/enzyme identities per species for the caffeine example
  (Phase 2 above) - would need literature sourcing, not done here -
  avoided asserting specific gene names without verification.
- The GO-term/WikiPathways-style "corresponds to" linking convention
  mentioned in Example C needs a short survey of how other Wikidata items
  already do this before standardizing it here.
