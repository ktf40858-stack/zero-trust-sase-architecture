# NIST SP 800-207 tenets → implementing controls

Each Zero Trust tenet mapped to the SASE/ZTNA control that implements it, and to where in this
portfolio it shows up. This is the table that connects the standard to the practice.

| # | NIST 800-207 tenet | Implementing control | In this portfolio |
|---|---|---|---|
| 1 | All data sources and services are resources | Resource-level (per-app) policy, not subnet policy | ZTNA per-app access; Palo Alto per-host rules |
| 2 | All communication secured regardless of location | Encrypted transport everywhere; no trusted internal segments | IPsec/TLS everywhere; no "inside = trusted" |
| 3 | Access granted per-session | Per-session authorization at the PDP | ZTNA broker session authorization |
| 4 | Access from dynamic policy (identity + device + attributes) | Policy engine consuming IdP + posture + risk | Conditional access; posture-gated ZTNA |
| 5 | Enterprise monitors integrity/posture of all assets | Device posture assessment (MDM/EDR), continuous | ZTNA device-compliance gating |
| 6 | Auth/authz dynamic and enforced before access | Verify-then-connect, continuous re-auth, MFA | ZTNA continuous verification; MFA everywhere |
| 7 | Collect all the information, use it to improve posture | Telemetry to SIEM, feedback into policy | [SOC Tier 1 detection lab](https://github.com/ktf40858-stack/soc-tier1-detection-lab) |

## The logical model, restated against the standard

NIST 800-207's core components and the products that fill each role:

| NIST component | Function | Fulfilled by |
|---|---|---|
| Policy Engine (PE) | makes the grant/deny decision | the SASE/ZTNA policy engine |
| Policy Administrator (PA) | executes the decision, manages the session | the ZTNA control plane |
| Policy Enforcement Point (PEP) | sits in the path, enforces | SASE PoP / ZTNA connector |
| Policy Decision Point (PDP) | PE + PA together | the cloud policy plane |

The PE draws on the supporting signals NIST names: identity management, device posture, threat
intelligence, SIEM data, data-access policy, and PKI. A SASE platform bundles most of these; the
IdP and the SIEM are the two an organisation almost always already owns and integrates.

## Related frameworks worth naming

- **CISA Zero Trust Maturity Model** — five pillars (Identity, Devices, Networks, Applications,
  Data) each rated Traditional → Initial → Advanced → Optimal. Useful for *assessing where an org
  is*, where 800-207 defines *what the target is*. The [migration roadmap](../docs/04-migration-roadmap.md)
  is essentially a path up this maturity model.
- **DoD Zero Trust Reference Architecture** — the defence-sector elaboration, relevant to the
  federal-integrator market in Virginia this portfolio targets. Adds seven pillars and specific
  capability activities; a cleared employer will expect familiarity with it by name.
- **Gartner SASE / SSE** — the market framing (SSE = SASE − SD-WAN) from
  [sase-components](../docs/03-sase-components.md).

Knowing that 800-207 defines the model, CISA measures the maturity, DoD elaborates it for
defence, and Gartner frames the market is the map that keeps the acronyms from blurring together.
