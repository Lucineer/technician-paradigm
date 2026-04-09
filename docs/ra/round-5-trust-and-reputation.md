# Round 5: trust-and-reputation

---

# The Trust Mesh: Reputation as Ontological Infrastructure

## I. Trust as the Irreducible Primitive

Before we design scoring algorithms and verification protocols, we must establish what trust *is* within the Technician Paradigm. Trust is not a social convenience overlaid on economic exchange. It is the ontological precondition for any exchange whatsoever. A customer who does not trust a technician cannot authorize them to touch a system. A technician who does not trust a customer cannot accept the liability of intervention. Without mutual trust, the flywheel does not slow—it ceases to rotate.

This is not metaphor. In physical systems, trust has material consequences. A misdiagnosed marine diesel kills sailors. A improperly sealed HVAC system breeds pathogens. An incorrectly torqued structural bolt shears under load. Trust, in the Technician Paradigm, is the epistemic bridge between *intention* and *outcome*—and that bridge must bear weight.

The digital economy has spent two decades flattening trust into star ratings and review counts. This works when the mediated transaction is low-stakes and reversible: a bad restaurant meal, a delayed package. But physical maintenance is high-stakes and irreversible. You cannot un-fix a broken system. The trust infrastructure must therefore be of a different order entirely—thicker, more contextual, more honest about its own uncertainties.

We call this infrastructure the **Trust Mesh**: a multidimensional, temporally-aware, contextually-indexed network of reputation relationships that reflects the actual epistemology of physical work.

---

## II. Reputation Scoring: The Multidimensional Signal

A single scalar reputation score is a lie. It compresses irreducibly different qualities into one number, destroying the information technicians and customers need to make decisions. The Trust Mesh rejects this compression.

A technician's reputation is a **vector**, not a scalar. The components of this vector correspond to the distinct ontological dimensions of maintenance work:

**Competence** — Can the technician diagnose and repair correctly? This is the most straightforward dimension, but even here, nuance matters. Competence is domain-specific. A technician may be supremely competent in marine diesel systems and dangerously incompetent in marine electrical systems. The competence score is therefore not one number but a sparse matrix: competence[technician][domain_subclass].

**Reliability** — Does the technician arrive when promised, complete work within estimates, and communicate proactively? Reliability is the dimension most correlated with customer satisfaction in the short term, but it is the dimension most easily gamed. A technician who is reliably mediocre will outscore a technician who is brilliantly unpredictable on a flattened metric. The vector representation preserves this distinction.

**Judgment** — Does the technician know when *not* to repair? When to escalate? When to recommend replacement over continued maintenance? This is the dimension most closely tied to the ontology of maintenance as practiced by the Cocapn Technician—the recognition that intervention is not always the correct action. Judgment is the hardest dimension to score because its positive expression is often *inaction*, which leaves no trace. We address this through the verification mechanisms described below.

**Documentation** — Does the technician produce clear, complete, and honest records via field-captain? Documentation is not merely administrative; it is the mechanism by which maintenance knowledge propagates through time and across technicians. A technician who documents well creates positive externalities for every future technician who works on that system. Documentation scores should therefore carry a multiplier effect on overall reputation.

**Mentorship** — Does the technician contribute to the learning pathway of other technicians? This dimension recognizes that the Technician Paradigm is not a collection of independent agents but an ecosystem of interdependent practitioners. Mentorship scores are derived from verifiable teaching activities: supervised apprenticeships, contributed diagnostic patterns, peer review participation.

The composite reputation is therefore:

```
R(t) = Σᵢ wᵢ(t, domain, locale) · Dᵢ(t)
```

Where Dᵢ are the dimension scores and wᵢ are context-dependent weights. A marine emergency dispatch weights competence and reliability heavily; a long-term service contract weights judgment and documentation more. The weights are not fixed—they are negotiated by the specific trust requirements of each transaction.

---

## III. Verification: The Epistemology of Physical Work

How do you know a repair was done correctly? In software, you run tests. In physical systems, you run the system and wait. This temporal gap between intervention and verification is the fundamental epistemological challenge of the Trust Mesh.

We propose a **layered verification architecture** that mirrors the actual ways physical work is validated:

### Layer 1: Instrumental Verification (field-captain)

The technician's instrument captures objective data before, during, and after intervention. Sensor readings, torque values, thermal images, vibration signatures—these form the instrumental layer of verification. This data is cryptographically signed and timestamped, creating an immutable record of what was done and what the system state was at each point.

