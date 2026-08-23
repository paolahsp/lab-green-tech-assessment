# Intellectus Green Retrofit Audit Notes

## Path and artifact

**Path:** B - Green retrofit audit  
**Artifact:** [Social Impact Consulting Intelligence](https://github.com/paolahsp/Social-Impact-Consulting-Intelligence)  
**Primary audience:** CTO / Operations Lead

Intellectus is a consultant-facing, human-reviewed diagnostic system for social-impact organizations. A web intake triggers an n8n workflow that gathers bounded public evidence, runs specialist analysis, validates the result, and returns a structured report to a React review interface. The current repository proves a live parent-to-child workflow boundary but does not yet prove production-level carbon, energy, hardware, or retention measurement.

## Functional unit

**One unit of value (R) is one completed, evidence-grounded diagnostic report for one social-impact organization, ready for consultant review.**

The proposed measurement boundary includes:

1. Web intake and response delivery.
2. n8n parent and child workflow execution.
3. Public web search and website extraction.
4. Local RAG retrieval and specialist analysis.
5. Any model/API inference used in the production version.
6. Temporary storage and network transfer required to produce the report.

The consultant's laptop and downstream client use are excluded from the first pilot boundary and should be disclosed as exclusions.

## Retrofit audit template

**Primary user journey:** A consultant enters an organization name, website, and country; the workflow researches public evidence, separates facts from unknowns, produces analysis and recommendations, and returns a report for human review.

**Likely hotspots:**

- Repeated web search and website extraction across runs.
- Evidence-gap research that may revisit already processed sources.
- Multi-step orchestration and possible redundant model calls.
- Long prompts, repeated context, and oversized response payloads.
- Storage or retention of raw pages, intermediate evidence, and execution logs.
- Unknown hosting region, model size, hardware utilization, and grid intensity.

## Prioritized green opportunities

1. **Cache and deduplicate public evidence.** Key by normalized URL, content hash, and freshness window; reuse unchanged extraction results.  
   **Pillars:** Energy, hardware, measurement.  
   **Pattern:** [Cache static data](https://patterns.greensoftware.foundation/development/data-handling/cache-static-data/).

2. **Add conditional routing before model invocation.** Use deterministic code for validation, formatting, and simple routing; call a model only when uncertainty or synthesis requires it.  
   **Pillars:** Energy, hardware, measurement.  
   **Pattern:** [Optimize agent orchestration to reduce unnecessary model calls](https://patterns.greensoftware.foundation/development/optimize-agent-orchestration-reduce-model-calls/).

3. **Right-size models by task and verify quality.** Use a smaller or task-specific model for extraction/classification, with sampled evaluation and fallback to a larger model when quality thresholds fail.  
   **Pillars:** Energy, hardware, measurement.  
   **Pattern:** [Use right-sized and energy-efficient AI models](https://patterns.greensoftware.foundation/development/right-sized-energy-efficient-ai-models/).

4. **Reduce context, payload, and retention.** Send only relevant evidence snippets, compress transfer where appropriate, store structured claims instead of duplicate raw pages, and apply retention windows.  
   **Pillars:** Energy, hardware, measurement.  
   **Pattern:** [Reduce transmitted data](https://patterns.greensoftware.foundation/catalog/cloud/reduce-transmitted-data/).

5. **Keep execution demand-driven and test carbon-aware placement.** Preserve webhook/event-driven execution, eliminate idle workers where feasible, and compare suitable regions for non-urgent research while respecting latency, data protection, and service availability.  
   **Pillars:** Carbon, energy, hardware.  
   **Patterns:** [Use on-demand execution for AI and agent workloads](https://patterns.greensoftware.foundation/architecture/) and [Use carbon-aware scheduling and region selection for AI workloads](https://patterns.greensoftware.foundation/operations/carbon-aware-ai-scheduling).

## What to measure for 1-2 weeks

| Metric | Unit | Why it matters | Initial success signal |
| --- | --- | --- | --- |
| External research calls | Calls per R | Finds duplicated retrieval | At least 25% fewer repeat fetches on recurring sources |
| Model invocations | Calls per R | Shows orchestration efficiency | No unnecessary call for deterministic steps |
| Input/output tokens | Tokens per R | Proxy for inference work and cost | Lower median with quality held constant |
| Cache hit rate | % eligible fetches | Proves reuse | At least 30% on repeat organizations/sources |
| Payload transferred | MB per R | Captures data movement | Lower median without missing evidence |
| End-to-end latency | Seconds per R | Protects user experience | No material regression; target improvement |
| Cost | EUR per R | Practical compute proxy | Lower median cost with quality held constant |
| Quality review | Pass rate / citation errors | Prevents blind optimization | No drop beyond agreed tolerance |
| Region and grid intensity | Region + gCO2e/kWh source | Enables carbon-aware comparison | Region documented for every measured run |

## Before/after hypotheses

- A 24-hour cache for unchanged public pages should reduce repeated fetches by at least 25% in a recurring-source pilot.
- Conditional routing should reduce model calls used only for validation, formatting, or simple branching.
- Evidence snippets and structured claim reuse should reduce median input tokens per R without reducing citation coverage.
- On-demand execution should keep idle capacity close to zero for this low-volume consultant workflow.

These are testable hypotheses, not observed results.

## Risks and trade-offs

- Caching can serve stale evidence; freshness windows and source timestamps are required.
- Smaller models can reduce quality; use a representative evaluation sample and fallback rules.
- Carbon-aware region changes may conflict with latency, data residency, contractual, or provider constraints.
- Compression and deduplication add implementation complexity and can make debugging harder.
- Cost and tokens are useful proxies but are not a verified carbon footprint.
- Offsets do not reduce the SCI numerator and should not replace direct efficiency work.

## Honest conclusion

**Proceed with a measured retrofit pilot.** Intellectus already has a bounded workflow and human review, which makes per-report measurement feasible. The current evidence supports likely hotspots and practical interventions, but it does not support a carbon-neutral claim or a quantified SCI score. The next defensible step is telemetry per R, followed by one controlled before/after comparison.

