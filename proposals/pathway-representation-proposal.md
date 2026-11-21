# Representing the compound -> pathway -> enzyme -> gene -> genome chain in Wikidata

**Status:** a design reflection, not a submission. Nothing here has been
posted to [Wikidata:Property proposal](https://www.wikidata.org/wiki/Wikidata:Property_proposal),
and no live Wikidata edits have been made. This is a project decision,
not a pending TODO: the aim of this document is to leave a clear,
well-reasoned trail for whoever picks this up next (possibly this project
continuing, possibly another course participant), not to take the action
itself.

This document fulfills Deliverable 4's *design* half ("at least one
formal property proposal with full justification [and] examples") while
deliberately not pursuing its *process* half (the open community
discussion), by the project's own choice.

## The problem, restated

Every stage of the chain `compound -> pathway -> enzyme -> gene -> genome`
was measured live across 6 species
([`../analysis/gap-analysis.md`](../analysis/gap-analysis.md), queries
02-07). The result: the chain is sparse to empty for every species
checked, with no exception found. Concretely:

- Genuine pathway coverage for a compound found in a species: 4-24%.
- Qualifier usage (taxon, tissue, evidence) on the pathway-compound link:
  **zero**, for every statement checked.
- Genes typed and taxon-matched to these 6 species: 28 total, all in one
  species, none with a genomic position.
- Enzyme-typed items inside genuine pathways reachable from these
  species' compounds: **one**, unrelated to plant biosynthesis.
- The caffeine case study (the project's own motivating example) traces
  this concretely: the compound step is solid for coffee, tea and cacao;
  every step after it is empty for all three, with the one pathway item
  Wikidata does link to caffeine tagged to a human taxon instead.

`has part(s)` (P527), the only property currently usable for the
compound-pathway link at all, is also **overloaded**: it mixes genuine
pathway membership with plain chemical composition facts (e.g. "ammonia
is part of ammonia solution").

## Two different needs, often conflated

### 1. External identifiers - grounding Wikidata pathway items in a maintained source

Linking a Wikidata pathway item to its record in an actively-maintained
pathway database (PlantMetWiki, and potentially others) via a dedicated
external-identifier property. This supports Objective 2's crosswalk and
federated queries, and is the more straightforward, lower-friction kind
of property proposal - but **on its own it does not change how the chain
is represented inside Wikidata.** PlantMetWiki could be perfectly
cross-linked and the chain inside Wikidata would be exactly as sparse and
ambiguous as queries 02-07 found it to be.

See "Piece 1" below for the concrete PlantMetWiki-based design (not a
LOTUS-based one - see why in the next section).

### 2. A Wikidata-native model for pathway membership

How should a compound, enzyme or gene's participation in a *specific
biosynthetic pathway* be stated, in a way that's distinguishable from an
incidental part-whole fact? This is the deeper problem the P527-
overloading finding exposes, and it's a chain-internal, Wikidata-native
modelling question - independent of whichever external database(s) get
linked.

See "Piece 2" below for a concrete option that doesn't require inventing
a wholly new property.

## Why not a "LOTUS ID" for piece 1

The course reviewer suggested starting with a LOTUS external-ID property,
since external identifiers are usually easier to get approved than
relational ones. Checked directly (2026-11-21): **LOTUS doesn't have
one.** LOTUS describes its own work as "knowledge management *through*
Wikidata" - its structure-organism pair data lives directly in Wikidata
as `found in taxon` (P703) statements referenced to source literature,
not in a separate database with its own stable record IDs. There is
nothing external for a "LOTUS ID" property to point to.

## Piece 1: an external identifier for PlantMetWiki pathways

Objective 5 itself names PlantMetWiki as a candidate, and - unlike
LOTUS - it genuinely has its own identifier scheme, verified against its
live SPARQL endpoint rather than assumed:

- Endpoint: `https://plantmetwiki.bioinformatics.nl/sparql`
- 1,162 distinct pathway items carry a stable `PC<number>` identifier
  (e.g. `PC1001` = "nicotine biosynthesis", a real plant biosynthesis
  pathway with no Wikidata counterpart yet - checked via
  `wbsearchentities`, 2026-11-21). This count matches the "1,162 plant
  metabolic pathways" figure from the PlantMetWiki preprint itself.
- `https://plantmetwiki.bioinformatics.nl/pathways/PC1000` resolves
  (HTTP 303 to a live record view) - a stable, dereferenceable
  identifier, not an artifact of one data release.
- Checked Wikidata: no "PlantMetWiki" or "PlantCyc" property exists yet.

Property sketch (not submitted):

| Field | Value |
|---|---|
| Property | PlantMetWiki pathway ID |
| Data type | External identifier |
| Domain | items `instance of` "biological pathway" (Q4915012), or the more specific "biosynthetic pathway" (Q70696303, see [`modelling-pattern.md`](modelling-pattern.md)) |
| Allowed values | `PC[0-9]+` |
| Example | "nicotine biosynthesis" [no Wikidata item exists yet] -> `PC1001` |
| Formatter URL | `https://plantmetwiki.bioinformatics.nl/pathways/$1` |
| Source | PlantMetWiki (Del Pup E, et al., 2026, bioRxiv, doi:10.64898/2026.07.22.733699) |

If this is ever submitted: find at least one *existing* Wikidata pathway
item that already corresponds to a PlantMetWiki `PC`-numbered pathway
(not yet cross-matched) for a second, real example, and confirm with
PlantMetWiki's maintainers that `PC<number>` is the identifier and URL
they want treated as canonical long-term.

## Piece 2: a lighter-weight model for pathway membership, before reaching for a new property

Rather than assuming the fix for P527-overloading must be an entirely new
base property (which would need its own full property-proposal process),
checked whether Wikidata already has a mechanism to disambiguate a part's
*role* within a statement. It does: **`object of statement has role`**
(**P3831**) - "role held by the predicate value (object) of a statement in
the context of that statement" - already exists, and reasonable role
values already exist too: `substrate` (Q12119506, "molecule upon which an
enzyme acts") and `reaction intermediate` (Q7458208).

Design sketch (not implemented):

```
pathway  [has part(s)]  compound
    qualifier: [object of statement has role] -> substrate | reaction intermediate
    qualifier: [found in taxon] -> (species this specific statement holds for)
    qualifier: [stated in] -> (source)

pathway  [has part(s)]  enzyme
    qualifier: [object of statement has role] -> catalyst-type role
    qualifier: [found in taxon] -> species
```

A `has part(s)` statement with **no** role qualifier (today's default)
stays exactly as ambiguous as it is now - which is itself useful: existing
statements aren't broken or require mass retroactive fixing, and newly,
carefully modelled biosynthesis statements become distinguishable from
legacy ones going forward.

**Why this is worth noting as a separate, lighter option:** P3831 and
reasonable role values already exist, so this piece needs **no new
property proposal and no community discussion at all** - it's a
documentation/convention question (how editors should use P527 + P3831
together for biosynthesis pathways), not a schema change. That's a
meaningfully lower-friction fix than piece 1's external identifier, let
alone a wholly new relational property, and worth keeping conceptually
separate from both.

**What it does not solve:** a role qualifier disambiguates the *semantic
type* of an existing P527 part, but doesn't create a first-class
compound -> pathway statement stated *from the compound's side* - the
project's own chain table lists "natural product -[produced by]->
pathway" as the missing link, imagining a statement you'd find by looking
at the compound item directly, not by reversing a pathway's P527 list.
Role qualifiers don't change that direction.

## Open question, deliberately left open

Does the chain also need a genuinely new, dedicated compound -> pathway
property (stated on the compound, the direction the project's own chain
table imagines), on top of - or instead of - piece 2's role-qualifier
convention on the existing `has part(s)` relation? This is a larger,
harder design question than either piece above, and this project
surfaces it without resolving it. Whoever picks this up next has three
live options to weigh, not one default answer:

1. Piece 2 alone (role-qualified P527) - lowest friction, no new property.
2. A new relational property, stated from the compound's side - matches
   the project's original chain-table framing most directly, but needs a
   full property proposal and community discussion.
3. Both - piece 2 for pathway-internal structure (what WikiPathways-style
   pathways already do), a new property only for the compound-level
   "what pathway(s) is this found in" summary view.

## Connections to the course material

See [`modelling-pattern.md`](modelling-pattern.md#connections-to-the-course-material-week-references)
for the full set of course-week citations this project draws on
(instance-of/subclass-of discipline, "where to state information", the
chemistry WikiProject's own class-migration precedent, and mereology/
"has part" being flagged as unfinished course material itself). The piece
2 idea above is a direct continuation of that last point: using an
existing qualifier mechanism to disambiguate an overloaded property,
rather than treating "has part" as a closed, finished part of the
ontology.
