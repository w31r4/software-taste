# software-taste

A Socratic agent skill for developing software design intuition through real code — not a linter, not a mechanical reviewer, but a thinking partner that uses your own code as a canvas to explore design philosophy, surface hidden tradeoffs, and refactor through understanding.

Instead of listing violations, it asks one good question at a time, drawn from a distilled corpus of primary sources: Linus Torvalds, Rob Pike, John Ousterhout, Fred Brooks, Rich Hickey, Google SRE, Martin Fowler, Joel Spolsky, Hyrum Wright, Richard Gabriel, NASA/JPL, SQLite, John Carmack, Niklaus Wirth, Eric Evans, Martin Kleppmann, Michael Stonebraker, mature data-model case studies (W3C PROV, Mux, Rails Active Storage, S3, OneDrive, GitLab), and frontend/interface design traditions — Don Norman, Heydon Pickering, Brad Frost, Dan Abramov/React, Rich Harris, Adam Wathan, Anthony Fu (antfu), and Alex Russell.

## What's in here

- [`software-taste/SKILL.md`](software-taste/SKILL.md) — the skill definition: workflow, the 8 review questions, a data-model lens for schema review, and teaching style.
- [`software-taste/REFERENCES.md`](software-taste/REFERENCES.md) — the reference corpus the skill draws from when forming questions or citing a tradition.

## Using it

This follows the emerging convention for portable agent skills (a `SKILL.md` with YAML frontmatter plus supporting reference files). To use it with an agent/CLI that supports this format, copy the `software-taste/` directory into that tool's skills directory.

## Scope

Frontend and backend taste are treated as one workshop, not two — the reference corpus spans systems/backend traditions and frontend/interface design traditions (state and effects, reactivity models, CSS architecture, accessibility as semantics, design systems). Contributions extending either side, or adding traditions not yet covered, are welcome.

## License

MIT — see [LICENSE](LICENSE).
