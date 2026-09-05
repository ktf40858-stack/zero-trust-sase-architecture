# Diagrams

Architecture and flow diagrams for the docs. Rendered as inline SVG so they stay crisp and
legible in both light and dark themes, and version cleanly in git (a diff on an SVG is
reviewable; a diff on a PNG is not).

| File | Shows | Referenced by |
|---|---|---|
| `pdp-pep-model.svg` | The NIST PDP/PEP decision-and-enforcement model | [01-zero-trust-principles](../docs/01-zero-trust-principles.md) |
| `sase-components.svg` | SASE decomposed into network edge + SSE stack | [03-sase-components](../docs/03-sase-components.md) |
| `reference-architecture.svg` | The mid-size-org target state | [05-reference-architecture](../docs/05-reference-architecture.md) |

The ASCII diagrams embedded directly in the docs are the authoritative version for reading in a
terminal or on GitHub; these SVGs are the presentation version for a slide or a portfolio page.

> These are placeholders in this commit — the ASCII versions in the docs carry the full content.
> The SVGs are the next iteration, drawn to be theme-aware (a neutral stroke that reads on both
> backgrounds, no baked-in white fill).
