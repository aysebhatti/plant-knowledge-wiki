# Ontology Course week-by-week summary and connections to this project

This is the full week-by-week summary of `slides_course/` (the course's
own lecture slides, not tracked in this repo - see `.gitignore`),
produced 2026-11-21 while looking for ways to ground this project's
findings in material the course actually taught. The distilled citations
drawn from this summary are embedded inline in:

- [`gap-analysis.md`](gap-analysis.md#connections-to-the-ontology-course-material) -
  "Connections to the Ontology Course material"
- [`../proposals/modelling-pattern.md`](../proposals/modelling-pattern.md#connections-to-the-course-material-week-references) -
  "Connections to the course material (Week references)"
- [`../proposals/pathway-representation-proposal.md`](../proposals/pathway-representation-proposal.md#connections-to-the-course-material) -
  "Connections to the course material"

This file keeps the full summary in one place so those citations can be
checked against their source, without needing to re-extract text from the
PDFs again.

---

## Week-by-week summary

**Week 1 - Intro to ontologies & the Wikidata ontology.**
Key concepts: ontology basics (classes/individuals/generalization),
instance-of vs subclass-of as the core distinction ("the most important
thing... is not mixing up instance of and subclass of"), metaclasses,
class order, upper/foundational ontologies (BFO, SUMO), strict vs
defeasible ontologies, and a dedicated discussion of "What is a Class -
Chemical Classes" using Aldol as the example ("the chemistry domain has
lots of ontological issues... even unclear whether there should be any
individuals").
*Directly applicable:* the chemistry-class discussion is a near-exact
precedent for this project's compound/pathway modelling questions (is a
compound a class or individual? are biosynthesis pathways classes or
individual processes?).

**Week 2 - Practical Wikidata editing & SPARQL.**
Key concepts: WDQS/QLever SPARQL querying, the `wdt:P31`/`wdt:P279*`
pattern for inference-free querying, non-explicit (uninferred) ontology
relationships, QuickStatements/OpenRefine/bots, the full property-proposal
creation workflow (propose -> discuss -> wait ~1 week -> "ready" ->
property created), statement rank and temporal qualifiers, methodologies
for ontology building (classes should be persistent things, not transient
states).
*Directly applicable:* this is this project's property-proposal playbook
almost verbatim - proposal page structure, transclusion on subject page,
discussion period, "ready" status.

**Week 3 - Upper ontology, foundational ontology, constraints.**
Key concepts: entity/class/metaclass as top of hierarchy; abstract vs
concrete object split; Wikidata constraints (mandatory-but-not-enforced,
limited expressive power, can't constrain classes); best-practice guidance
that constraints are advisory, not a substitute for correct modelling;
triples have inverses and a decision of "where to state" information (on
the more natural side, avoiding huge statement blocks).
*Directly applicable:* constraint limitations directly explain why
qualifier-based data-quality gaps (missing P703/P927/P459) won't be caught
by constraints alone - supports this project's "SPARQL-based gap analysis
is necessary because constraints are not enough" argument.

**Week 4 - Advanced ontology constructs + example domains (ships/person/gene/product).**
Key concepts: union-of / disjoint-union-of / disjoint-with properties and
disjointness violations/"culprits"; metaclasses and "is metaclass for";
class order (1st/2nd/3rd-order classes) as a structural sanity check; gene
domain explicitly flagged as instance-vs-subclass inconsistent ("gene was
subclass of nucleic acid sequence" causing abstract/physical disjointness
violation; "For a similar situation, see disease").
*Directly applicable:* the strongest match in the whole course. Gene
domain modelling problems (instance vs subclass confusion, disjointness
between physical/abstract) are structurally identical to what this
project would expect for biosynthetic pathway/enzyme/gene chains.

**Week 5 - Continued domains: book, chemistry (deep dive).**
Key concepts: chemistry WikiProject as a "well-developed domain" with its
own metaclass conventions; explicit discussion of chemical-compound-as-
class decision and its 2024 shift from "instance of" to "subclass of"
grouping (to fix class-order mismatches with "related domains" -
explicitly including genes/proteins, "still messed up"); book domain used
as case study of P279 hierarchy confusion and "avoid using this item"
constraint guidance as a documented anti-pattern.
*Directly applicable:* a precedent, rationale, and open admission that
chemistry/gene/protein modelling is still inconsistent - used as
literature/precedent evidence for this project's P31/P279 gap-analysis
findings on compounds and enzymes/genes.

**Week 6 - Chemistry domain again + "Problems in the Wikidata Ontology" survey + fixing disjointness/class-order violations.**
Key concepts: catalogued problem taxonomy (semantic drift, subclass-of
cycles, meta-level mix-up, redundant/exchanged sub-superclass, conceptual
ambiguity, inconsistent modelling, overgeneralization) with suggested
fixes (better documentation, better guidance, better software support,
preferred rank); disjointness "culprit" analysis methodology with real
statistics (86K subclass violations, ~10M instance violations, 14,480
culprits); gene-domain fix case study (removing "sequence" as superclass
of gene cut culprits from 10,656 to 3,727); Wikidata Roundtable gamified
crowd-fix tool; three "fundamental problems of Wikidata" (insufficient
fixing effort, resistance to changing existing modelling, lack of formal
definitions).
*Directly applicable:* this project's methodological template for a
gap-analysis report - the problem taxonomy gives ready-made categories to
classify findings into, and the culprit-counting/statistics approach is a
model for quantifying gaps via SPARQL exactly as this project does.

**Week 7 - Same disjointness/class-order material repeated + live-fix demos + AI-assisted workflow + future work.**
Key concepts: live worked fix examples (Buddharupa disjointness fix,
instance-loop fix); modern AI-assisted Wikidata-fixing workflow with
explicit transparency/ethics guidance (disclose AI use, verify SPARQL
results aren't hallucinated); "Be Bold" editing culture; future-work list
includes "mereology (part-of) in Wikidata... done, but more work remains."
*Directly applicable:* the "mereology/part-of" future-work line is
directly relevant to this project's P527 "has part(s)" overload problem
(real biosynthetic pathway composition vs. plain chemical composition) -
this project's finding continues this acknowledged open problem. The
AI-disclosure norms are the reason this file, and the inline citations
throughout the repo, document AI involvement and live-verification
discipline explicitly rather than presenting findings as given.

**Week 8 - Wrap-up/project presentations only.**
No new technical content (this project is listed by name on the agenda as
one of ~15 concurrent course projects - the instructors already know
about it). Mentions ongoing structures to plug into post-course:
WikiProject Ontology, the "Wikidata Ontology Cleaning Task Force" weekly
meetings, and a proposed "User Group for Data Organization and Quality" -
worth joining/citing as the venue to carry this project's property
proposal and pathway-modelling fixes forward.

---

## Source

Produced by reading all 8 PDFs in `slides_course/` (pypdf text
extraction, all pages). Original PDFs are not tracked in this repository
(see `.gitignore`) - this summary is a derived analysis of their content,
not a reproduction of the slides themselves.
