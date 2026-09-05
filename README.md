# Zero Trust & SASE Architecture

A reference architecture and decision guide for replacing perimeter VPN with identity-based
access — ZTNA over VPN, SASE as the delivery model. This is the design layer that sits above
the [Palo Alto segmentation](https://github.com/ktf40858-stack/palo-alto-segmentation-lab) and
[FortiGate remote-access](https://github.com/ktf40858-stack/fortigate-secure-remote-access)
labs and explains what they are building toward.

It backs the **Palo Alto SASE** and **Cloud Security** certifications with the thing a
certification alone does not show: the architectural judgement to apply them — what to move
first, what breaks, and how to phase it without a big-bang cutover.

> This is an architecture repo. It carries designs, decision frameworks, mappings to NIST, and
> annotated diagrams — not device configs (those live in the two lab repos above). It is the
> "why and in what order", they are the "how".

---

## The one-sentence thesis

The network perimeter stopped being the security boundary the day the users, the applications,
and the data all moved outside it — so the boundary has to move to **identity**, verified
continuously, granting access to one resource at a time. That is Zero Trust. SASE is how you
deliver it: security controls as a cloud service the user reaches wherever they are.

## What is in here

| Path | Contents |
|---|---|
| [`docs/01-zero-trust-principles.md`](docs/01-zero-trust-principles.md) | The NIST 800-207 tenets, in plain terms, mapped to controls |
| [`docs/02-ztna-vs-vpn.md`](docs/02-ztna-vs-vpn.md) | Why ZTNA replaces VPN, the concrete differences, what VPN can't fix |
| [`docs/03-sase-components.md`](docs/03-sase-components.md) | SASE decomposed: SD-WAN, SWG, CASB, ZTNA, FWaaS, DLP — what each does |
| [`docs/04-migration-roadmap.md`](docs/04-migration-roadmap.md) | A phased path from VPN + castle-and-moat to ZTNA/SASE, without a big bang |
| [`docs/05-reference-architecture.md`](docs/05-reference-architecture.md) | A worked target-state design for a mid-size org |
| [`diagrams/`](diagrams/) | The architecture and flow diagrams (SVG, readable in both light and dark) |
| [`reference/nist-800-207-mapping.md`](reference/nist-800-207-mapping.md) | Each NIST tenet → the SASE/ZTNA control that implements it |
| [`reference/glossary.md`](reference/glossary.md) | The acronym field guide — ZTNA, SWG, CASB, FWaaS, SSE, SD-WAN |

## The core ideas it argues

1. **Never trust, always verify.** No implicit trust from network location. A packet from the
   corporate LAN is trusted exactly as much as one from a coffee shop — which is to say, not at
   all until identity and device posture are verified.
2. **Per-application access, not network access.** ZTNA connects a user to *an application*, not
   to a network. There is no "inside" to move laterally across, because the user was never put
   on a network.
3. **Continuous verification.** Trust is re-evaluated during the session on signals — device
   health, user behaviour, risk — not granted once at login and held for eight hours.
4. **The policy decision point is central; the enforcement points are everywhere.** SASE puts
   the enforcement in the cloud, close to the user, so the control follows the user instead of
   the user being backhauled to the control.

## Why this repo exists in this portfolio

The other five labs are hands-on: switches, firewalls, detections, configs. This one is the
architecture that ties them into a coherent security posture and a coherent story: "I can
configure the segmentation and the remote access, *and* I understand the model they serve and
how an organisation gets there from where it is today." That combination — the hands and the
head — is what a Network & Cloud Security Engineer role is actually asking for.

## Author

Kodjo Apedoh — Network & Cloud Security · Arlington, VA
CCNA · Fortinet NSE · **Palo Alto SASE & Cloud Security** · [LinkedIn](https://www.linkedin.com/in/kodjo-apedoh-03030990/) · [Other labs](https://github.com/ktf40858-stack)

## License

MIT — see [LICENSE](LICENSE).
