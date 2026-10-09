# Prompts

## Prompt 1 Initial Multimodal Screening
Continue Parkinson's Disease Drug Repurposing
### CURRENT STAGE
Do NOT start Deep Validation.
We currently have 27 raw candidate outputs.
Screen the existing candidates only.
### ALLOWED DATA SOURCES
Use only:

Monarch Initiative KG

PD-related biological associations

biological route / context

PubMed

exact-compound pharmacology

mechanism

functional phenotype

prior PD experimental evidence

contradictory evidence

CNS evidence

ClinicalTrials.gov

human PD exposure

clinical trials

Google Patents (patents.google.com)

patent prior-art / novelty screening ONLY
Justia Patents may only be used as a backup search/listing source.
Do not use Justia alone for a final patent-based decision.
Do not use PubChem patent pages, Espacenet, WIPO PATENTSCOPE,
or USPTO search because they are unavailable in the current environment.
Do not use general WebSearch results, vendor pages, Wikipedia,
blogs, or unsupported model knowledge as scientific evidence.
If evidence cannot be verified, report UNRESOLVED.

---
### STEP 1 — IDENTITY
---
Resolve the exact candidate identity first.
Record:
candidate

canonical name

synonyms

discovery branch

target/action if supported
Merge duplicate exact compounds but preserve all discovery branches.
All classifications must concern the EXACT compound,
not merely its target, pathway, drug class, or related molecule.

---
### STEP 2 — NOVELTY / PRIOR PD EXPOSURE
---
Apply N rules deterministically and in order.
#### N0:
Exact compound is an established/approved PD treatment.
-> REJECT
#### N1:
Exact compound has been directly tested in:

human PD study

PD clinical trial

animal PD model

cellular PD model

PD-associated genetic/molecular experimental model
-> REJECT
#### N2:
No direct PD experiment, but exact compound has already been:

explicitly proposed as a PD therapeutic

explicitly claimed/included for PD in a therapeutic patent

substantially discussed/predicted as a PD repurposing candidate
-> REJECT from strict novel pool
#### N3:
No direct exact-compound PD evidence found, but its known
target/pathway has a clear PD biological connection.
-> KEEP
#### N4:
No meaningful exact Drug-PD connection found and the proposed
Drug-PD hypothesis arises mainly by combining distributed evidence.
-> KEEP
Assign Prior PD Exposure:
P0 = approved / established PD use
P1 = human PD study or clinical trial
P2 = animal / cell / PD-associated experimental model
P3 = explicit proposal / patent / computational prediction
P4 = no meaningful exact Drug-PD link found
IMPORTANT:
If N0, N1, or N2 is confirmed, stop expensive screening for that
candidate and mark REJECT.
#### Patent rule:
If Google Patents confirms that the exact compound, verified synonym,
or clearly corresponding prodrug/form is explicitly proposed or claimed
for Parkinson's disease therapy:
N = N2
P = P3
Decision = REJECT
Target-class or related-compound patents are NOT sufficient.

---
### STEP 3 — M / F / DIRECTION / CNS
---
Run this step only for candidates surviving N/P screening.
#### M — Mechanistic Evidence
M1 = weak / largely speculative mechanistic evidence
M2 = moderate mechanistic component evidence
M3 = strong mechanistic component evidence
Evaluate evidence supporting:
Exact Drug
→ target / process / phenotype
→ PD-relevant biology
Do not silently attribute target-class evidence to the exact compound.
#### F — Exact-Compound Functional Evidence#### F0:
Only binding, target annotation, or mechanism-of-action evidence.
#### F1:
Exact compound experimentally modulates the relevant target/pathway,
but no relevant downstream functional phenotype is demonstrated.
#### F2:
Exact compound produces a relevant functional phenotype in cell or
animal experiments outside PD.
Examples:
mitochondrial function, lysosomal function, autophagic flux,
inflammatory state, proteostasis, neuronal survival,
oxidative stress, synaptic phenotype, cellular trafficking.
#### F3:
Exact compound demonstrates the relevant functional phenotype in a
strong translational or human context while still lacking a direct PD
therapeutic link.
#### DIRECTION
consistent:
drug action is compatible with correcting the supported PD-relevant biology.
inconsistent:
drug action opposes the better-supported biological direction.
unclear:
evidence is insufficient or contradictory.
Do not infer therapeutic direction from a generic association.
#### CNS
Classify:
feasible
uncertain
fatal_liability
Absence of BBB/CNS evidence alone = uncertain.
Use fatal_liability only when evidence gives a strong reason that
the proposed CNS mechanism is not realistically achievable.
### EVIDENCE RULE
For major claims, record the supporting source:

Monarch relationship

PMID

NCT ID

Google Patent number
Use these evidence labels where appropriate:
OBSERVED_KG
OBSERVED_LITERATURE
SUPPORTED_CONTEXT
AGENT_INFERENCE
CONTRADICTED
UNRESOLVED
Never present AGENT_INFERENCE as an observed fact.

---
### PRIORITIZATION
---
Preferred Round 3 profile:
P4
AND M2 or M3
AND F2 or F3
AND Direction = consistent
AND CNS != fatal_liability
AND no direct PD clinical, animal, cellular,
or PD-associated experimental exposure.
F0 candidates should normally not proceed.
F1 candidates may remain HOLD / lower priority.
Do not keep weak candidates merely to reach a target number.

---
### OUTPUT
---
Return ONE complete screening table containing:
Candidate
Canonical identity
Discovery branch
Target/action
Exact prior PD evidence
Evidence source
N
P
M
F
Direction
#### CNS
Exact-compound functional phenotype
PD relevance of phenotype
Contradictory evidence
KEEP / REJECT / HOLD
Reason
Most important remaining inference gap
Then report:

total screened

REJECT count

HOLD count

KEEP count

ranked shortlist of strongest survivors
Do NOT start Deep Validation.
STOP after returning the screening table for review.
## Prompt 2: N/P Critic Agent
Do NOT rerun the full screening. Do NOT change existing M / F / Direction / CNS classifications. Do NOT start Deep
Validation yet. Only re-check the N/P novelty status of the 8 current KEEP candidates: - Oltipraz - Ru265 - Neflamapimod
(VX-745) - Emricasan (IDN-6556) - Bis-T-23 - ANX005 - PMX205 - Ruxolitinib For each exact compound: 1. Re-check prior
PD exposure using: - PubMed - ClinicalTrials.gov - Google Patents 2. Google Patents is the primary patent-content source.
Justia may only be used to locate a patent number and is not sufficient alone for an N2/P3 decision. 3. Apply the existing
novelty rules: - N0/P0 = established PD use → REJECT - N1/P1 or P2 = direct human/clinical or experimental PD exposure
→ REJECT - N2/P3 = explicit PD therapeutic proposal/patent/prediction → REJECT - N3/N4 with P4 = no meaningful
exact Drug-PD prior exposure found → retain 4. If Google Patents or ClinicalTrials.gov cannot be successfully checked, do
NOT claim: “no prior PD evidence/patent exists.” Instead record that component as: UNRESOLVED or
SOURCE_ACCESS_FAILED. 5. Preserve all previously established: - M - F - Direction - CNS - functional evidence -
mechanistic evidence Return a compact table with: Candidate | Previous N/P | PubMed check | ClinicalTrials.gov check |
Google Patents check | Revised N/P | KEEP/REJECT/HOLD | Reason Then provide the final shortlist. STOP after the table.
Do not start Mechanism Proposing or Deep Validation. Make sure the table is in excel file type
## Prompt 3: Mechanism Hypothesis Proposing and Independent Validation
### INPUT CONTEXT
Use the finalized screening and subsequent N/P critic output as the starting context for Deep Validation.
For the candidates, preserve and reuse all available prior information, including:
canonical identity
discovery branch
previous N/P classification
previous M/F/Direction/CNS classification
previously retrieved evidence
known contradictory evidence
unresolved source-access issues
previous KEEP rationale
Do not restart candidate discovery or screening from zero.
However, previous screening results are not treated as final mechanistic proof.
Deep Validation must independently verify the relationships needed for the mechanism using the approved sources.
If newly retrieved evidence contradicts or changes a previous screening conclusion, explicitly record the conflict and
explain the revision.
Do not silently overwrite prior results.
Perform Deep Validation for candidates:
Oltipraz
PMX205
Neflamapimod (VX-745)
Emricasan (IDN-6556)
ANX005
Bis-T-23
Do not investigate any other candidates.
Complete all candidates without waiting for further user confirmation.

