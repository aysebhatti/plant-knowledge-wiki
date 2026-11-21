# Pilot, Phase 1: implementation guide

Status: a guide, not yet run. This requires your own Wikidata account - I
can't post to the live wiki on your behalf. Everything below was checked
against live Wikidata/PlantMetWiki data (dates noted), not assumed.

Phase 1 = the pathway skeleton only (one new item, four `has part(s)`
statements, no taxon claims, no new genes). Phase 2 (enzymes/genes per
species) is intentionally deferred - see the pilot section draft below.

## Why no taxon qualifier in Phase 1

The compound route itself (xanthosine → 7-methylxanthine → theobromine →
caffeine) is general plant biochemistry, not something that differs
between coffee/tea/cacao - the project's own "different enzymes per
species" claim is about the *enzyme* step, not the compound route. Adding
`found in taxon` to the compound statements now, without a specific
per-species source for each, would be an overclaim. Taxon qualifiers
belong on the Phase 2 enzyme/gene statements, once those are actually
sourced per species.

## Pre-flight checklist

1. Log into your own Wikidata account (not anonymous - edits should be
   attributable).
2. Re-check no "caffeine biosynthesis" pathway item has been created by
   someone else since 2026-11-21 (`wbsearchentities` or a quick search on
   wikidata.org) - unlikely to have changed, but free to confirm.
3. Decide where to run this: [QuickStatements](https://quickstatements.toolforge.org/)
   (batch, see below) or the Wikidata web UI item-by-item (slower, see
   "Manual alternative" below). QuickStatements needs your Wikidata OAuth
   login via Toolforge.

## QuickStatements batch (V1 syntax)

Every QID below was verified live (2026-11-21): `Q408788` xanthosine,
`Q27121426` 7-methylxanthine, `Q206844` theobromine, `Q60235` caffeine,
`Q70696303` biosynthetic pathway, `Q7458208` reaction intermediate,
`Q542929` product, `Q105208193` the cited paper, `P31`/`P527`/`P3831`/
`P248` all confirmed real properties with these exact meanings.

```
CREATE
LAST	Len	"caffeine biosynthesis"
LAST	Den	"biosynthetic pathway converting xanthosine to caffeine via 7-methylxanthine and theobromine"
LAST	P31	Q70696303
LAST	P527	Q408788	P3831	Q7458208	S248	Q105208193
LAST	P527	Q27121426	P3831	Q7458208	S248	Q105208193
LAST	P527	Q206844	P3831	Q7458208	S248	Q105208193
LAST	P527	Q60235	P3831	Q542929	S248	Q105208193
```

Paste this into QuickStatements' "V1" input mode and run it. It creates
one new item with: an English label and description, `instance of
biosynthetic pathway`, and four `has part(s)` statements (to xanthosine,
7-methylxanthine, theobromine, caffeine), each qualified with its role
(`reaction intermediate` for the first three, `product` for caffeine) and
referenced to the cited paper.

## Manual alternative (Wikidata web UI)

If you'd rather not use QuickStatements:

1. Go to wikidata.org, "Create a new item."
2. Label: `caffeine biosynthesis`. Description:
   `biosynthetic pathway converting xanthosine to caffeine via 7-methylxanthine and theobromine`.
3. Add statement: `instance of` → `biosynthetic pathway` (Q70696303).
4. Add statement: `has part(s)` → `xanthosine` (Q408788). On that
   statement, add qualifier `object of statement has role` →
   `reaction intermediate` (Q7458208), and reference `stated in` →
   the paper (Q105208193).
5. Repeat step 4 for `7-methylxanthine` (Q27121426, same role) and
   `theobromine` (Q206844, same role).
6. Repeat once more for `caffeine` (Q60235), but with role `product`
   (Q542929) instead of `reaction intermediate`.

## After it's created

Run this to confirm it looks right (replace `Q_NEW` with the real QID
QuickStatements/the UI gives you):

```sparql
SELECT ?prop ?propLabel ?val ?valLabel ?role ?roleLabel WHERE {
  wd:Q_NEW ?p ?val .
  ?prop wikibase:directClaim ?p .
  OPTIONAL {
    wd:Q_NEW p:P527 ?stmt .
    ?stmt ps:P527 ?val ; pq:P3831 ?role .
  }
  SERVICE wikibase:label { bd:serviceParam wikibase:language "en". }
}
```

Then let me know the new QID - I can update `modelling-pattern.md`'s
Example A to point at the real item instead of "[NEW ITEM]", and add it
as the "Example 2" the property-proposal reflection currently leaves
blank.
