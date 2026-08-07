# Software Taste — Reference Corpus

Distilled from primary sources. Agent should draw from these when asking questions or citing traditions during the Socratic workshop.

---

## 1. Linus Torvalds — Good Taste & Linux Kernel Style

**Core insight (linked-list example):** Good taste means rewriting code so that special cases disappear and become the normal case. The "tasteful" linked-list removal uses an indirect pointer (`list_item **p = &head`) to unify the head-removal case with the general case — eliminating the `if (prev) ... else ...` branch entirely.

> "I don't want you to understand why it doesn't have the if statement. I want you to understand that sometimes you can see a problem in a different way and rewrite it so that a special case goes away and becomes the normal case, and that's good code." — Linus Torvalds

**Kernel coding style principles:**
- 8-character indentation forces you to keep nesting shallow; if you need >3 levels, your function is broken
- Functions should be short (one screenful), do one thing, and do it well
- Use `goto` for centralized error cleanup (one exit path with cleanup labels) — this is the *tasteful* error handling pattern in C
- Naming: global functions get descriptive names; local vars can be short (`i`, `tmp`). No Hungarian notation, no CamelCase
- Data structures are more important than algorithms: "Bad programmers worry about the code. Good programmers worry about data structures and their relationships."
- Avoid `inline` keyword abuse — let the compiler decide; premature inlining hides complexity
- Function return values: if the function returns whether it succeeded/failed, use 0 for success (Unix convention). If it computes a result, return the result (NULL on failure)

---

## 2. Rob Pike — Notes on Programming in C

**Variable naming:**
- Length is not a virtue; clarity is. `maxphysaddr` for a global, `np` for a local pointer — context fills in the rest
- Minimum-length, maximum-information names. Let context disambiguate
- Consistency matters more than any single naming rule

**Pointers as notation:**
- A pointer is a name for exactly one object — cleaner than array[index] which requires proving index is valid
- Pointer notation reduces ink and compiler effort vs complex index expressions
- Correct use of pointers simplifies code; they are "sharp tools used well"

**Procedure vs function names:**
- Procedures reflect what they DO; functions reflect what they RETURN
- `validsize(x)` >> `checksize(x)` because the latter doesn't tell you what `true` means

**Comments:**
- Err on the side of eliminating comments
- If your code needs a comment to be understood, rewrite it
- Use comments only as introduction to what follows, or to explain global data
- Never: banner comments, cute typography, restating the code

**Complexity — Pike's 6 Rules:**
1. You can't tell where a program will spend its time. Don't optimize without measuring
2. Measure. Don't tune until you've measured
3. Fancy algorithms are slow when n is small, and n is usually small
4. Fancy algorithms are buggier and harder to implement. Use simple algorithms and simple data structures
5. **Data dominates.** If you've chosen the right data structures and organized things well, the algorithms will almost always be self-evident
6. There is no Rule 6

**Programming with data:**
- Encode complexity in data (tables) rather than code (if-statements)
- Data-driven programs + function pointers = the heart of OO programming without the ceremony
- "I cannot recommend an implementation style more highly. All the programs I have organized this way have survived comfortably after much development"

---

## 3. Go Code Review Comments

**Interface design:**
- Interfaces belong in the package that USES them, not the package that implements them
- Don't define interfaces before they are used — without realistic usage examples, you can't know what methods belong
- Don't define interfaces "for mocking" on the implementor side. Return concrete types
- Implementors return concrete types; consumers define interfaces

**Naming:**
- Variable names should be short. The further from declaration, the more descriptive
- Package names: avoid `util`, `common`, `misc`. The package name is part of every reference
- Receiver names: 1-2 letter abbreviation of type. Never `this`, `self`, `me`
- Initialisms stay consistent case: `URL` not `Url`, `ID` not `Id`

