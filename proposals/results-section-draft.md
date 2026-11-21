# Results section draft (for the Wikidata project page)

**Written for:** readers of
[Wikidata:WikiProject Ontology/Ontology Course/Projects Offering 3/Plant Knowledge Wiki](https://www.wikidata.org/wiki/Wikidata:WikiProject_Ontology/Ontology_Course/Projects_Offering_3/Plant_Knowledge_Wiki)
(course instructors, reviewers, and the wider WikiProject Ontology
community) - not a chat reply. The wikitext block below is meant to be
pasted directly into that page's "Results" section. A copy is kept here
for the project's own history; edit this file if the wording changes
before pasting.

All queries and figures below are reproducible from this repository
(`queries/`, `analysis/gap-analysis.md`, `notebooks/`,
`proposals/`) and were re-run against the live Wikidata Query Service,
not computed once and cached.

---

```wikitext
== Results ==

=== Domain survey and gap analysis (Objectives 1 and 3) ===

Coverage of the full chain (compound → pathway → enzyme → gene → genome)
was measured with SPARQL across six species: ''Arabidopsis thaliana'',
''Camellia sinensis'' (tea), ''Coffea arabica'' (coffee), ''Theobroma
cacao'' (cacao), ''Brassica oleracea'' and ''Brassica rapa'' - chosen to
match the project's own motivating examples (caffeine, glucosinolates).

* '''Compound → pathway.''' Of compounds known to occur in a species
  ({{P|703}}, "found in taxon"), the share with a backlink to a genuine
  WikiPathways-sourced pathway (typed {{Q|4915012}}, "biological
  pathway", and carrying a WikiPathways ID, {{P|2410}}) stays under 24%
  for every species checked, and under 10% for three of the six. This is
  not an ''Arabidopsis''-specific artifact.
* '''Qualifier usage.''' Every genuine pathway–compound statement across
  all six species was checked for ''any'' qualifier at all - taxon,
  tissue, evidence, anything. Result: '''zero'''. The qualifier pattern
  proposed under Objective 4 would be the first qualifier convention on
  this part of the graph, not a fix to inconsistent existing usage.
* '''Gene → genome.''' Of the six species, five have '''zero''' items
  typed as a gene ({{Q|7187}}) and linked to that taxon ({{P|703}}) at
  all. The sixth, ''Arabidopsis thaliana'', has 28 - and '''none''' of
  them carry a genomic position ({{P|644}}/{{P|645}}).
* '''Enzyme → gene.''' Across all six species, the genuine pathways
  reachable from their compounds contain exactly '''one''' item typed as
  an enzyme ({{Q|8047}}) in total - unrelated to plant biosynthesis - with
  no {{P|702}} ("encoded by") gene link.
* '''Modelling issues found:''' {{P|527}} ("has part(s)") is overloaded,
  mixing genuine pathway membership with plain chemical composition facts;
  natural-product class typing is inconsistent (not every LOTUS-sourced
  compound shares one {{P|31}} value); Wikidata currently runs '''two
  incompatible conventions''' for "pathway-like" entities side by side
  (WikiPathways-style items typed {{Q|4915012}}, and separately-imported
  Gene Ontology process terms typed {{Q|2996394}}/"biological process",
  which share no common type); and a pre-existing, correctly-placed
  subclass, "biosynthetic pathway" ({{Q|70696303}}), sits completely
  unused (zero items) in favour of its generic parent class.

'''Case study:''' the project's own motivating example - that caffeine is
biosynthesized by different enzymes in coffee, tea and cacao - was tested
directly rather than assumed. Caffeine ({{Q|60235}}) is confirmed
{{P|703}} for all three species. Every item linked to caffeine via
{{P|527}} was examined (48 distinct items): consumer products, drug
combinations, human pharmacology pathways and taste-receptor entries, and
exactly '''one''' genuine metabolic pathway - "caffeine and theobromine
metabolism" ({{Q|28869950}}, WP3633) - which is tagged {{P|703}} to
'''{{Q|15978631}}, archaic ''Homo sapiens''''', i.e. human caffeine
breakdown, not plant biosynthesis in any of the three species. '''Zero'''
plant-side caffeine biosynthesis data exists in Wikidata today.

A reusable chart set generated from this analysis (species coverage, the
caffeine case study, and a stage-by-stage diagram of the chain) is in
<code>notebooks/gap_visualizations.ipynb</code> in the project repository
and embedded in <code>analysis/gap-analysis.md</code>.

=== Modelling pattern (Objective 4/6) ===

A modelling pattern was drafted covering: one pathway item per
biosynthetic route (taxon stated as a statement-level qualifier via
{{P|703}}, not by forking the pathway item per species); typing new
pathway items as the specific, currently-unused "biosynthetic pathway"
class ({{Q|70696303}}) rather than its generic parent; disambiguating
{{P|527}} statements with a role qualifier ({{P|3831}}, "object of
statement has role") instead of assuming a new base property is required;
a required qualifier set (taxon, tissue, evidence, reference) for
organism-specific statements; and reuse of existing, already-correct
properties ({{P|702}}, {{P|644}}/{{P|645}}/{{P|659}}) for the enzyme→gene
and gene→genome legs, where the gap is a data problem, not a modelling
one.

Three worked examples were developed: caffeine biosynthesis (no such item
exists in Wikidata yet); a real PlantMetWiki pathway with no Wikidata
counterpart ("nicotine biosynthesis", PlantMetWiki ID <code>PC1001</code>,
verified against PlantMetWiki's live SPARQL endpoint); and glucosinolate
biosynthesis, which exposes the two-pathway-convention collision above
directly, since the only existing Wikidata item for it is a Gene Ontology
process term with no taxon, gene or enzyme data attached.

Full details: <code>proposals/modelling-pattern.md</code> in the project
repository.

=== Property proposal (Objective 5) ===

The course reviewer's suggestion to start with a LOTUS external-ID
property was investigated and found not to apply: LOTUS performs
"knowledge management ''through'' Wikidata" rather than maintaining a
separate database with its own record identifiers, so there is nothing
external for such a property to point to. This was verified directly
against Wikidata (no "LOTUS" property exists) rather than assumed.

Instead, the proposal was reframed around the broader question of how to
represent the compound→pathway→enzyme→gene→genome chain, with two
concrete, independently-useful pieces: (1) a "PlantMetWiki pathway ID"
external-identifier property, with its format and formatter URL verified
against PlantMetWiki's live SPARQL endpoint (1,162 pathways, stable
<code>PC<number></code> identifiers); and (2) the {{P|3831}}
role-qualifier approach above, which needs no new property proposal at
all.

'''This proposal has not been submitted to [[Wikidata:Property
proposal]], and no community discussion has been opened''' - a deliberate
choice for this round of the project. Full reasoning, the property
sketch, and the open question of whether a dedicated compound→pathway
property is also needed: <code>proposals/pathway-representation-proposal.md</code>.

=== Pilot (optional, Objective 6) ===

Not implemented as live edits in this round. The modelling pattern's
worked examples above sketch what a pilot on caffeine biosynthesis or the
PlantMetWiki-sourced nicotine biosynthesis pathway would look like.

=== Visualizations ===

Added following course review feedback (the suggestion to invest in
visualizations, since much of this analysis is difficult to follow as raw
SPARQL output). A Jupyter notebook
(<code>notebooks/gap_visualizations.ipynb</code>) re-runs the project's
queries live and renders: per-species pathway coverage, a category
breakdown of everything Wikidata links to caffeine, and a stage-by-stage
diagram tracing caffeine through the full chain for coffee, tea and
cacao.

=== Resources ===

All queries, analysis, the modelling pattern, the property-proposal
reflection, and the visualization notebook are in the project repository:
https://github.com/elenadelpup/plant-knowledge-wiki
```
