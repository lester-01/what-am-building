What I'm Building

A living dashboard of the projects, experiments, and ideas I'm building.

This repository is intended to become the public window into my work — a simple way to see what I'm currently working on without exposing the private source code, infrastructure, business details, or sensitive information behind those projects.

The current allowlisted list is in [now.md](now.md). Status meanings are copied from the command center into [STATUS.md](STATUS.md). Freshness is in `last_generated_at`.

This repository stays private until the publication boundary has been tested.

---

What is this?

I work on multiple projects at the same time. Some are public, some are private, some are experiments, and some may eventually become commercial products.

"what-am-building" is intended to keep track of all of them in one place.

The goal isn't to expose everything.

The goal is to expose just enough to answer:

- What am I building?
- What is currently active?
- What stage is each project in?
- What have I recently been working on?
- Which projects are public?
- Which projects are private but still worth following?
- What am I experimenting with right now?

Think of this repository as a public read-only projection of my projects, rather than the place where the projects themselves are developed.

---

The bigger system

The system is split into two concepts.

"projects-command-center"

This is the private source of truth and development control center.

It contains the internal project registry, project state, development information, agent instructions, and other operational data that should not be public.

It knows about projects whether they are:

- public
- private
- experimental
- commercial
- unfinished
- under NDA
- not ready to announce

"what-am-building"

This repository is the public projection.

It should only contain information that has explicitly been approved for publication.

Conceptually:

                 PRIVATE
        ┌─────────────────────────┐
        │ projects-command-center │
        │                         │
        │  Project Registry       │
        │  Project State          │
        │  Internal Metadata      │
        │  Development Activity   │
        │  Private Repositories   │
        └────────────┬────────────┘
                     │
                     │ controlled publication
                     ▼
                 PUBLIC
        ┌─────────────────────────┐
        │     what-am-building    │
        │                         │
        │  Public Project List    │
        │  Approved Descriptions  │
        │  Public Status          │
        │  Selected Activity      │
        │  Builder Updates        │
        └─────────────────────────┘

The important rule is:

«The private system can know everything. The public system should know only what I explicitly allow it to know.»

---

Automatic updates

The eventual goal is for this repository to update itself.

The planned architecture is for the GitHub Action to live in "projects-command-center", rather than here.

A scheduled workflow there would run periodically — initially something like once per day — and:

1. Read the internal project registry.
2. Determine which projects are approved for public listing.
3. Collect only the approved public metadata.
4. Optionally inspect selected project repositories for approved status information.
5. Generate/update the corresponding content in this repository.
6. Commit and push the changes to "what-am-building".

The private repository therefore remains the automation and source-of-truth layer, while this repository becomes the generated/public-facing layer.

Planned authentication

The automation should use a scoped GitHub credential, initially potentially a fine-grained PAT, with the smallest permissions necessary.

For example:

Private project repositories
        │
        │ Contents: Read
        ▼
 projects-command-center
        │
        │ Contents: Write
        ▼
 what-am-building

The credential should not have broad account-wide access.

A GitHub App may eventually be preferable if the system grows, but a tightly scoped fine-grained PAT is sufficient for the initial implementation.

No credentials should ever be committed to this repository.

---

The publication boundary

One of the most important parts of this project is deciding what is safe to publish.

The automation should never simply scrape private repositories and let an AI decide what information is safe to expose.

Instead, the private project registry should explicitly define the public information for each project.

Each project repo holds a `project.json`. The GitHub **description** is the short blurb (not stored in JSON). The allowlist flag lives in JSON:

{
  "schema_version": 1,
  "status": "building",
  "tags": ["android"],
  "public": {
    "listed": true
  }
}

A project that should remain completely invisible uses `"listed": false` (the default).

This gives the system an explicit publication contract.

Deterministic code decides what information is allowed through the boundary.

An LLM may eventually help turn that approved information into readable summaries, descriptions, or updates — but it should not be responsible for deciding what is secret.

---

Privacy & sensitive projects

Some projects may contain information that must never leave the private environment.

This could include:

- proprietary source code
- credentials and API keys
- customer information
- revenue or financial information
- private infrastructure
- internal architecture
- unreleased product details
- commercial strategy
- private roadmaps
- security-sensitive information
- NDA-protected information
- private repository URLs
- information belonging to third parties

Those projects can still exist in the private project registry without appearing here.

The public repository should therefore be treated as an allowlist, not a mirror.

---

How the automation will work

The exact implementation is still being designed.

A future version of the system could look approximately like:

                    GitHub Actions
                         │
                         ▼
              projects-command-center
                         │
                 Read project registry
                         │
                         ▼
              Public metadata filter
                         │
                  approved data only
                         │
                         ▼
                Optional LLM formatting
                         │
                         ▼
                  Generated content
                         │
                         ▼
                 what-am-building
                         │
                         ▼
                  Git commit + push

