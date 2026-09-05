# Migration roadmap: from castle-and-moat to ZTNA/SASE

Nobody arrives at Zero Trust by cutover. Every real migration is phased, runs hybrid for years,
and is driven by picking the highest-risk, highest-value resources first. This is a defensible
sequence — the point is not the specific quarters, it is the ordering logic and knowing what
breaks at each step.

## Phase 0 — Know what you have (weeks, not months, but do not skip it)

You cannot write per-resource policy for resources you have not inventoried. Before touching
architecture:

- **Application inventory.** Every app, who uses it, where it lives, what it talks to. The
  East-West dependency map — the one the [segmentation lab](https://github.com/ktf40858-stack/palo-alto-segmentation-lab)'s
  deny-all rule surfaces the hard way — is gold here.
- **Identity foundation.** A single authoritative identity provider (Entra ID, Okta) with MFA.
  Zero Trust is identity-centric; if identity is fragmented, nothing above it works. **This is
  the true prerequisite** — an org with clean identity can move fast; one without cannot start.
- **Device posture capability.** Some way to assess device health (MDM, EDR). Tenet 5 needs it.

Skipping Phase 0 is the number-one reason Zero Trust programs stall: they try to write dynamic
policy on top of an identity and inventory mess.

## Phase 1 — Replace remote-access VPN with ZTNA for one app group

The highest-value first move, because remote-access VPN is the most-attacked entry point.

- Pick a **web app used by a bounded group** (contractors are ideal — external, limited, and the
  group whose VPN access scares you most).
- Stand up ZTNA (Prisma Access, Zscaler Private Access, or the platform you are certified on) in
  front of it. Deploy the outbound connector; the app goes dark.
- Move that group off the VPN for that app. Keep the VPN running for everything else.
- **What breaks:** the app that expected a network path and a source IP from the VPN pool now
  sees the broker; IP allow-lists inside the app need updating. Legacy thick-client apps may not
  work through an HTTP-oriented broker — note them for Phase 3.

Deliverable: one group, one app, no VPN, and a lateral-movement path closed.

## Phase 2 — Route traffic through the security edge (SSE)

Now the security stack, for the same and growing population.

- **SWG** for web traffic — replace the on-prem proxy / bring roaming users under web filtering.
- **CASB** for the sanctioned SaaS (start with the email/collaboration suite — highest data
  concentration).
- **FWaaS** for branch egress, retiring branch firewall appliances as their refresh comes due.
- **What breaks:** TLS inspection breaks certificate-pinned apps and some thick clients; you will
  build an inspection bypass list, and keeping that list tight is an ongoing discipline. Latency
  complaints if the PoP selection is wrong for your user geography.

Deliverable: users are inspected wherever they are, not only at the office.

## Phase 3 — Expand ZTNA to the application estate, shrink the VPN

- Migrate app groups to ZTNA in order of risk and feasibility. Web apps first, then the
  thick-client and non-HTTP apps that need the ZTNA platform's TCP/UDP tunnelling.
- Each migrated app lets you tighten the VPN's reach until the VPN serves only the stragglers.
- **What breaks:** server-to-server and machine-to-machine flows are not remote-user ZTNA — they
  need their own micro-segmentation (this is where the segmentation lab's tag-based Dynamic
  Address Groups come in). Do not try to force ZTNA onto east-west server traffic.

## Phase 4 — Internal micro-segmentation and continuous verification

- Apply Zero Trust *inside* — micro-segment the data centre / cloud workloads so that "on the
  network" grants nothing internally either. Identity/tag-based segmentation, not subnet-based.
- Wire device posture and risk signals into the policy engine so access adapts mid-session
  (Tenet 6). Feed access and traffic telemetry into the SIEM (Tenet 7) — the
  [SOC detection lab](https://github.com/ktf40858-stack/soc-tier1-detection-lab) is this loop.
- **What breaks:** over-tight internal segmentation takes down undocumented app dependencies —
  same discovery-then-enforce discipline as the segmentation lab, at larger scale.

## Phase 5 — Decommission the VPN, operate the loop

- The VPN carries nothing that has not moved; retire it.
- Zero Trust is now steady-state: posture is monitored, policy adapts, telemetry tunes it. There
  is no "done" — Tenet 7 makes it operational, not a finished project.

## The sequencing logic, extracted

The specific phases matter less than three rules that decide the order:

1. **Identity and inventory before architecture.** No dynamic policy without them.
2. **Highest-attacked surface first.** Remote-access VPN before internal segmentation, because
   that is where the intrusions start.
3. **Discover before you enforce, every time you add a boundary.** The deny-all rule finds the
   dependency nobody documented; find it in log-only mode before it finds you in an outage.

Being able to state those three, and name what breaks at each step, is what turns "we should do
Zero Trust" into a plan an organisation can actually fund and survive.