Critically, field-captain does not verify *correctness*—it verifies *fidelity*. It confirms that the technician's report matches the instrument's record. A technician who reports "replaced impeller, flow rate restored to spec" must produce instrumental evidence that flow rate was below spec before and within spec after. The instrument is the lie detector, not the judge.

### Layer 2: Temporal Verification (time-series performance)

The truest verification of maintenance quality is system performance over time. A repair that holds for six months and then fails is a different quality of repair than one that holds for six years. The Trust Mesh therefore implements **deferred reputation adjustments**: a portion of the reputation impact of any intervention is held in escrow and released (or reclaimed) based on the system's subsequent performance.

This creates a natural incentive for durable repairs over quick fixes. The technician who patches a symptom receives partial reputation immediately but loses it when the underlying problem resurfaces. The technician who addresses root cause receives full reputation as the system continues to perform.

The escrow period is domain-dependent: marine systems might use 12-month cycles, HVAC systems seasonal cycles, structural systems multi-year cycles. The escrow durations are calibrated to the characteristic failure modes of each domain.

### Layer 3: Peer Verification (technician review)

When a second technician works on a system previously serviced by another, they encounter the material consequences of the prior technician's work. This encounter is an inherently honest moment—the second technician has no incentive to misrepresent the quality of prior work, and every incentive to accurately assess it for their own diagnostic purposes.

The Trust Mesh captures these encounters as **peer verification events**. The second technician, upon encountering prior work, records an assessment: was the prior work competent? Was the documentation accurate? Were the right judgments made? These assessments are weighted heavily in the reputation vector because they are the least gameable signal in the system.

To prevent collusion and retaliation, peer verification is anonymized and aggregated. No individual technician sees who assessed their work; they see only the statistical profile of peer assessments over time. A technician who consistently receives poor peer assessments cannot identify which specific encounter produced the negative signal, making targeted retaliation impossible.

### Layer 4: Customer Verification (post-service review)

The customer's perspective is essential but insufficient. Customers can verify reliability and communication directly. They can verify competence only indirectly—the system works or it doesn't. They cannot verify judgment at all, because they lack the expertise to distinguish a correct decision not to intervene from negligence.

Customer reviews therefore contribute primarily to the reliability dimension, secondarily to competence (through a simple "did it work?" signal), and not at all to judgment. This is not a flaw—it is an honest acknowledgment of the epistemic limits of the customer's position.

---

## IV. Dispute Resolution: The Ontology of Causation

When something goes wrong, the question becomes: who is responsible? In physical systems, causation is almost always multifactorial. The technician's repair may have been correct, but the parts were defective. The diagnosis may have been accurate, but the customer delayed authorization. The work may have been flawless, but the operating environment exceeded design parameters.

The Trust Mesh implements a **causal adjudication protocol** that resists the human temptation to find a single responsible party:

### The Three Horizons of Dispute

**Horizon 1: Instrumental Arbitration** — If field-captain data unambiguously resolves the dispute (e.g., the technician documented a pre-existing condition that later failed), the resolution is automated. No human judgment required. The instrument speaks.

**Horizon 2: Peer Panel Adjudication** — When instrumental data is ambiguous, a panel of three senior technicians from the relevant domain reviews the case. They examine the instrumental record, the technician's documentation, the system's history, and the customer's report. They render a judgment that assigns fractional responsibility across all contributing factors.

The panel's judgment does not produce a binary "technician at fault / not at fault" outcome. Instead, it produces a **causal decomposition**: "40% pre-existing condition, 30% technician error in torque specification, 20% parts defect, 10% customer operation outside parameters." The reputation adjustment follows the decomposition: the technician's score is affected only by the portion attributed to their error.

**Horizon 3: Escalation to Stewardship Council** — In rare cases where the peer panel cannot reach consensus or where the dispute involves systemic issues (e.g., a batch of defective parts affecting many technicians), the case escalates to the Stewardship Council—a governance body composed of elected technicians, domain experts, and customer representatives. The Council's rulings establish precedent and may result in systemic changes to verification protocols, reputation algorithms, or escrow durations.

### The Asymmetric Protection Principle