The goal is for the process to be boring, predictable, and auditable.

If something is not explicitly approved for publication, it should not cross the boundary.

---

Project structure

The structure will evolve as the system becomes functional.

A possible starting point:

what-am-building/
├── README.md
├── now.md
├── projects/
│   ├── project-one.md
│   ├── project-two.md
│   └── ...
├── changelog.md
└── stats.md

The repository may eventually become more data-driven, for example:

what-am-building/
├── README.md
├── projects/
├── data/
├── now.md
├── changelog.md
└── stats.md

The exact structure is intentionally not fixed yet.

---

Project statuses

A simple status vocabulary may eventually be used:

Status| Meaning
💡 Idea| Early concept or exploration
🔬 Exploring| Researching or validating
🛠️ Building| Active development
🧪 Testing| Being tested or validated
🚀 Launching| Preparing for release
🟢 Active| Running / actively maintained
⏸️ Paused| Temporarily inactive
📦 Shipped| Completed or released
💤 Archived| No longer actively pursued

This is subject to change as the system evolves.

---

Why this exists

I don't want a traditional portfolio that only shows finished projects.

A lot of interesting work happens before something is finished:

- ideas being tested
- prototypes being built
- technologies being explored
- products being validated
- experiments that may go nowhere
- projects that eventually become something much larger

This repository is meant to capture that process.

It is a living record of what I'm building, rather than a collection of polished case studies.

---

Planned automation

Eventually, the system should be able to answer questions such as:

«What am I building right now?»

«Which projects changed this week?»

«What is currently active?»

«Which projects are public?»

«What have I recently shipped?»

«What am I experimenting with?»

The intention is that I should not have to manually maintain all of this information.

The private command center should maintain the underlying state, while this repository presents the selected public view.

---

Future: command center integration

The following are potential changes to make later in "projects-command-center".

These are suggestions, not yet implemented decisions.

1. Keep the public allowlist on each project’s `project.json`

The schema already has this. Status and tags live on the project; the GitHub description is the blurb. The only publication switch is:

"public": {
  "listed": true
}

2. Add a public projection/export script

Create a deterministic script that reads the private registry and produces a sanitized dataset containing only approved fields.

For example:

private registry
      │
      ▼
public projection script
      │
      ▼
sanitized project data
      │
      ▼
what-am-building

3. Add a scheduled GitHub Action

Run the exporter automatically, initially once per day.

The workflow would:

- read the project registry
- collect approved metadata
- optionally inspect approved repositories
- generate public content
- push changes to "what-am-building"

4. Keep credentials tightly scoped

The automation should use credentials with only the permissions it actually needs.

Initial target:

Project repositories → read
what-am-building     → write
Everything else      → no access

A GitHub App could eventually replace the PAT if more granular or scalable access becomes useful.

5. Separate public data from private activity

The command center may eventually contain considerably more information than should ever appear here.

It may therefore be useful to distinguish between:

Internal activity
Public activity
Public changelog
Public roadmap

rather than treating all project activity as publishable.

6. Let the LLM format, not authorize

If an LLM is used in the pipeline, it should receive the already-sanitized public dataset.

Its job should be things like:

- summarizing progress
- writing readable project descriptions
- generating daily/weekly updates
- organizing information

It should not receive unrestricted private repository contents and decide what is safe to publish.

---

Current status

The publisher lives in `projects-command-center` (`scripts/publish_dashboard.py` and the **Publish dashboard** workflow). Generated files here are `now.md`, `projects/`, `STATUS.md`, `last_generated_at`, and `last_generated_interval`. Do not edit those by hand.

This repository stays private until a listed publish has been inspected for leaks.

Next steps

- [x] Define the public project schema (`project.json` + GitHub description)
- [ ] Decide which projects should initially be listed
- [x] Create the generated project pages (`now.md`, `projects/`)
- [x] Define the generated-vs-manual content boundary (this README is manual; `now.md` and `projects/` are generated)
- [x] Design the command-center → public projection workflow
- [x] Add the scheduled GitHub Action to projects-command-center
- [x] Copy STATUS.md from command-center on each CI run
- [x] Stamp `last_generated_at`
- [ ] Configure narrowly scoped GitHub authentication (`DASHBOARD_PUBLISH_TOKEN` secret)
- [ ] Test the publication boundary with private/NDA-sensitive projects
- [ ] Verify that no secrets or private metadata can leak
- [ ] Make this repository public once the system is functional and the boundary has been validated

---

Principle

«Build privately. Project publicly.»

The projects themselves can remain private.

The process of building them doesn't have to be.