# software-taste

A Socratic agent skill for developing software design intuition through real code — not a linter, not a mechanical reviewer, but a thinking partner that uses your own code as a canvas to explore design philosophy, surface hidden tradeoffs, and refactor through understanding.

Instead of listing violations, it asks one good question at a time, drawn from a distilled corpus of primary sources: Linus Torvalds, Rob Pike, John Ousterhout, Fred Brooks, Rich Hickey, Google SRE, Martin Fowler, Joel Spolsky, Hyrum Wright, Richard Gabriel, NASA/JPL, SQLite, John Carmack, Niklaus Wirth, and mature data-model case studies (W3C PROV, Mux, Rails Active Storage, S3, OneDrive, GitLab).

## What's in here

- [`software-taste/SKILL.md`](software-taste/SKILL.md) — the skill definition: workflow, the 8 review questions, a data-model lens for schema review, and teaching style.
- [`software-taste/REFERENCES.md`](software-taste/REFERENCES.md) — the reference corpus the skill draws from when forming questions or citing a tradition.

## Using it

This follows the emerging convention for portable agent skills (a `SKILL.md` with YAML frontmatter plus supporting reference files). To use it with an agent/CLI that supports this format, copy the `software-taste/` directory into that tool's skills directory.

## Scope

Currently the reference corpus leans toward systems/backend/general software design traditions. Frontend and UI/component design taste (React design principles, CSS architecture, design systems, etc.) is a known gap — contributions welcome.

## License

MIT — see [LICENSE](LICENSE).