**Error handling:**
- Never discard errors with `_`
- Indent error flow (handle error first, return early). Normal path at minimal indentation
- Error strings: lowercase, no punctuation (they're composed into larger messages)
- Don't panic for normal error handling

**Goroutine lifetimes:**
- Make it clear when goroutines exit. Leaking goroutines is a common source of subtle bugs
- Prefer synchronous functions — callers can easily add concurrency, but can't remove it

**Key philosophy:**
- Line length: break lines by semantics, not arbitrary length. If lines are too long, the problem is long names or complex semantics
- "If someone says your code is not obvious, then it isn't"
- Don't pass pointers just to save bytes. If a function only uses `*x`, pass the value

---

## 4. Ousterhout — A Philosophy of Software Design

**Complexity definition:** Complexity is anything that makes it hard to understand or modify a system. Three manifestations:
1. **Change amplification** — simple change requires modifications in many places
2. **Cognitive load** — developer needs to know too many things to complete a task
3. **Unknown unknowns** — it's unclear what to do or what might break

**Two causes:** Dependencies (between components) and Obscurity (important info not obvious).

**Deep vs shallow modules:**
- A deep module has a simple interface but powerful functionality (hides significant complexity)
- A shallow module has a complex interface relative to its functionality (adds complexity without hiding it)
- Example of deep: Unix file I/O — 5 system calls hiding enormous complexity
- Breaking code into many tiny methods often creates many shallow modules, increasing total system complexity

**Key principles:**
- Tactical vs strategic programming: invest continuously in design, don't just make it work
- Complexity is incremental — each shortcut is small, but they accumulate into an unfixable mess
- **Pull complexity downwards** — better to have a simple interface with complex implementation than vice versa
- General-purpose modules are deeper — "somewhat general-purpose" with fewer methods is often simpler than highly specialized APIs
- **Define errors out of existence** — design operations so errors can't happen (Unix file deletion vs Windows)
- Pass-through methods and pass-through variables are red flags for confused responsibility
- Write code for the reader, not the writer
- "If someone says your code is not obvious, then it isn't"
- Comments describe things that aren't obvious from code. They're part of the abstraction
- Consistency minimizes complexity — learn once, apply everywhere
- Implementation inheritance increases complexity; prefer composition
- Design it twice — consider multiple options before committing

---

## 5. Fred Brooks — No Silver Bullet

**The essential/accidental distinction:**
- **Essential complexity** — inherent in the problem domain; cannot be removed by any tool or technique
- **Accidental complexity** — introduced by our tools, languages, processes; can be reduced by engineering effort
- Most past breakthroughs (high-level languages, time-sharing, unified environments) attacked accidental difficulties
- The essential difficulties explain why no single technique gives 10x improvement

**Four inherent properties of software:**
1. **Complexity** — no two parts are alike (or we'd make them a subroutine). States grow combinatorially. Non-linear interaction between components
2. **Conformity** — software must conform to arbitrary human institutions and interfaces. This complexity cannot be designed away
3. **Changeability** — all successful software gets changed. Software embodies function, and function is what people want to extend
4. **Invisibility** — software has no natural geometric representation. Multiple overlapping graphs (control flow, data flow, dependency, time sequence) that are not even planar

**Key insight:** "I believe the hard part of building software to be the specification, design, and testing of this conceptual construct, not the labor of representing it." The conceptual work IS the essential difficulty.

**On great designers:** The difference between good designs and great ones is not methodology — it's the designer. "Great designs come from great designers." The gap between best and average is an order of magnitude.

---

## 6. Rich Hickey — Simple Made Easy + Hammock Driven Development

**Simple vs Easy:**
- **Simple** (sim-plex = one braid): one role, one concept, one dimension. Having few things interleaved. Objective
- **Easy** (adjacent = lie near): familiar, at hand, near our capabilities. Subjective and relative
- "We are far too interested in things being easy. This is to our detriment"

**Complecting:**
- Complect = braid together. Compose = place together
- Complexity comes from things being interleaved/braided
- State complects value and time — you can never treat them independently again
- Just partitioning into modules doesn't give simplicity if modules are internally complected

**Key arguments:**
- "We can only hope to make reliable those things we can understand"
- There is a hard cognitive limit. Smart programmer vs average: like 9 balls vs 3 — no one juggles 900
- Easy things make us feel fast, but ignoring complexity slows you down on non-trivial projects
- "What type of runner can run full speed from the very start? Someone who runs very short distances"
- "Simplicity is a choice. It is your fault if you don't have a simple system"
- Tests are not a substitute for simplicity — "Who drives their car around banging into the guard rails?"

**Hammock Driven Development:**
- Most of the biggest problems in software are problems of misconception — not typos or type errors
- Features != solving problems. Features are shiny chrome knobs, not the purpose of the car
- Avoiding problems != solving problems. Getting around something is not solving it
- Look for tradeoffs in everything. "Usually when people talk about tradeoffs in their software, they're talking about the parts that suck"
- Feed your background mind: state the problem clearly, enumerate known constraints, then step away
- Don't be afraid of being wrong — you will always think of something better

---

## 7. Google SRE — Simplicity Chapter

**Core thesis:** Software simplicity is a prerequisite to reliability. Every line of code is a liability.

**Principles:**
- **Boring is good** — "Unlike a detective story, the lack of excitement, suspense, and puzzles is actually a desirable property of source code"
- **Negative lines of code** — some of the most satisfying coding is deleting thousands of lines. Smaller projects are easier to understand, test, and have fewer defects
- **I won't give up my code** — "What if we need it later?" Source control exists. Dead code, commented code, and flag-gated never-executed code are all time bombs
- **Minimal APIs** — "Perfection is attained not when there is no longer more to add, but when there is no longer anything to take away" (Saint-Exupery). Fewer methods = easier to understand = more effort on making each one great
- **Modularity** — loose coupling between binaries and configuration. A bug in one component can be fixed and pushed independently
- **Release simplicity** — small releases let you understand impact. 100 changes at once makes diagnosis nearly impossible
- **Essential vs accidental complexity** (citing Brooks) — SRE teams should push back on accidental complexity and constantly strive to eliminate it
- "Every time we say 'no' to a feature, we are not restricting innovation; we are keeping the environment uncluttered so that focus remains on real engineering"

---

## 8. Hyrum's Law

> "With a sufficient number of users of an API, it does not matter what you promise in the contract: all observable behaviors of your system will be depended on by somebody." — Hyrum Wright

**Implications:**
- There is no such thing as a private implementation detail if the interface has enough consumers
- Users depend on every aspect of behavior, intentionally or not — including bugs ("bug-for-bug compatibility")
- This constrains changes: you must conform to both the documented interface AND the implicit interface captured by usage
- Design implication: minimize observable behavior surface. The less behavior you expose, the more freedom you retain to change implementation

---

## 9. Martin Fowler — Is Design Dead?

**Evolutionary design:**
- Planned design assumes you can get it right upfront. Evolutionary design assumes you can't, and relies on continuous refactoring
- XP/Agile didn't kill design — it changed WHEN design happens (continuously, not just upfront)
- Evolutionary design REQUIRES: continuous refactoring, testing safety net, and the skill to recognize when design needs to change
- Without refactoring discipline, evolutionary design degenerates into "code and fix"

**The role of simplicity:**
- YAGNI (You Ain't Gonna Need It): don't add generality until you actually need it
- But: refactoring is NOT violating YAGNI. Keeping code clean is investment, not speculation
- "Doing a good design is something that I can't do at the beginning. I need to let the design emerge as my understanding grows"

**Patterns:**
- Patterns are not things to be applied upfront — they're targets you refactor TOWARD when the code calls for it
- "The key thing about XP's approach is that you don't try to build the flexible thing from scratch — you add flexibility as requirements change"

**Architecture:**
- Some things need upfront thought (things hard to refactor: database schemas, published APIs, deployment topology)
- But even architecture can grow if you have good separation and the courage to refactor
- "The last responsible moment" — defer irreversible decisions as long as possible

---

## 10. Joel Spolsky — The Law of Leaky Abstractions

> "All non-trivial abstractions, to some degree, are leaky."

**What it means:**
- Abstractions are supposed to hide complexity, but they never completely succeed
- When the abstraction leaks, you must understand the layer below to debug or work around it
- TCP abstracts unreliable IP into reliable delivery — but when the network cable is chewed, TCP can't help you
- SQL abstracts away procedural query steps — but some logically equivalent queries are 1000x slower

**Implications:**
- Higher-level tools save working time but not learning time — you still need to know the fundamentals
- "Learn how to do it manually first, then use the wizzy tool to save time"
- As abstraction layers increase, becoming a proficient programmer gets HARDER, not easier
- Code that works around modifications to old code is the most common source of leaky-abstraction bugs: "An incredibly common error pattern is perfectly good code that checks for NULL, but a later modification uses the pointer without checking"

---

## 11. Worse is Better (Richard P. Gabriel)

**The two approaches (four dimensions):**

| Dimension | New Jersey ("Worse is Better") | MIT ("The Right Thing") |
|-----------|-------------------------------|------------------------|
| Simplicity | Implementation simplicity > interface simplicity | Interface simplicity > implementation simplicity |
| Correctness | Slightly better to be simple than correct | Incorrectness not allowed |
| Consistency | Can be sacrificed for simplicity | As important as correctness |
| Completeness | Sacrificed whenever implementation simplicity is jeopardized | Simplicity must not overly reduce completeness |

**Why "worse" wins:**
- Simpler implementation → easier to port → spreads faster (virus-like)
- Users conditioned to accept "worse" → pressure to improve incrementally
- By the time "The Right Thing" is built, the "worse" system already has the ecosystem
- Unix and C are the canonical examples — simple implementations that spread everywhere

**The lesson for design:** A working, simple, portable system that's 50% right will beat a perfect system that's 90% right but too complex to ship. Ship, spread, iterate.

---

## 12. NASA JPL — Power of 10 Rules

Ten rules for safety-critical code (Gerard Holzmann, 2006):

1. **Simple control flow only** — no goto, no setjmp/longjmp, no recursion
2. **Fixed loop bounds** — a tool must be able to statically prove the loop terminates
3. **No dynamic allocation after init** — eliminates memory leaks, fragmentation, use-after-free
4. **Short functions** — max ~60 lines. Fits on one printed page
5. **High assertion density** — minimum 2 assertions per function. Check invariants that should never be violated
6. **Minimal scope** — declare data at the smallest possible scope level
7. **Check all returns** — every non-void return value checked by caller; every parameter validated by callee
8. **Limited preprocessor** — only #include and simple #define. No token pasting, no varargs macros, no recursive macros
9. **Restricted pointers** — max one level of dereference. No function pointers. No hiding dereference in macros/typedefs
10. **Zero warnings at max pedantic level** — all compiler warnings enabled, all static analyzers pass with zero warnings

**Philosophy:** Make code amenable to automated verification. If a tool can't prove a property, the code is too complex. Verifiability is a design constraint.

---

## 13. SQLite — Quality Management

**Testing philosophy:**
- 100% MC/DC (Modified Condition/Decision Coverage) in as-delivered configuration
- Testing of both source code AND object code
- Multi-platform, multi-compiler testing
- Fuzz testing (including a private custom fuzzer — dbsqlfuzz — kept secret precisely because it's effective)
- Code change inspection + dynamic and static analysis

**Design principles:**
- "Small. Fast. Reliable. Choose any three." — the tagline IS the design constraint
- Code is written assuming it will be read and maintained by people not yet born (target: year 2050)
- Minimal coding standards (2-space indent, 80-char lines, no tabs) — all other rules are subjective, aimed at long-term readability
- "All problems are fixed expeditiously. There are no lingering problems in the SQLite software"
- Continuous integration with no pre-defined release cycle — releases when there's critical mass of value
- Survivability: code mirrored across 3 machines in different cities with different hosting companies
- Inspired by DO-178B (avionics safety standard) but stripped to only the parts that genuinely improve quality

---

## 14. John Carmack — Static Analysis & Code Quality

**Core conviction:** "It is irresponsible to not use static code analysis."

**Key insights:**
- "The first step is fully admitting that the code you write is riddled with errors. That is a bitter pill to swallow, but without it, most suggestions for change will be viewed with irritation"
- Automation is necessary. "Exhortations to 'write better code', plans for more code reviews, pair programming... just don't cut it" under real time pressure
- In a large enough codebase, any class of error that is syntactically legal probably exists
- Code quality is statistical, like physical material properties — flaws exist everywhere, you can only minimize impact

**On NULL pointers and state:**
- "NULL pointers are the biggest problem in C/C++. The dual use of a single value as both a flag and an address causes an incredible number of fatal issues"
- Favor C++ references over pointers — a reference has the implicit contract of being non-NULL
- Printf format string errors were #2 — solved completely by annotation

**On code modifications:**
- "An incredibly common error pattern is perfectly good code that checks for NULL, but a later modification uses the pointer without checking" — failure to communicate preconditions
- Focus on code that ships to customers. Aggressively isolate internal-only code
- Shrink your important code — code size alone predicts error rate as well as any quality metric

**Cooperation with tools:**
- Favor indexing over pointer arithmetic
- Keep call graphs inside a single source file
- Use explicit annotations
- "Anything that isn't crystal clear to a static analysis tool probably isn't clear to your fellow programmers, either"

---

## 15. Wirth — Program Development by Stepwise Refinement

**Method:** Start with a high-level description of what the program should do. At each step, decompose one aspect into more concrete sub-operations. Defer concrete data representation decisions as long as possible.

**Key principles:**
- Each refinement step should introduce one decision and explain why it was made
- The sequence of decisions matters — some orderings lead to better programs
- Data structure choice and program structure co-evolve — you often can't decide one without the other
- "The art of programming" is knowing which decisions to make first and which to defer
- A program developed this way carries its own documentation — the series of refinements IS the design rationale

---

## 16. Provenance and Audit Modeling — W3C PROV, GitHub Audit Log, CloudEvents

**W3C PROV core distinction:**
- An **entity** is the thing whose provenance matters, such as a file.
- An **activity** is what happened to produce or use it, such as uploading, editing, or migrating.
- An **agent** is the person, organization, or software bearing responsibility for that activity.
- Generation and attribution connect these concepts; they are independent dimensions, not one combined enum.
- Source: [W3C PROV-DM](https://www.w3.org/TR/prov-dm/)

**Audit convention:**
- GitHub audit logs separate the **actor** that performed work from the **action** that occurred.
- CloudEvents defines **source** as the context in which an occurrence happened, not as the responsible actor; `type` describes the occurrence and `subject` identifies the affected thing within that source.
- Sources: [GitHub audit log](https://docs.github.com/en/organizations/keeping-your-organization-secure/managing-security-settings-for-your-organization/reviewing-the-audit-log-for-your-organization), [CloudEvents specification](https://github.com/cloudevents/spec/blob/main/cloudevents/spec.md)

**Design lens:**
- Keep `who`, `what happened`, `where it happened`, and `what was affected` separate when they can vary independently.
- Avoid composite values such as `user_upload` when future combinations such as user copy, API-triggered agent delivery, or system restore would multiply enum cases.
- Separation does not mean every table stores every dimension. Persist each fact where its lifetime and consumers belong: durable entity attribution on the entity, occurrence/action/context in an append-only audit or event record.

---

## 17. Mature Data-Model Case Studies — Files and Uploads

These examples are contrasts, not templates. First identify who owns the durable object, who owns transfer state, and what the application must recover after failure.

### Evidence levels

| Evidence | What it can support | What it cannot support |
|----------|---------------------|------------------------|
| Published SQL or migration | Concrete table roles, columns, constraints, and tradeoffs in the published design | A claim that the schema fits a different product unchanged |
| Public API or protocol | Resource boundaries, lifecycle states, identifiers, retry and cleanup obligations | Claims about the provider's private database tables |
| Product observation | User-visible behavior and naming | Hidden storage, transaction, or deletion implementation |

Do not describe an API resource model as a company's internal schema. Mature design review depends as much on evidence quality as on the example itself.

### Mux: operation, durable asset, event inbox, and product metadata

Mux publishes recommended SQL schemas that separate four concerns:

- `mux_uploads` tracks the direct-upload operation, its status, timeout, and eventual asset ID.
- `mux_assets` mirrors the durable video asset and its processing/playback metadata.
- `mux_webhook_events` records incoming events for deduplication, debugging, and reprocessing.
- An optional application `videos` table owns product metadata such as title, user, visibility, and tags.

The important lesson is not "use four tables." Each row has a distinct lifecycle and source of truth. The upload can fail without an asset; the asset can outlive the upload; the webhook event is immutable evidence; product metadata belongs to the application rather than Mux.

Source: [Mux database schema design](https://www.mux.com/docs/integrations/database-schema)

### Rails Active Storage: fewer tables, accepted orphan cleanup

Rails publishes a deliberately compact model:

- `active_storage_blobs` stores uploaded-file metadata.
- `active_storage_attachments` connects application records to blobs.
- `active_storage_variant_records` exists only when generated variants are tracked.

Rails does not require a separate application upload table. With direct uploads, it creates storage/blob state before the business record is attached. The official guide explicitly warns that a file may upload but never attach, and recommends purging unattached blobs.

This is a valid alternative to a first-class upload table when the product does not need durable upload history, resumable coordination, support diagnostics, or retry recovery in its own database. The schema is smaller because it accepts orphan cleanup as the tradeoff; it did not make the incomplete-upload problem disappear.

Source: [Rails Active Storage overview](https://guides.rubyonrails.org/active_storage_overview.html)

### S3 multipart upload: transport state can belong to the provider

Amazon S3 models multipart upload as a separate operation: initiate, upload parts, then complete or abort. S3 issues an upload ID; completion creates the object. Incomplete parts remain billable until completion or abort.

Applications may need to retain the upload ID and completion inputs, but should not automatically reproduce every provider-owned part record in their business database. Persist only the state needed for application recovery, authorization, user-visible progress, or reconciliation.

Source: [Amazon S3 multipart upload overview](https://docs.aws.amazon.com/AmazonS3/latest/userguide/mpuoverview.html)

### OneDrive upload session: temporary resource before final file

Microsoft Graph exposes an upload session with an expiration time and missing byte ranges. The final drive item is a different resource produced when upload completes.

This supports the lifecycle distinction between "an attempt to create a file" and "the file that now exists." It does not reveal OneDrive's internal tables. Use it to reason about boundaries, not to copy columns blindly.

Source: [Microsoft Graph createUploadSession](https://learn.microsoft.com/en-us/graph/api/driveitem-createuploadsession?view=graph-rest-1.0)

### GitLab: table ownership, database boundaries, and enforceable integrity

GitLab's public database dictionary gives every table a description, feature category, schema/database placement, introduction history, size class, and sharding key where applicable. This makes table ownership and operational scale part of the reviewed design rather than tribal knowledge.

Its database guidelines also prefer real foreign keys for associations and database constraints for integrity. For large live tables, GitLab separates adding a constraint from validating existing rows so the desired invariant can be introduced without treating a long lock as an acceptable migration plan.

The lesson is broader than GitLab's exact fields:

- A table should be explainable in one sentence and have an identifiable product owner.
- Logical relationships should normally be enforceable by the database; application-only consistency needs a stated reason.
- Schema correctness and rollout safety are separate design problems. A clean final schema can still require staged migration.
- Scale metadata is not automatically a column on every business row; it belongs in schema documentation and operational tooling unless runtime behavior reads it.

Sources: [GitLab database dictionary](https://docs.gitlab.com/development/database/database_dictionary/), [GitLab foreign keys and associations](https://docs.gitlab.com/development/database/foreign_keys/), [GitLab CHECK constraints](https://docs.gitlab.com/development/database/check_constraints/)

### Synthesis: what compact schemas have in common

1. **They name ownership clearly.** Provider mirrors, application metadata, transfer operations, and audit evidence do not silently share one row.
2. **They keep only useful state.** A status deserves storage when users, workers, support, retries, or cleanup read it; not merely because the process has stages.
3. **They make residue explicit.** Incomplete uploads, unattached blobs, duplicate callbacks, and abandoned sessions have an owner and cleanup rule.
4. **They distinguish current truth from history.** A current state column answers "what is true now"; an event row answers "what happened." One does not automatically justify the other.
5. **They earn derived columns.** Cached counts, normalized names, and duplicated metadata need a query, invariant, or recovery reason plus a refresh strategy.
6. **They use constraints to compress logic.** Foreign keys, conditional uniqueness, checks, and transactional state changes are often more elegant than extra repair fields and background jobs.
7. **They are small for a reason.** Rails is compact because it accepts orphan cleanup. Mux is explicit because it mirrors an asynchronous provider. S3 and OneDrive can keep transfer detail outside the application database because the provider owns the session.

The review question is therefore not "Can this be done in two tables?" It is: "What facts must this system own, and what is the smallest schema that preserves their independent lifecycles and failure recovery?"

---

## The 8 审查 Questions (Synthesis)

When examining any piece of code, ask:

1. **Special cases** (Linus): Does this eliminate special cases, or add them?
2. **Hidden decisions** (Parnas/Ousterhout): Does the boundary hide a real design decision, or is it a shallow wrapper?
3. **State inventory** (Hickey/Carmack): What state was introduced? Who owns/mutates/reads/cleans it?
4. **Observable behavior** (Hyrum): What behavior is exposed that callers might accidentally depend on?
5. **Simple vs Easy** (Hickey): Is this simple (few concepts, orthogonal) or merely familiar/convenient?
6. **Unhappy paths** (Pike/NASA): Are error/cancel/timeout/cleanup paths as clear as the happy path?
7. **Change locality** (Brooks/Ousterhout): Does this make future changes more local or more global?
8. **Deletability** (SRE/Carmack): Can anything be removed — a parameter, branch, abstraction, layer — without losing correctness?
