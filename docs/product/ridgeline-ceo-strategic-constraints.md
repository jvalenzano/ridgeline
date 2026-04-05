# Ridgeline — CEO Strategic Constraints & Design Reference

> **Date:** 2026-04-05 | **Source:** CEO strategic advisory session
> **Status:** Approved — these constraints govern all Ridgeline spec and build decisions

---

## Design Reference: Bridge Fire BAER Technical Report

The primary design target for Ridgeline's Post-Fire Intelligence Summary is the **Bridge Fire BAER Technical Report** (Angeles National Forest, September 2024, 54,659 acres, San Gabriel Mountains).

- **Source:** `wrightwoodfsc.com/fires/Bridge/BridgeBAER_PostFireTechnicalReport_10152024.pdf`
- **Why this document:** It's a real, public, recent BAER technical report. Ridgeline's output should feel like a *precursor* to this document — the intelligence picture that exists before the BAER team arrives.

**The design exercise:** Read it section by section and ask: "What in here could Ridgeline have pre-assembled from public data before the team arrived?" Sections where the answer is "most of it" are v1 targets. Sections where the answer is "none of it — requires boots on the ground" become explicit "data pending — requires field assessment" placeholders. The boundary between those two is where Ridgeline's value lives.

---

## Five Strategic Constraints

### 1. Temporal Honesty

The Post-Fire Intelligence Summary must have explicit version tiers reflecting data availability:

- **T+24h:** Perimeters, hotspots, weather, WUI exposure, stream gauges, historical analogs. Says "burn severity data not yet available — expected [date]" rather than omitting or fabricating.
- **T+7-14d:** Upgrades when BARC satellite imagery drops.
- **T+30d:** Further refinement as USGS debris flow models and validated burn severity become available.

Temporal honesty is itself a trust signal. A practitioner who sees "data pending" trusts you more than one who sees a confident claim from a source that doesn't exist yet.

### 2. Inline Provenance (Not Appendix)

Every factual claim gets an inline source tag visible at the point of consumption:

> "The Springs Fire burned 3,500 acres *(Source: NIFC InciWeb, accessed 2026-04-05)*"

A practitioner scanning this on a helicopter needs to see the source next to the number, not buried in footnote 47. The provenance manifest at the bottom is the machine-readable audit version. The inline citations are the human-readable trust version. This is the "curation not generation" principle made visible.

### 3. Historical Analog Is the Differentiator

Any tool can pull current fire stats from NIFC. What makes Ridgeline's output recognizably *intelligent* is the historical analog section:

> "The Springs Fire burned through chaparral terrain on 30-45% slopes with south-facing aspect. The most relevant historical analog is the [specific fire] in [year], which produced debris flows in [X] of [Y] burned watersheds within [Z] days of the first 15mm/hr rainfall event."

That cross-domain, cross-temporal reasoning is the VistaIR doing its job — connecting this fire's characteristics to historical outcomes in similar terrain and fuel types.

- **If thin or generic:** You've built an expensive API aggregator.
- **If specific and surprising:** You've built something a practitioner can't get anywhere else.

### 4. BAER-Native Framing

The "AI-Synthesized" framing must mirror how the BAER community already talks. BAER teams use satellite imagery which is then "validated and adjusted by BAER team field surveys." That's their mental model: remote sensing produces a first approximation, human expertise refines it.

**Recommended language:**

> *AI-Synthesized Intelligence Summary — Initial Assessment from Remote Data Sources — Requires Field Validation and Expert Review*

This tells a BAER practitioner: this is the same kind of artifact you already use (satellite-derived initial assessment), it just covers more domains simultaneously and arrives faster. You're not replacing their judgment — you're giving them a head start.

### 5. Two-Sided Acceptance Test (v1)

Can a BAER-adjacent professional read the Springs Fire Post-Fire Intelligence Summary and identify:

1. **At least one insight they didn't already have** — proof the cross-domain synthesis adds value
2. **At least one error they can correct** — proof the document is substantive enough to critique

If both are yes: worth iterating on.
If every insight is obvious (just restating InciWeb): the synthesis layer isn't working.
If every claim is wrong (hallucinated sources): the data pipeline is broken.

The goal isn't perfection — it's a document good enough to provoke a substantive conversation about accuracy, not about concept.

---

## First Practitioner Reviewer

**Shadat** — TechTrend colleague who saw the Vista globe demo on March 27 and expressed interest. Internal to TechTrend with USFS context. Serves as initial reviewer and bridge to formal BAER analysts or regional FS contacts as needed.

**Why internal first:** Honest feedback without burning external credibility on a v1. If the intelligence output impresses him, he's the bridge outward. If it doesn't, we need to know that before it goes anywhere else.

---

## BAER Workflow Context (From Research)

For reference — how the formal BAER process works, so Ridgeline's output is positioned correctly:

1. BAER team generates a **soil burn severity map** using satellite imagery
2. Team **validates through field surveys** (boots on ground)
3. Team produces an **assessment report** covering:
   - Effects of fire on the watershed
   - Values at risk (life, property, cultural resources, critical natural resources)
   - Treatments to address values at risk
   - Cost breakdowns for all treatments (funding request)

**Ridgeline's position:** Produces the intelligence picture that exists *before* step 1 — the synthesis a team lead reads to get oriented before field surveys begin. Not a replacement for the BAER report, but a pre-assembled head start on the research layer that feeds it.
