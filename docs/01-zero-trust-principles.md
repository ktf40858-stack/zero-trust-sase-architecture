# Zero Trust principles (NIST SP 800-207, in plain terms)

Zero Trust is not a product and there is no "Zero Trust appliance" to buy — anyone selling one
is selling you a component. It is an architectural model, and the authoritative definition is
NIST SP 800-207. Here are its seven tenets, translated out of standards language, each with the
control that actually implements it.

## The seven tenets

**1. All data sources and computing services are resources.**
There is no "the network" to be on. A database, a SaaS app, a print server, an API — each is a
resource, and access to each is decided on its own. *Control: resource-level policy, not
subnet-level.*

**2. All communication is secured regardless of network location.**
Being on the corporate LAN grants nothing. Traffic from inside is encrypted and authenticated
exactly like traffic from the internet, because "inside" is not a trust signal. *Control:
mutual TLS / encrypted transport everywhere; no unencrypted "trusted" internal segments.*

**3. Access to individual resources is granted per-session.**
Trust is not a badge you get at the door and keep all day. Each session is evaluated, and access
to one resource does not imply access to the next. *Control: per-session authorization at a
policy decision point.*

**4. Access is determined by dynamic policy** — identity, device state, and other attributes.
The decision uses more than "who are you": it uses what device you are on, whether it is
patched, where you are, what time it is, how you are behaving. *Control: policy engine consuming
identity + device posture + risk signals.*

**5. The enterprise monitors the integrity and security posture of all assets.**
No device is trusted because it is corporate-owned. Its actual current health — patch level, EDR
status, configuration — is measured and fed into the access decision. *Control: device posture
assessment, continuous.*

**6. All resource authentication and authorization are dynamic and strictly enforced before
access.** Verify, then connect — never connect, then verify. Authentication is continuous, not a
one-time gate. *Control: re-authentication and re-authorization during the session, MFA.*

**7. The enterprise collects as much information as possible** about asset state, traffic, and
access, and uses it to improve posture. Zero Trust is a feedback loop, not a static config.
*Control: telemetry into a SIEM/analytics that tunes the policy — this is where the
[SOC detection lab](https://github.com/ktf40858-stack/soc-tier1-detection-lab) plugs in.*

## The logical components

NIST models the decision as three parts, and every ZTNA/SASE product maps onto them:

```
        signals (identity, device posture, risk, threat intel)
                          |
                          v
   [ Policy Decision Point (PDP) ]  -- the policy engine + policy administrator
                          |
                 grant / deny / step-up
                          |
                          v
   [ Policy Enforcement Point (PEP) ]  -- sits in the traffic path, enforces the decision
                          |
              user  <-->  PEP  <-->  resource
```

- The **PDP** decides. It is centralised, and it consumes signals.
- The **PEP** enforces. It is distributed — as many as you need, close to the resources and the
  users. In SASE, the PEPs live in the cloud provider's points of presence.

The user and the resource never talk directly. Every session goes through a PEP that enforces
what the PDP decided, for that user, that device, that resource, this session.

## What Zero Trust is not

- **Not "VPN with MFA".** MFA on a VPN is good and still puts you on a network. Zero Trust does
  not put you on a network at all.
- **Not a single product.** It is a model implemented by several components working together.
- **Not a one-time project.** Tenet 7 makes it a continuous loop. "We did Zero Trust last year"
  is a category error.
- **Not all-or-nothing.** You migrate to it resource by resource — which is the subject of
  [the migration roadmap](04-migration-roadmap.md).

## Why an interviewer values this

The market is saturated with "Zero Trust" marketing. Being able to name the NIST tenets, map
them to real controls, and separate the model from the products that implement it is what
distinguishes someone who understands the architecture from someone who has read a vendor
datasheet. The certifications prove the products; this proves the model.
