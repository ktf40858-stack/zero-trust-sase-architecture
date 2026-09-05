# Reference architecture — a mid-size org, target state

A worked target-state design, so the principles land on something concrete. The subject: a
~500-person organisation with a headquarters, three branches, a hybrid workforce, workloads
split between a data centre and a public cloud, and the usual SaaS estate. Every real
architecture differs; the value is the reasoning, which transfers.

## The picture

```
   Roaming users        Branches            HQ users
   (managed laptops)    (SD-WAN edge)       (SD-WAN edge)
        |                    |                   |
        |  FortiClient/ZTNA  |  SD-WAN on-ramp   |
        v                    v                   v
   ============================================================
                    SASE cloud edge (PoPs)
     [ ZTNA broker ] [ SWG ] [ CASB ] [ FWaaS ] [ DLP ]
                    one policy engine, one identity
   ============================================================
        |                    |                   |
        v                    v                   v
   Private apps          SaaS                Internet
   (dark, via            (M365, Salesforce   (inspected,
    connectors)          — via CASB)          filtered)
        |
   +----+--------------------------+
   |                               |
   Data centre                Public cloud (VPC)
   micro-segmented            micro-segmented
   (tag-based policy)         (tag-based policy)
```

## The decisions, and why

**Identity provider: one, authoritative, with MFA and conditional access.**
Entra ID or Okta as the single source of identity. Every access decision — ZTNA, SWG, CASB —
consumes it. Conditional access (risk-based, device-compliance-gated) is the policy engine's
richest signal source. Everything else is built on this; it is chosen first and it is not
compromised on.

**Private application access: ZTNA, apps dark behind connectors.**
No inbound exposure. Each private app (in the DC or the cloud VPC) sits behind an outbound-only
connector dialing the broker. Users reach an app, never a network. This retires the remote-access
VPN. Platform: whichever the team is certified and staffed on — Prisma Access, Zscaler, or the
Fortinet ZTNA fabric — the architecture is platform-independent, the operations are not.

**Internet and SaaS: SWG + CASB + DLP at the edge, one policy.**
Roaming and branch traffic reaches the nearest PoP for web filtering, SaaS control and DLP.
No backhaul to HQ. One DLP policy spans web, SaaS and private-app egress.

**Branch connectivity: SD-WAN on-ramp to the SASE edge.**
Branches get an SD-WAN edge that steers traffic to the nearest PoP over commodity broadband,
with MPLS retired as circuits expire. Branch firewall appliances retire into FWaaS at refresh.

**Internal workloads: micro-segmentation, tag/identity-based.**
Inside the DC and the cloud VPC, "on the network" grants nothing. Policy follows the workload by
tag (role:app, role:db), not by subnet — so a workload's access is determined by what it is, not
where its IP lands. This is the [Palo Alto segmentation lab](https://github.com/ktf40858-stack/palo-alto-segmentation-lab)'s
Dynamic Address Group model at production scale.

**Telemetry: everything into the SIEM, feeding back into policy.**
Access logs, posture events, edge traffic, DLP hits — all into the SIEM/analytics. This is
Tenet 7, and it is where the [SOC Tier 1 detection lab](https://github.com/ktf40858-stack/soc-tier1-detection-lab)
sits in the whole picture: the loop that turns collected signal into tuned policy and detections.

## Mapping the portfolio onto the architecture

This is the point of the whole repo set — six labs, one coherent architecture:

| Portfolio repo | Where it lives in this diagram |
|---|---|
| [palo-alto-segmentation-lab](https://github.com/ktf40858-stack/palo-alto-segmentation-lab) | Internal micro-segmentation, DC + cloud VPC |
| [fortigate-secure-remote-access](https://github.com/ktf40858-stack/fortigate-secure-remote-access) | The VPN this design is migrating *off* — and the transitional state most orgs are in |
| [soc-tier1-detection-lab](https://github.com/ktf40858-stack/soc-tier1-detection-lab) | The telemetry-to-detection loop, Tenet 7 |
| [network-config-compliance](https://github.com/ktf40858-stack/network-config-compliance) | Continuous config assurance on the network devices underneath it all |
| [l2-attacks-and-mitigations](https://github.com/ktf40858-stack/l2-attacks-and-mitigations) | The Layer 2 hardening the whole model still rests on — Zero Trust does not remove the wire |
| this repo | The architecture that ties them together |

## The honest limitations

- **Availability of the edge.** If the SASE PoP or the identity provider is unreachable, access
  is impacted. Real designs need break-glass paths and think hard about the IdP as a single point
  of failure.
- **Vendor lock-in.** A converged SASE platform is a deep commitment to one vendor's policy
  model. The mitigation is keeping identity and inventory vendor-neutral so the edge is
  replaceable.
- **Cost and change.** This is a multi-year program with real licensing and real
  organisational change. The [roadmap](04-migration-roadmap.md) exists because the target state
  is not something you buy on a Tuesday.

Stating these is part of the competence. An architecture presented without its failure modes is
a sales pitch, not a design.