---
### DATA SOURCES
---
Use only:
PubMed
Monarch Initiative
ClinicalTrials.gov
Do not use any other source.
For PubMed:
Use title, abstract, PMID, and publication metadata first.
Full text may be used only when the abstract is insufficient to verify a critical relationship.
If full text is used, explicitly mark the evidence as FULL_TEXT.
Do not use model background knowledge as evidence.
If an approved source cannot be accessed, record:
SOURCE_ACCESS_FAILED
If a relevant question cannot be resolved from the approved evidence, record:
UNRESOLVED
Do not substitute another source.
A source-access failure for one candidate or one relationship must not stop the overall analysis.
Continue with the avaliable evidence and continue to the next candidate.

---
### TASK 1 — MECHANISM HYPOTHESIS PROPOSING
---
For each candidate, independently explore and propose the strongest evidence-grounded mechanistic hypothesis
connecting the exact compound to Parkinson’s-disease-relevant biology.
Do not assume a predefined mechanism.
Do not require any specific intermediate entity type.
Do not require a fixed number of intermediate steps.
Allow the mechanism to emerge from the evidence.
The Agent must decide:
which biological relationships are relevant,
which relationships should be investigated further,
how the evidence can be connected,
and where inference is necessary.
Prefer mechanisms that are biologically coherent, well supported, and require fewer unsupported assumptions.
Do not attempt to preserve the previous screening mechanism if the evidence supports a different mechanism.
If the strongest evidence requires revising the original mechanism, explicitly report the revision.

---
### EDGE-LEVEL EVIDENCE REQUIREMENT
---
Every relationship in the final mechanism chain must be individually documented.
For every edge report:
Edge number
Subject
Predicate
Object
Evidence status
Source
Source identifier
Evidence location
Brief evidence statement
Evidence status must be one of:
OBSERVED_KG
OBSERVED_LITERATURE
SUPPORTED_CONTEXT
AGENT_INFERENCE
CONTRADICTED
UNRESOLVED
Source identifier must be:
PMID for PubMed
Monarch identifier for Monarch
NCT number for ClinicalTrials.gov
Evidence location must indicate:
ABSTRACT
FULL_TEXT
MONARCH_KG
CLINICALTRIALS_RECORD
If an edge is supported by direct evidence, cite the exact source.
If an edge is AGENT_INFERENCE, do not provide a false citation for the inferred relationship.
Instead, explicitly provide:
the reasoning for the inference
which observed/supported edges the inference is based on
why those pieces of evidence justify proposing the bridge
Never present AGENT_INFERENCE as an established fact.
Never silently convert association into causation.
Never silently attribute target-class, pathway-class, phenotype-level, or contextual evidence to the exact compound.
A failure to find evidence is not itself contradictory evidence.
Do not label a zero-result search as CONTRADICTED.
Use UNRESOLVED or explicitly state that no direct evidence was identified using the approved searches.

---
### TASK 2 — INDEPENDENT VALIDATION
---
After Mechanism Hypothesis Proposing, run a separate independent Critic for each candidate.
The Critic must actively test whether the proposed mechanism is weakened or invalidated by available evidence.
The Critic must independently search the same three approved sources.
It should examine contradictions, incompatible directions, context mismatches, reachability issues, alternative
explanations, unsupported edges, and any other evidence that materially challenges the proposed hypothesis.
Do not constrain the Critic to a predefined checklist if other important problems emerge.
Every criticism must also include its evidence source.
If the criticism is an inference rather than a directly observed fact, clearly state the reasoning.
The Critic must not try to rescue a hypothesis when material contradictory evidence is found.

---
### FINAL OUTPUT
---
For each candidate provide:
Candidate identity
Proposed mechanistic hypothesis
Complete mechanism chain
Edge-level Evidence Ledger
Agent-inferred bridges and their reasoning
Unresolved relationships
Contradictory evidence
Independent Critic findings
Main remaining evidence gap
Explicit conflicts or revisions relative to the previous screening and N/P critic assessment
Final verdict
Final verdict must be one of:
HYPOTHESIS_HOLDS_UP
INCONCLUSIVE
HYPOTHESIS_UNDERMINED
Brief justification for the verdict
Do not make therapeutic efficacy claims.
The goal is to produce a testable, evidence-grounded mechanistic hypothesis.

---
### EXECUTION RULE
---
Complete Deep Validation for all candidates:
Oltipraz
PMX205
Neflamapimod
Emricasan
ANX005
Bis-T-23
Mechanism Hypothesis Proposing
→ Edge-level Evidence Ledger
→ Independent Critic
→ Final Verdict
