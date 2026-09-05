# Acronym field guide

The Zero Trust / SASE space is acronym-dense to the point of parody. This is the working
vocabulary, defined so the architecture docs read cleanly and so you can hold your own when a
vendor or an interviewer strings six of them together.

| Term | Stands for | In one line |
|---|---|---|
| **Zero Trust** | — | Never trust based on network location; verify identity + device continuously, per resource |
| **NIST 800-207** | — | The authoritative Zero Trust standard; seven tenets, PDP/PEP model |
| **SASE** | Secure Access Service Edge | Networking + security converged into one cloud-delivered edge service |
| **SSE** | Security Service Edge | SASE minus the SD-WAN — the security half on its own |
| **ZTNA** | Zero Trust Network Access | Per-application access broker; the VPN replacement inside SASE |
| **SD-WAN** | Software-Defined WAN | Policy-driven WAN steering across any transport; the SASE on-ramp |
| **SWG** | Secure Web Gateway | Cloud web proxy — URL filtering, malware scan, TLS inspection |
| **CASB** | Cloud Access Security Broker | Visibility and control over SaaS use, sanctioned and shadow |
| **FWaaS** | Firewall as a Service | The firewall delivered from the cloud edge, not a rack appliance |
| **DLP** | Data Loss Prevention | Inspects content in motion, blocks sensitive data leaving |
| **PDP** | Policy Decision Point | Where the access decision is made (PE + PA) |
| **PEP** | Policy Enforcement Point | Where the decision is enforced, in the traffic path |
| **PE / PA** | Policy Engine / Administrator | The two halves of the PDP: decide / execute |
| **IdP** | Identity Provider | The authoritative identity source (Entra ID, Okta) |
| **MFA** | Multi-Factor Authentication | Something you know + have/are; baseline for remote access |
| **mTLS** | mutual TLS | Both ends present certificates — used for secured service-to-service |
| **EDR** | Endpoint Detection and Response | Endpoint agent providing the device-posture signal |
| **MDM** | Mobile Device Management | Device enrolment and compliance state |
| **Micro-segmentation** | — | Segmentation fine enough that each workload is its own segment |
| **Dark application** | — | An app with no inbound exposure, reachable only via broker |
| **Backhaul** | — | Hairpinning traffic to a central stack for inspection — the thing SASE removes |
| **Split tunnel** | — | Only some traffic goes through the tunnel; the rest goes direct |
| **CISA ZTMM** | Zero Trust Maturity Model | CISA's 5-pillar model for rating how far along an org is |
| **SSE vs SASE** | — | If someone conflates them: SSE is the security stack, SASE adds the network (SD-WAN) |

## The three you will be tested on

If an interview probes vocabulary, it is almost always these distinctions:

1. **SASE vs SSE** — SSE = SASE − SD-WAN. SSE is the security service edge; SASE bundles the
   network edge with it.
2. **ZTNA vs VPN** — ZTNA connects you to an *application*; VPN connects you to a *network*. See
   [ztna-vs-vpn](../docs/02-ztna-vs-vpn.md).
3. **PDP vs PEP** — the PDP *decides* (central), the PEP *enforces* (distributed, in the path).
   Getting these backwards is the tell that someone learned the words but not the model.
