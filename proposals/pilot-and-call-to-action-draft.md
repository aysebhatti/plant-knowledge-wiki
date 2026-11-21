# Pilot section and Call to Action drafts (for the Wikidata project page)

**Written for:** readers of the course project page (same audience as
`results-section-draft.md`). Two wikitext blocks below, meant to be
pasted in once Phase 1 is actually run (see
`pilot-phase1-implementation.md`).

**Before pasting the Pilot section:** replace every `Q_NEW` placeholder
with the real QID QuickStatements (or the manual UI) gives you after
running Phase 1.

---

## Pilot section (replaces/extends the current "Pilot" text)

```wikitext
=== Pilot (Objective 6) ===

'''Phase 1 implemented:''' a new item, [[Q_NEW|caffeine biosynthesis]],
was created as a minimal pilot of the modelling pattern above. It is
typed {{P|31}} "biosynthetic pathway" ({{Q|70696303}}) - using the
specific, previously-unused subclass identified during the gap analysis,
rather than its generic parent - and carries four {{P|527}} ("has
part(s)") statements tracing the real biosynthetic route: xanthosine
({{Q|408788}}) → 7-methylxanthine ({{Q|27121426}}) → theobromine
({{Q|206844}}) → caffeine ({{Q|60235}}). Each statement is qualified with
{{P|3831}} ("object of statement has role") - ''reaction intermediate''
({{Q|7458208}}) for the first three, ''product'' ({{Q|542929}}) for
caffeine - and referenced to a general caffeine-biosynthesis mechanism
paper ({{Q|105208193}}).

This deliberately covers only the compound-level route, which is shared
plant biochemistry, not something that differs between coffee, tea and
cacao. No taxon qualifier was added at this stage, to avoid overclaiming
species-specific data that hasn't been sourced yet.

'''Phase 2, left for future work:''' the project's own motivating claim -
that caffeine is made by ''different enzymes'' in each of the three
species - lives at the enzyme/gene level, not the compound level, and is
exactly where this project's gap analysis found Wikidata to be emptiest
(zero genuine enzyme-typed pathway participants across all six species
checked, and gene data limited to 28 unpositioned genes in ''Arabidopsis
thaliana''). Completing Phase 2 would mean, per species: sourcing the
actual methyltransferase genes from the literature (e.g. coffee's XMT/
MXMT/DXMT; already-citable sources exist, e.g. {{Q|28139580}}, "7-
Methylxanthine methyltransferase of coffee plants"), checking whether a
Wikidata gene item already exists for each (unlikely, per the gap
analysis), creating one with {{P|703}} (found in taxon) and, where
available, {{P|644}}/{{P|645}}/{{P|659}} (genomic position), and linking
it with {{P|702}} ("encoded by") to the corresponding enzyme, qualified
with {{P|703}} for the species it holds in. This is real literature
research per species, not Wikidata editing alone, and is left as an open
invitation rather than completed here.
```

## Call to action for other Wikidatans

```wikitext
=== Call to action ===

This project leaves more open than it closes, by design - plant
biosynthesis is a large domain and six species is a small sample. Three
concrete ways to pick this up:

* '''Curate more plant pathways using the pattern above.''' Any editor
  who knows a specific plant biosynthetic pathway can apply the same
  recipe: type the pathway item {{P|31}} "biosynthetic pathway"
  ({{Q|70696303}}) instead of its generic parent, and qualify its
  {{P|527}} statements with {{P|3831}} roles rather than leaving them
  ambiguous. Neither step needs a new property or a community discussion
  - this is exactly the kind of incremental, "be bold" fix the course
  highlighted as still needed: mereology ("has part") in Wikidata was
  flagged as "done, but more work remains," and this project's gap
  analysis found that remark still true for the biosynthesis domain
  specifically.
* '''Take the open representation question to the Ontology Cleaning Task
  Force / the proposed Data Organization and Quality User Group.''' This
  project's reflection on representing the compound → pathway → enzyme →
  gene → genome chain
  (see the project's GitHub repository, <code>proposals/pathway-representation-proposal.md</code>)
  raises a genuinely open design question - whether a dedicated
  compound → pathway property is needed on top of the role-qualifier
  approach - that is better settled by the people who maintain this part
  of the ontology than by one course project alone.
* '''Help close the plant gene data gap.''' Of six species checked, five
  have ''zero'' genes typed and linked to that taxon at all, and the
  sixth (''Arabidopsis thaliana'') has 28 genes with ''no'' genomic
  position between them. This is the single largest, most concrete gap
  this project found, is plain data entry once a source exists (no new
  property or modelling decision needed), and would directly unblock the
  Phase 2 pilot work above for anyone who wants to pick it up.

All queries, the full gap analysis, and the modelling pattern are kept as
a reusable, re-runnable resource (not a one-time report) at
https://github.com/elenadelpup/plant-knowledge-wiki, including the
Jupyter notebook used to generate this page's charts, so any of the above
can be re-verified rather than taken on faith.
```
