What I'm Building

A living dashboard of the projects, experiments, and ideas I'm building.

This repository is intended to become the public window into my work — a simple way to see what I'm currently working on without exposing the private source code, infrastructure, business details, or sensitive information behind those projects.

«Status: 🚧 Private / Work in Progress
Goal: Make this repository public once the system is functional and the publication boundary has been properly tested.»

---

What is this?

I work on multiple projects at the same time. Some are public, some are private, some are experiments, and some may eventually become commercial products.

"what-im-building" is intended to keep track of all of them in one place.

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

"command-center-project"

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

"what-im-building"

This repository is the public projection.

It should only contain information that has explicitly been approved for publication.

Conceptually:

                 PRIVATE
        ┌─────────────────────────┐
        │   command-center-project│
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
        │     what-im-building    │
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

The planned architecture is for the GitHub Action to live in "command-center-project", rather than here.

A scheduled workflow there would run periodically — initially something like once per day — and:

1. Read the internal project registry.
2. Determine which projects are approved for public listing.
3. Collect only the approved public metadata.
4. Optionally inspect selected project repositories for approved status information.
5. Generate/update the corresponding content in this repository.
6. Commit and push the changes to "what-im-building".

The private repository therefore remains the automation and source-of-truth layer, while this repository becomes the generated/public-facing layer.

Planned authentication

The automation should use a scoped GitHub credential, initially potentially a fine-grained PAT, with the smallest permissions necessary.

For example:

Private project repositories
        │
        │ Contents: Read
        ▼
 command-center-project
        │
        │ Contents: Write
        ▼
 what-im-building

The credential should not have broad account-wide access.

A GitHub App may eventually be preferable if the system grows, but a tightly scoped fine-grained PAT is sufficient for the initial implementation.

No credentials should ever be committed to this repository.

---

The publication boundary

One of the most important parts of this project is deciding what is safe to publish.

The automation should never simply scrape private repositories and let an AI decide what information is safe to expose.

Instead, the private project registry should explicitly define the public information for each project.

For example:

{
  "slug": "ai-workspace",
  "name": "AI Workspace",
  "visibility": "private",
  "public": {
    "listed": true,
    "description": "An AI-powered workspace for Android.",
    "status": "building",
    "show_activity": true,
    "show_roadmap": false,
    "show_changelog": false
  }
}

A project that should remain completely invisible could simply have:

{
  "public": {
    "listed": false
  }
}

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
              command-center-project
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
                 what-im-building
                         │
                         ▼
                  Git commit + push

The goal is for the process to be boring, predictable, and auditable.

If something is not explicitly approved for publication, it should not cross the boundary.

---

Project structure

The structure will evolve as the system becomes functional.

A possible starting point:

what-im-building/
├── README.md
├── now.md
├── projects/
│   ├── project-one.md
│   ├── project-two.md
│   └── ...
├── changelog.md
└── stats.md

The repository may eventually become more data-driven, for example:

what-im-building/
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

The private Mission Control system should maintain the underlying state, while this repository presents the selected public view.

---

Future: Mission Control integration

The following are potential changes to make later in "command-center-project".

These are suggestions, not yet implemented decisions.

1. Add explicit public metadata to the project registry

Each project could define a "public" section containing the fields that are safe to publish.

For example:

"public": {
  "listed": true,
  "description": "...",
  "status": "building",
  "show_activity": true,
  "show_roadmap": false,
  "show_changelog": false
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
what-im-building

3. Add a scheduled GitHub Action

Run the exporter automatically, initially once per day.

The workflow would:

- read the project registry
- collect approved metadata
- optionally inspect approved repositories
- generate public content
- push changes to "what-im-building"

4. Keep credentials tightly scoped

The automation should use credentials with only the permissions it actually needs.

Initial target:

Project repositories → read
what-im-building     → write
Everything else      → no access

A GitHub App could eventually replace the PAT if more granular or scalable access becomes useful.

5. Separate public data from private activity

Mission Control may eventually contain considerably more information than should ever appear here.

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

This repository is currently being prepared as the public-facing layer of the system.

For now it remains private while the architecture, automation, and publication rules are being developed.

Planned: status legend + freshness

- Status meanings are owned by `projects-command-center` (`docs/STATUS-LEGEND.md`). Daily CI there should **copy** that legend into this repo so the two never drift.
- Each successful publish should write `last_generated_at` (and the expected interval, e.g. 24h). If the stamp is older than the interval, viewers (and later a small CI check here) can treat the dashboard as **stale / pipeline broken**. Implement and test when the publish pipeline is live.

Next steps

- [ ] Define the public project schema
- [ ] Decide which projects should initially be listed
- [ ] Create the initial project pages/data
- [ ] Define the generated-vs-manual content boundary
- [ ] Design the Mission Control → public projection workflow
- [ ] Add the scheduled GitHub Action to projects-command-center
- [ ] Copy STATUS-LEGEND.md from command-center on each CI run
- [ ] Stamp `last_generated_at`; optional stale-label CI here
- [ ] Configure narrowly scoped GitHub authentication
- [ ] Test the publication boundary with private/NDA-sensitive projects
- [ ] Verify that no secrets or private metadata can leak
- [ ] Make this repository public once the system is functional and the boundary has been validated

---

Principle

«Build privately. Project publicly.»

The projects themselves can remain private.

The process of building them doesn't have to be.