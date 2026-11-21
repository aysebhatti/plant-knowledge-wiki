# Property proposal draft: PlantMetWiki pathway ID

**Status:** draft, not yet submitted to Wikidata. Posting a property
proposal is a real, public community action tied to a Wikidata account -
this file is a complete, ready-to-use draft, written so it can be pasted
onto [Wikidata:Property proposal](https://www.wikidata.org/wiki/Wikidata:Property_proposal)
by whoever submits it, but submission itself is a deliberate separate step
(see "Before submitting" at the end).

This fulfills Deliverable 4 ("at least one formal property proposal with
full justification, examples and a community discussion") and follows the
property-proposal workflow covered in the Ontology Course (Week 2):
propose -> discuss (~1 week) -> "ready" -> property created.

## Why this property, and not a "LOTUS ID"

The course reviewer (Egezort) suggested starting with a LOTUS external-ID
property, since external identifiers are generally easier to get approved
than relational properties. Investigating that (2026-11-21, see
[`../analysis/gap-analysis.md`](../analysis/gap-analysis.md#property-proposal-candidate-lotus-id-doesnt-work-plantmetwiki-id-does)):
**LOTUS doesn't have one.** LOTUS does "knowledge management *through*
Wikidata" - its structure-organism pair data lives directly in Wikidata
as `found in taxon` (P703) statements, not in a separate database with
its own record IDs. There is nothing external for a "LOTUS ID" property
to point to.

PlantMetWiki, the *other* candidate Objective 5 itself names, does have
one, and it's verified against the live endpoint, not assumed:

- Live SPARQL endpoint: `https://plantmetwiki.bioinformatics.nl/sparql`
- 1,162 distinct pathway items carry a stable `PC<number>` identifier
  (e.g. `PC1001`), matching the "1,162 plant metabolic pathways" figure
  from the PlantMetWiki preprint (Del Pup et al., 2026).
- `https://plantmetwiki.bioinformatics.nl/pathways/PC1000` resolves
  (HTTP 303 to a live record view) - confirming this is a stable,
  dereferenceable identifier, not an artifact of one data release.
- Checked Wikidata (`wbsearchentities`, type=property): no "PlantMetWiki"
  or "PlantCyc" property exists yet.

## Proposal (Wikidata property-proposal template)

**Property:** PlantMetWiki pathway ID

**Description:** identifier for a plant metabolic pathway in PlantMetWiki,
a FAIR knowledge graph of plant metabolic pathways built on WikiPathways
infrastructure (PlantCyc-derived)

**Data type:** External identifier

**Domain:** items about metabolic/biosynthetic pathways - specifically,
items that are `instance of` "biological pathway" (Q4915012) or, per this
project's recommended practice (see
[`modelling-pattern.md`](modelling-pattern.md)), the more specific
"biosynthetic pathway" (Q70696303)

**Allowed values:** `PC[0-9]+` (e.g. `PC1001`)

**Example 1:**
- Item: nicotine biosynthesis [no Wikidata item currently exists for
  this pathway - see the modelling pattern's worked example B]
- Value: `PC1001`

**Example 2 (to be filled in with an existing Wikidata pathway item once
one is identified that corresponds to a PlantMetWiki PC-numbered pathway -
not yet cross-matched; see "Before submitting" below)**

**Source:** [PlantMetWiki](https://plantmetwiki.bioinformatics.nl/)
(Del Pup E, et al. (2026). PlantMetWiki: a FAIR knowledge graph for plant
metabolic pathway cross-species representation and integration. bioRxiv.
doi:10.64898/2026.07.22.733699)

**Formatter URL:** `https://plantmetwiki.bioinformatics.nl/pathways/$1`

**Planned use:** link Wikidata pathway items (both existing WikiPathways-
sourced ones and new biosynthesis-pathway items created under this
project's modelling pattern, e.g. for caffeine or glucosinolate
biosynthesis) to their PlantMetWiki counterpart. This directly supports
Objective 2's crosswalk and enables federated SPARQL queries between
Wikidata and PlantMetWiki (as the original project proposal's Methods
section describes) without relying on label-string matching.

**Motivation:** this project's gap analysis
([`../analysis/gap-analysis.md`](../analysis/gap-analysis.md)) found that
genuine pathway coverage for plant natural products stays under ~24% even
using Wikidata's best current (overloaded) property, `has part(s)`
(P527), and that the compound -> pathway -> enzyme -> gene -> genome chain
is sparse to empty across every one of the 6 species checked. PlantMetWiki
already holds 1,162 curated plant metabolic pathways that could fill much
of this gap; a dedicated identifier property is a low-friction first step
toward linking the two resources, ahead of (and independent from) the
larger, harder relational-property question (compound -> pathway) that
Objective 5 also raises.

**See also:** WikiPathways ID (P2410) - the property PlantMetWiki's own
pathway infrastructure is modelled after, and the closest existing analog.

## Before submitting

This draft is ready to paste onto Wikidata:Property proposal, but three
things should happen first, ideally by the project team together:

1. **Decide who submits it.** Property proposals are tied to the
   submitter's Wikidata account and reputation; this should go out under
   a project participant's own account, not be posted automatically.
2. **Find at least one existing Wikidata item that already corresponds to
   a PlantMetWiki PC-numbered pathway**, to use as "Example 2" (a real,
   already-existing Wikidata item, not just the hypothetical "nicotine
   biosynthesis" item from Example 1). This likely needs a short
   label-matching pass between PlantMetWiki's ~1,162 pathway names and
   Wikidata's WikiPathways-sourced pathway items.
3. **Check with PlantMetWiki's maintainers** (the project's own
   co-authors) that `PC<number>` is the identifier they want Wikidata to
   treat as canonical and stable long-term, and that the formatter URL
   above is the one they want used - since you're also a PlantMetWiki
   author, this is a short conversation, not a cold outreach.
