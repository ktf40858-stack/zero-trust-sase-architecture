# ZTNA versus VPN

VPN and ZTNA both answer "how does a remote user reach an internal application?" They answer it
in fundamentally different ways, and the difference is the whole reason the industry is
migrating.

## The core difference in one line

**A VPN connects you to a network. ZTNA connects you to an application.**

Everything else follows from that.

## Side by side

| | VPN | ZTNA |
|---|---|---|
| What you get access to | a network segment | one specific application |
| Trust model | authenticate once, then trusted on the network | verify continuously, per resource |
| Lateral movement | possible — you are on the network | impossible — you were never on a network |
| Network visibility to the user | sees the subnet, can scan it | sees nothing but the one app, cannot scan |
| Access decision inputs | credentials (+ MFA) at login | identity + device posture + risk, continuously |
| Application discoverability | apps are reachable, therefore attackable | apps are dark — invisible until you are authorized |
| Where enforcement happens | the VPN concentrator (often backhauled) | a broker close to the user (cloud PoP) |
| Blast radius of a stolen credential | the network | one application |

## The lateral-movement problem VPN cannot fix

This is the argument that ends the debate. A VPN, even with MFA, even with a tight firewall
policy, puts the endpoint **on a network**. A compromised endpoint on that network can see other
hosts, scan them, and attack them — the firewall policy narrows this but the endpoint still has a
network position from which to work. Every ransomware post-mortem that starts "the initial access
was a VPN credential" is describing this.

ZTNA removes the network position entirely. The user's device connects to a broker; the broker
connects to the application; the two connections are stitched at the application layer. The user
never receives a route to a network, never gets an IP on the inside, and therefore has nothing to
move laterally *across*. A compromised endpoint under ZTNA can attack the one application it is
authorized for and literally cannot see anything else — the other applications are not merely
firewalled, they are unreachable and undiscoverable.

## "Dark" applications

Under ZTNA, applications are not exposed to the network at all. There is no open port for a
scanner to find, because the application sits behind an outbound-only connector that dials out to
the broker. An attacker scanning the internet — or the internal network — finds nothing to
attack, because there is no listening service exposed. You cannot exploit what you cannot reach,
and you cannot reach what does not accept inbound connections. This alone eliminates a large
class of attacks that VPN-fronted applications remain exposed to.

## Continuous verification

A VPN checks you at login and trusts you for the session — often eight hours. If your device is
compromised at hour two, the VPN does not know and does not care; you are already trusted. ZTNA
re-evaluates continuously: if device posture degrades mid-session (EDR disabled, a new critical
CVE, anomalous behaviour), access is revoked or stepped up *during* the session. Trust is a
running assessment, not a gate you passed once.

## What this means for the two lab repos

The [FortiGate SSL VPN lab](https://github.com/ktf40858-stack/fortigate-secure-remote-access)
builds VPN remote access as well as it can be built — per-role, MFA, least-privilege policy. This
document explains why, done perfectly, it is still a transitional technology: it still puts the
user on a network. The [segmentation lab](https://github.com/ktf40858-stack/palo-alto-segmentation-lab)
is the internal counterpart — it limits lateral movement for the world that still has VPNs and
flat-ish networks. ZTNA is where both are headed: segmentation so fine-grained and identity-bound
that "the network" stops being a place you can be.

## The honest caveat

ZTNA is not a drop-in for every case. Server-to-server traffic, legacy apps that expect a network
path, low-latency or non-web protocols, and things that must work when the broker is unreachable
all still need thought — which is why real migrations are phased and hybrid for years, not a
cutover. That phasing is [the roadmap](04-migration-roadmap.md).