Dispute resolution in the Trust Mesh is deliberately asymmetric: it is harder to damage a technician's reputation than to build it. This principle recognizes that false accusations are cheaper to make than real reputation is to earn, and that the cost of a false negative (driving a competent technician out of the ecosystem) exceeds the cost of a false positive (allowing a marginal technician to remain).

Reputation adjustments from disputes are therefore applied gradually and are subject to appeal. A single complaint cannot catastrophically reduce a technician's score. The system assumes good faith unless a pattern of evidence suggests otherwise.

---

## V. Privacy: The Paradox of Identity

The Technician Paradigm requires a fundamental tension: reputation requires persistence of identity, but persistence of identity creates vulnerability. A technician's reputation history is valuable—to themselves, to customers, to the ecosystem. But that same history is a dossier: where they worked, what systems they serviced, when they were available, what went wrong. In the wrong hands, this is a surveillance dataset.

The Trust Mesh resolves this through **selective pseudonymity**:

### The Dual Identity Architecture

Every technician possesses two linked but separable identities:

**The Professional Identity** — A persistent, public pseudonym associated with all reputation data. This identity accumulates dimension scores, verification history, and peer assessments. It is the identity customers see when evaluating technicians. It is not the technician's legal name.

**The Legal Identity** — The technician's real-world identity, verified through KYC processes and held in escrow by the Trust Mesh infrastructure. This identity is revealed only under specific conditions: dispute resolution requiring legal accountability, regulatory compliance, or mutual consent between technician and customer.

The linkage between professional and legal identity is maintained through zero-knowledge proofs. The system can verify that a professional identity corresponds to a verified legal identity without revealing the legal identity. This allows accountability without surveillance.

### The Right to Reset

A technician who has accumulated a poor reputation may apply for a **professional identity reset**. This is not a clean slate—it is a supervised transition. The technician's new professional identity is marked with a "reset" flag for a probationary period (typically 12 months), during which their work is subject to enhanced verification. The prior identity's history is archived but not publicly visible.

The reset mechanism prevents reputation traps—situations where early failures permanently doom a technician's career while providing transparency to customers during the rebuild period. It acknowledges that humans learn and improve, and that the ecosystem benefits from redemption pathways.

### Data Minimization in field-captain

The technician's instrument is designed to capture diagnostic data, not surveillance data. Field-captain records system states and intervention actions; it does not record location when not actively engaged in service, it does not capture ambient audio or video beyond what is necessary for documentation, and all raw data is processed locally with only derived metrics uploaded to the Trust Mesh.

The technician retains ownership of their raw instrumental data. The Trust Mesh receives only the cryptographic attestations necessary for verification. This ensures that the infrastructure of trust does not become the infrastructure of control.

---

## VI. Cross-Domain Reputation: The Transfer Problem

A marine-certified technician applies to service residential HVAC systems. Should their marine reputation transfer? The naive answer is no—the domains are too different. The sophisticated answer is: it depends on what aspects of reputation we're transferring.

The Trust Mesh implements **domain transfer coefficients** that model the similarity between domains across each reputation dimension:

**Competence transfer** is low between distant domains. Marine diesel competence tells you little about HVAC competence. Transfer coefficient: ~0.1.

**Reliability transfer** is high across all domains. A technician who shows up on time for marine jobs will likely show up on time for HVAC jobs. Transfer coefficient: ~0.8.

**Judgment transfer** is moderate. The specific technical judgments don't transfer, but the *disposition* toward good judgment—knowing when to escalate, when to stop—does. Transfer coefficient: ~0.5.

**Documentation transfer** is high. Good documentation practices are domain-agnostic. Transfer coefficient: ~0.7.

**Mentorship transfer** is moderate. Teaching skill transfers, but domain-specific mentorship does not. Transfer coefficient: ~0.4.

When a technician enters a new domain, their initial reputation in that domain is:

```
R_new(domain) = Σᵢ Tᵢ(domain_source, domain_new) · Dᵢ(domain_source)
```

Where Tᵢ are the transfer coefficients for each dimension. This produces a reasonable prior that is rapidly updated as the technician accumulates domain-specific verification data. The transfer coefficients are themselves empirically calibrated: as the Trust Mesh accumulates data on technicians who cross domains, the coefficients are adjusted to reflect actual transferability.

### The Adjacency Graph

Domains are not independent points—they form an adjacency graph based on shared skills, shared tools, shared safety requirements, and shared system architectures. Marine diesel and marine electrical are adjacent. Marine electrical and residential electrical are moderately