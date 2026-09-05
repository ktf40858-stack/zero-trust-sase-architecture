# SASE, decomposed

SASE (Secure Access Service Edge, Gartner's 2019 coinage) is not one thing — it is the
convergence of networking and security into a single cloud-delivered service. The value is the
convergence; the substance is the components. Here is what SASE is actually made of, what each
piece does, and why delivering them together matters.

## The two halves

SASE = **network** as a service + **security** as a service, from the same cloud edge.

```
                          SASE
            ______________|______________
           |                             |
     Network edge                  Security edge (SSE)
     -------------                  ----------------
     SD-WAN                         SWG   (Secure Web Gateway)
                                    CASB  (Cloud Access Security Broker)
                                    ZTNA  (Zero Trust Network Access)
                                    FWaaS (Firewall as a Service)
                                    DLP   (Data Loss Prevention)
```

The security half has its own name: **SSE (Security Service Edge)** — SASE minus the SD-WAN. If
an organisation already has its WAN sorted and only wants the security stack as a service, SSE is
what they are buying. Knowing that SSE = SASE − SD-WAN is a distinction interviewers use to
separate people who read the marketing from people who understand the model.

## The components

**SD-WAN — the network fabric.**
Software-defined WAN. Steers traffic across whatever links exist (broadband, LTE, MPLS) by
application and policy, from branch to cloud, without backhauling everything to a data centre.
In SASE it is the on-ramp: it gets the traffic to the nearest security edge efficiently.

**SWG — Secure Web Gateway.**
Inspects web traffic. URL filtering, malware scanning, TLS inspection, acceptable-use policy.
The cloud-delivered replacement for the on-prem web proxy — the difference is the user reaches it
wherever they are, instead of being backhauled to the office to browse.

**CASB — Cloud Access Security Broker.**
Sits between users and SaaS applications (Microsoft 365, Salesforce, Google Workspace). Sees and
controls what data goes into and out of sanctioned SaaS, and discovers *unsanctioned* SaaS
("shadow IT"). Enforces things like "you may read from corporate OneDrive but not upload to a
personal one".

**ZTNA — Zero Trust Network Access.**
The per-application access broker described in [ztna-vs-vpn](02-ztna-vs-vpn.md). The VPN
replacement inside SASE: identity-based, per-app, dark applications, continuous verification.

**FWaaS — Firewall as a Service.**
The firewall itself as a cloud service. Layer 3–7 policy, IPS, threat prevention — delivered from
the edge instead of an appliance in a rack. Branches and roaming users get enterprise firewalling
without an enterprise firewall on site.

**DLP — Data Loss Prevention.**
Inspects content in motion for sensitive data (PII, PCI, source code, classified markings) and
blocks or logs it leaving. In SASE it runs at the same edge as everything else, so one DLP policy
covers web, SaaS and private-app traffic instead of three separate DLP products.

## Why converge them

The pre-SASE world ran these as separate boxes and services, each with its own console, its own
policy language, its own choke point. That produced three problems SASE solves:

1. **Backhaul.** Traffic was hairpinned to a central stack for inspection, adding latency and
   cost — brutal once the applications moved to the cloud and the "centre" was no longer where
   the apps were. SASE inspects at an edge near the user, near the cloud.
2. **Policy fragmentation.** "Block this risky app" had to be configured in the proxy, the
   firewall, the CASB and the DLP separately, and they disagreed. SASE is one policy engine,
   one identity, applied across all functions.
3. **The perimeter assumption.** Every separate box assumed a perimeter that no longer exists.
   SASE assumes the user, the app and the data are all outside, and delivers the control to
   wherever they are.

## The one-line test

If someone describes SASE as "a firewall in the cloud", they have one component. SASE is the
**convergence** — SD-WAN plus the SSE security stack, one identity, one policy, one edge. The
convergence is the product; the components are the parts list.
