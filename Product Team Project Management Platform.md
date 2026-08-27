## Product Requirements Document — v2

_Revised to reflect the three-tab architecture established in wireframing._

---

### 1. Overview

The Product Team Project Management Platform is an internal tool purpose-built to help the Product Team plan, organize, document, and communicate its work across multiple products.

The platform is **not intended to replace Azure DevOps** or function as a developer task-management system. DevOps remains the system of record for technical implementation, development work, bugs, sprints, and engineering execution.

Instead, this platform manages the layer **above implementation**:

- What should we build?
- Why are we building it?
- What are we currently working on?
- What have we implemented?
- What do we know about our products and processes?
- What are we planning to work on next?
- How does the rest of the company understand what the Product Team is doing?

The platform combines aspects of:

- Product backlog
- Roadmap
- Changelog
- Knowledge base
- Project documentation
- Internal product wiki
- Team communication/status

Its organizing principle is inspired by **Obsidian's nested tag system**, allowing information to be classified through flexible, hierarchical tags rather than requiring every piece of work to fit into a rigid project hierarchy.

---

## 2. Product Architecture: Three Tabs

The platform is organized into three top-level tabs. This is the single most important structural decision in the product, because it separates **who each surface is for**.

```text
┌──────────┐  ┌───────────────┐  ┌─────────┐
│ PROJECTS │  │ KNOWLEDGEBASE │  │ ROADMAP │
└──────────┘  └───────────────┘  └─────────┘
   authoring        published        published
   (Product Team)   (company)        (company)
```

### 2.1 Projects — the working surface

Projects is where the Product Team actually works. It contains the full backlog, every item type, all tags, all relationships, and the design-work tracking for each item.

Projects serves two purposes simultaneously:

1. **A CMS for the other two tabs.** Knowledgebase articles and Roadmap entries are not authored in their own systems. They are authored on items in Projects and _published_ outward.
2. **The Product Team's own work tracker.** Item detail pages carry a checklist so design work — wireframes, specs, reviews, handoff — can be tracked at the item level without creating DevOps tasks.

Projects is the only tab where content is created or edited.

### 2.2 Knowledgebase — published product knowledge

A read-oriented, browsable documentation surface covering how the products and processes actually work. Every article originates as a Knowledge item in Projects.

### 2.3 Roadmap — published team status

A single page answering what the Product Team is working on, what it recently shipped, and what is coming next. Every card on it is a published view of an item in Projects.

### 2.4 The publishing model

Nothing is written twice. An item in Projects can be _promoted_ to one or both published tabs:

```text
Item (Projects)
  ├── + Knowledge Article ──→ Knowledgebase
  └── + Changelog Article ──→ Roadmap / Changelog
```

This is the mechanism behind the "one source of truth" principle. If an article is wrong, it is corrected on the item, not on the published page.

---

## 3. Problem Statement

The Product Team works across multiple products and operates at dramatically different scales.

A single team may simultaneously be responsible for:

- A large-scale WMS redesign
- A new product or application
- A multi-month workflow overhaul
- A small process improvement
- A UI refinement
- An internal tool
- A new feature requested by another department
- Documentation and product knowledge
- Ongoing maintenance and improvements

These efforts do not naturally fit into a single project-management hierarchy.

For example:

> Product → Project → Epic → Task

works reasonably well for a large engineering initiative, but becomes cumbersome for something as small as:

> Improve the wording of the shipping confirmation screen.

Conversely, a massive WMS redesign requires substantially more structure than a simple feature request.

The team therefore needs a system that can accommodate **different scales of work without forcing every item into the same structure**.

Additionally, the team's knowledge currently becomes fragmented across applications, documents, conversations, DevOps work items, and individual people's knowledge.

There is also no reliable channel for telling the rest of the company what Product is doing. Status is communicated ad hoc, and knowledge that other departments need is locked inside the team's own working files.

The new platform creates a centralized, connected representation of everything we do and publishes the relevant slices of it outward.

---

## 4. Goals

### Primary Goals

1. **Maintain a single product backlog**
    
    - Capture potential features, improvements, ideas, and initiatives.
    - Prioritize and organize future work.
    - Track the progression from idea to implemented functionality.
2. **Provide flexible organization**
    
    - Organize information using nested tags.
    - Allow the same item to belong to multiple contexts.
    - Avoid forcing small pieces of work into heavyweight project structures.
3. **Create a product knowledge base**
    
    - Document how products work.
    - Record business rules, workflows, terminology, and institutional knowledge.
    - Make knowledge discoverable and interconnected.
4. **Maintain a product changelog**
    
    - Record meaningful product changes.
    - Connect changes back to the work that produced them.
    - Make changes understandable to non-technical employees.
5. **Communicate Product Team activity**
    
    - Give the company visibility into current, recent, and planned work through the Roadmap tab.
6. **Track design work at the product level**
    
    - Give the Product Team a place to track its own pre-implementation work — research, wireframes, specs, reviews — without creating DevOps items.
7. **Publish rather than duplicate**
    
    - Every company-facing surface is generated from working items, never maintained separately.
8. **Complement DevOps rather than duplicate it**
    
    - Product work should be represented at the appropriate level of abstraction.
    - Technical implementation can be linked to DevOps rather than recreated inside this system.

---

## 5. Non-Goals

The platform should **not** attempt to become a replacement for Azure DevOps. The following remain primarily within DevOps:

- Developer tasks
- Sprint planning
- Technical subtasks
- Code-related work
- Pull requests
- Build/deployment workflows
- Developer estimates
- Technical bug tracking
- Engineering velocity metrics

The platform may link to or summarize this information, but should not duplicate it unnecessarily.

The platform is also not intended to be a generic enterprise project-management system like Jira, Monday, or Asana.

The Knowledgebase is not a general-purpose company wiki. It covers products and product processes only.

The Roadmap is not a commitment or delivery-date system. It communicates intent and status, not contractual dates.

---

## 6. Core Concept: Nested Tags

The foundational organizational model is a hierarchical tagging system inspired by Obsidian.

Instead of requiring every item to belong to exactly one project, users assign one or more hierarchical tags.

```text
#product/platinum
#product/platinum/jobs
#product/platinum/jobs/shipping
#product/platinum/jobs/shipping/distribution
```

Another item might be tagged:

```text
#product/platinum
#workflow/shipping
#department/operations
#type/feature
```

Tags provide **context**, not workflow state. This distinction matters:

```text
#product/platinum      →  what something is, where it belongs
#area/shipping
#type/feature

Backlog / In Progress  →  its current lifecycle state
```

These remain separate concepts. Status lives in the lifecycle field and drives the Board view; tags drive navigation, filtering, and the Knowledgebase tree.

### 6.1 Tag Namespaces

The tag tree in the Projects sidebar is namespaced at the root. here is where to start with the prototype, but the structure should be completely free-flowing

|Namespace|Purpose|Example|
|---|---|---|
|`#product`|Which product and which area of it|`#product/platinum/jobs/shipping`|
|`#initiative`|Cross-cutting bodies of work|`#initiative/wms-rework`|
|`#department`|Requesting or affected department|`#department/operations`|
|`#workflow`|Business process the item touches|`#workflow/shipping`|
|`#area`|Functional area, product-agnostic|`#area/shipping`|
|`#type`|Item type, applied automatically|`#type/feature`|


### 6.2 Tag Counts

Every node in the tag tree displays a live item count (`platinum · 41`, `jobs · 22`, `shipping · 9`). Counts are inclusive of descendants. This makes the sidebar double as a rough map of where work is concentrated.

---

## 7. Information Model

### 7.1 Items

The fundamental unit of the system is an **Item**: a piece of information or work.

Items share a common underlying structure while allowing different types to expose type-specific fields.

#### Common Item Properties

- Title
- Description
- Type
- Status (lifecycle)
- Tags
- Created date
- Updated date
- Target date
- Related items
	- knowledgebase articles
	- changelog article
- Design checklist
- Attachments
- Publication state (Knowledgebase / Changelog)
- Activity/history

---

## 10. Lifecycle

Work has a simple lifecycle independent of its type. This drives the Board view columns directly.

```text
Idea → Backlog → Planned → In Progress → Complete → Released
```

Not every item passes through every stage.

- `Idea → Backlog` may remain there indefinitely.
- A small improvement: `Backlog → In Progress → Released`
- A large project: `Idea → Planned → In Progress → Complete → Released`

### 10.1 Complete vs. Released

These are deliberately distinct.

- **Complete** — the product work is finished and accepted, but the change is not yet in customers' hands. Cards in this column commonly show an `awaiting release` marker.
- **Released** — the change is live. Entering Released is the trigger that makes an item eligible for the Roadmap's "Recently Released" column and for a changelog entry.

The system should avoid imposing unnecessary process beyond this.

---

## 11. Projects Tab

### 11.1 Structure

Projects has a persistent tag-tree sidebar and two view toggles: **List** and **Board**.

```text
┌─────────────────────────────────────────────────────┐
│  [PROJECTS]  KNOWLEDGEBASE  ROADMAP                 │
│  [LIST] BOARD                                       │
├──────────────┬──────────────────────────────────────┤
│ filter tags… │  #product/platinum                   │
│              │  FILTER BY                           │
│ #product     │  #backlog #planned #in-progress      │
│  platinum 41 │  #jobs #proofs                       │
│   jobs 22    │  ┌────────────────────────────────┐  │
│    shipping 9│  │ Search                         │  │
│    billing 4 │  └────────────────────────────────┘  │
│    approval 5│                                      │
│   proofs 8   │  [ item ]  [ item ]  [ item ]        │
│  menumaker 17│  [ item ]  [ item ]  [ item ]        │
│  express shp6│                                      │
│ #initiative  │                                      │
│ #department 3│                                      │
│ #workflow 5  │                                      │
│ #type        │                                      │
└──────────────┴──────────────────────────────────────┘
```

### 11.2 Sidebar

wireframe shows this appearing on the left but it should actually appear on the right

- A `filter tags…` input at the top filters the tree itself, not the results. With dozens of nested tags this is the difference between the tree being navigable and being a wall.
- Namespaces are collapsible. Counts are shown per node.
- Selecting a node scopes the main panel to that tag and its descendants, and sets the panel heading (`#product/platinum`).

### 11.3 List View

The main panel shows the scoped item set with:

- **Filter by** — a row of one-click tag chips reflecting common refinements within the current scope. these should be derived by all tags that exist on currently shown items
- **Search** — full-text within the current scope. Clearing scope searches everything.
- Item rows/cards showing title, description, product

### 11.4 Board View

The same scoped item set, arranged in lifecycle columns with per-column counts:

```text
IDEA 7 │ BACKLOG 18 │ PLANNED 5 │ IN PROGRESS 3 │ COMPLETE 4 │ RELEASED 12
```

Cards display title, tags. Drag between columns changes status. The active column is visually emphasized so the team's current focus is obvious on open.

Board and List are representations of the same query — switching views preserves scope, filters, and search.

### 11.5 Quick Capture

Capture is a modal available from anywhere in the tool via **⌘K / Ctrl+K**, and from an inline `+` affordance.

```text
┌──────────────────────────────────────────────┐
│ + Add ability to duplicate a job      ⏎ save │
├──────────────────────────────────────────────┤
│ TAGS  [#product/platinum/jobs ×]  #area/ship│ │
│                                              │
│  #area/shipping · 9 items                    │
│  #workflow/shipping · 6                      │
│  #product/platinum/jobs/shipping · 9         │
│  + create #area/ship…                        │
│                                              │
│                          [ Save to Backlog ] │
└──────────────────────────────────────────────┘
```

Requirements:

- Title is the only required field. Everything else is optional and deferrable.
- Tag autocomplete shows **item counts per suggestion**. This is the primary defense against tag sprawl — a user typing `#area/ship` sees that `#area/shipping` already has 9 items and picks it instead of coining a near-duplicate.
- Creating a new tag is possible but is the _last_ option in the list, never the first.
- If the modal is opened while a tag node is selected in the sidebar, that tag is pre-applied as a chip.
- Enter saves. The default destination is Backlog; the button states this explicitly rather than saying only "Save."
- After save, a brief confirmation with an undo/open affordance — the user should not lose their place.

### 11.6 Item Detail

```text
Backlog / #product/platinum/jobs/shipping / Flexible shipping dates

Flexible shipping dates per job              Target   8/4
[#product/platinum] [#product/platinum/jobs] Updated  2 days ago
[#type/feature] [+ tag]
                                             ┌────────────────────────┐
DESCRIPTION                                  │ Add Changelog Article  │
────────────────────────────                 └────────────────────────┘
────────────────────────────                 ┌────────────────────────┐
                                             │ Add Knowledge Article  │
┌─────────────────────────┐                  └────────────────────────┘
│                         │
│        Checklist        │
│                         │
└─────────────────────────┘
```

Components:

- **Breadcrumb** showing status, primary tag path, and item title — this doubles as the way back out to a scoped list.
- **Title, tags, and inline tag add.**
- **Meta rail** — target date, updated date, owner, priority, DevOps link.
- **Description** — rich text.
- **Design checklist** — a free-form, user-defined checklist for tracking the Product Team's own work on this item (e.g. _problem framed, wireframes, spec written, reviewed with dev, handed off_). Checklist items are not DevOps tasks and never sync to DevOps. Progress is expressed as a simple ratio on Board cards. 
- **Publish actions** — `Add Changelog Article` and `Add Knowledge Article`. These are the CMS mechanism described in §2.4, and are covered in detail in §14.
- **Related items** panel.
- **Activity/history.**

Progressive structure applies: a two-line Improvement shows title, tags, and description, with checklist, relationships, and publish actions collapsed until invoked.

---

## 12. Knowledgebase Tab

A read-oriented, company-facing documentation surface. Content originates as Knowledge articles in Projects.

### 12.1 Layout

```text
┌────────────────┬──────────────────────────┬───────────┐
│ KNOWLEDGE      │ Knowledge / Platinum /   │ Related   │
│ ⌕ search       │           Jobs           │           │
│                │                          │  link     │
│ ▾ Platinum     │ KNOWLEDGE  verified 3wks │  link     │
│   ▾ Jobs       │                          │  link     │
│     How ship   │ How shipping dates are   │  link     │
│       dates    │ calculated in Platinum   │  link     │
│       work     │                          │           │
│     How jobs   │ [#product/platinum]      │           │
│       are      │ [#area/shipping]         │           │
│       struct.  │ [#type/knowledge]        │           │
│     Job status │                          │           │
│       defs     │ BUSINESS RULES           │           │
│   ▸ Billing    │ ──────────────           │           │
│   ▸ Proofs     │ ──────────────           │           │
│ ▸ MenuMaker    │                          │           │
│ ▸ Processes    │ KNOWN LIMITATIONS        │           │
│ ▸ Terminology  │ ──────────────           │           │
└────────────────┴──────────────────────────┴───────────┘
```

### 12.2 Navigation

- A persistent article tree mirroring the `#product` tag hierarchy, plus product-agnostic roots for **Processes** and **Terminology**.
- Article search scoped to the Knowledgebase.
- The tree is generated from tags, not maintained by hand. Retagging an article moves it.
- Breadcrumb above the article title.

### 12.3 Article

- **Tags**, displayed and clickable, filtering the Knowledgebase to that tag.
- **Related panel** — links derived from the item's relationships in Projects plus shared tags. Relations to Features, Decisions, and Changes are included, so a reader can trace _how shipping dates work_ to _why they work that way_ and _what recently changed about them_.

### 12.4 Editing

Knowledgebase articles are read-only in this tab. An `Edit` affordance visible to Product Team members deep-links to the source item in Projects.

---

## 13. Roadmap Tab

A single company-facing page answering three questions.

### 13.1 Layout

```text
PRODUCT TEAM
What we're building
Updated weekly · questions go to the team channel

CURRENTLY WORKING ON      RECENTLY RELEASED         COMING UP
┌──────────────────────┐  ┌──────────────────────┐  ┌────────────────────┐
│ Platinum Job Mgmt    │  │ Saved Ship-To    Aug │  │ Bulk Job Editing   │
│ ▓▓▓▓▓▓▓░░░░░░        │  │ Addresses            │  │               next │
│ Platinum in progress │  │ ─────────────────    │  ├────────────────────┤
├──────────────────────┤  ├──────────────────────┤  │ Customer Approval  │
│ Customer Portal      │  │ Improved Job     Jul │  │ Workflow        Q4 │
│ Improvements         │  │ Search               │  ├────────────────────┤
│ ▓▓▓▓░░░░░░░░░        │  │ ─────────────────    │  │ Shipment Templates │
│ Portal  in progress  │  ├──────────────────────┤  │                 Q4 │
├──────────────────────┤  │ New Proof        Jul │  ├────────────────────┤
│ Shipping Workflow    │  │ Notifications        │  │ Have a request?    │
│ ▓▓░░░░░░░░░░░        │  │ ─────────────────    │  │ Submit an idea →   │
└──────────────────────┘  └──────────────────────┘  └────────────────────┘
                          Full changelog →
```

### 13.2 Requirements

- **Header** states the update cadence and where to direct questions. A roadmap with no stated cadence is assumed stale.
- **Currently Working On** — items with status `In Progress`, published to Roadmap. Each card shows title, product pill, status pill, and a coarse progress indicator. Progress is deliberately coarse and derived from the item's design checklist and status, not from a percentage anyone maintains by hand.
- **Recently Released** — items that entered `Released`, newest first, with release month. Capped at a fixed count with `Full changelog →` for the complete history.
- **Coming Up** — items with status `Planned`, showing a timeframe label (`next`, `Q4`). Timeframes are intentionally imprecise. No specific dates appear on this surface.
- **Request intake** — a `Submit an idea →` entry point at the foot of Coming Up. Submissions create Idea items in Projects with `#department/<submitter's department>` applied automatically. This is the front door for other departments and replaces hallway requests. (Intake is listed as MVP-optional in §16 — if deferred, the affordance links to the team channel instead.)
- **No internal complexity.** Priorities, owners, tags, checklists, and DevOps links are not exposed here.

### 13.3 Editorial control

Not every in-progress item belongs on the Roadmap. Publication is opt-in per item, and the Roadmap title and blurb shown publicly may differ from the internal item title — the internal record can say _Rework Platinum Job Management_, while the public card says _Platinum Job Management_. Both live on the same item.

---

## 14. Publishing Model

This section defines the CMS behavior that connects the three tabs.

### 14.1 Add Knowledge Article

Invoked from an item in Projects. Behavior:

1. Creates a related Knowledge Article, pre-populated with the source item's product and area tags.
2. Opens the article editor
3. On save, sets a verification date and publishes to the Knowledgebase tree at the position implied by its tags.

An item may have multiple knowledge articles. An article may document multiple items.

### 14.2 Add Changelog Article

Invoked from an item in Projects, typically at `Complete` or `Released`. Behavior:

1. Creates a related Change item pre-populated with the source item's title and product.
2. Prompts for a plain-language, company-audience description — what changed and why it matters, not how it was built.
3. Publishes to the changelog, and to the Roadmap's Recently Released column when the source item reaches `Released`.

An item may have multiple changelog articles. An article may document multiple items.

### 14.3 Publication indicators

Board and List items display a small marker when published (`changelog ✓`, `kb ✓`) so the team can see at a glance which released work has been communicated and which has not. An item in `Released` with no changelog entry is a visible gap, and should be surfacable as a filter.

### 14.4 Editing and unpublishing

- Editing the source item does not silently rewrite published content; published article bodies are edited directly on the article record.
- Unpublishing removes the item from the public surface without deleting the underlying item or its history.

---

## 15. Search and Discovery

Because the system functions as both a project-management tool and a knowledge base, search is critical.

Search operates across titles, descriptions, tags, products, people, relationships, knowledge articles, changelog entries, projects, and features.

Search returns both **work** and **knowledge**. Searching:

> shipping date

might return:

- item: Flexible Shipping Dates
- Knowledge: How Shipping Dates Work
- Changelog: Flexible Shipping Dates
- item: Shipping Date Calculation

### 15.1 Scoped vs. global

- Search inside Projects respects the active tag scope by default, with a one-click escape to search everything.
- Search inside Knowledgebase is limited to published articles.
- Results are grouped by item type, since a mixed flat list of work and documentation is hard to read.

### 15.2 ⌘K

⌘K is a combined command palette: capture a new item, jump to an item, or search. Capture is the default action when the input does not match an existing item.

---

## 16. DevOps Integration

DevOps remains the technical system of record. The platform stores links between product-level items and technical implementation.

```text
Feature "Allow bulk editing of jobs"  →  Azure DevOps Epic / Feature / Work Items
```

The platform may display high-level implementation status (Not started / In development / Complete) but does not recreate the work-item hierarchy.

Future integrations could synchronize DevOps links, work-item status, release status, development completion, and deployment information. Integration is not required for the initial version — a stored URL is sufficient for MVP.

---

## 17. UX Principles

### 17.1 Capture First, Organize Later

Creating an item must be extremely low-friction. ⌘K from anywhere, title only, save to Backlog.

### 17.2 Avoid Forced Hierarchy

Not everything is a project. A small improvement exists independently.

### 17.3 Relationships Over Duplication

Link information instead of copying it.

### 17.4 One Source of Truth

A feature is never entered separately into backlog, roadmap, changelog, and knowledge base. Projects is the source; the other tabs are published views.

### 17.5 Progressive Structure

Simple items stay simple. Additional structure — checklists, publishing — appears as complexity increases.

### 17.6 Product Language Over Engineering Language

Terminology appropriate for product management and the broader organization. Technical detail belongs in DevOps.

### 17.7 Separate the Internal from the Published

Every surface has one audience. Projects never has to be sanitized for outside readers, and the published tabs never expose internal planning noise. This is why the tab split exists.

### 17.8 Make Staleness Visible

Verification dates, update cadence, and publication indicators exist so that decay is observable rather than silent.

---

## 18. MVP

The first version establishes the core information model and the three-tab split, rather than a complete project-management suite.

### Core

- Create/edit/delete items
- Item types
- Status, priority, owner, product
- Tags and nested tags
- Tag tree with counts and tag filtering
- ⌘K quick capture with count-annotated autocomplete
- Relationships between items
- Search

### Projects Tab

- List view with tag scope, filter chips, sort
- Board view across the six lifecycle columns
- Item detail with description, meta, and design checklist
- DevOps link field
- Publication indicators

### Knowledgebase Tab

- Tag-derived article tree
- Article view with sections, tags, and verification date
- Related panel
- Knowledgebase search
- Deep link back to source item for editing

### Roadmap Tab

- Currently Working On / Recently Released / Coming Up
- Per-item publish opt-in and public-facing title override
- Cadence header
- Link to full changelog

### Publishing

- Add Knowledge Article
- Add Changelog Article

### Deferred from MVP

- Request intake form (link to team channel instead)
- Automated progress indicators (manual coarse status acceptable initially)
- Needs-review queue for stale articles (flag only)
- Any DevOps synchronization

---

## 19. Future Features

- **Roadmap timeline** — visualize initiatives over time rather than in three columns.
- **Product analytics** — associate features with metrics and outcomes.
- **User feedback** — connect customer and internal feedback to problems and features.
- **Request intake** — full submission workflow with triage queue and status notification back to the requester.

---

## 20. Worked Example

### Platinum Job Management Rework

Tags:

```text
#product/platinum
#product/platinum/jobs
#initiative/wms-rework
```

**In Projects**, the project relates to:

- **Knowledge** — how jobs are structured, how shipping dates are calculated, job status definitions.
- **DevOps** — each feature links to its technical implementation.

Each feature carries a design checklist tracking wireframes, spec, and handoff.

**In Knowledgebase**, under `Platinum → Jobs`, readers find _How shipping dates are calculated in Platinum_, with a Related panel linking to the Flexible Shipping Dates feature and the navigation-model decision.

**On the Roadmap**, the company sees _Platinum Job Management — in progress_ under Currently Working On, _Saved Ship-To Addresses — Aug_ under Recently Released, and _Bulk Job Editing — next_ under Coming Up.

One set of records. Three audiences.

---

## 21. Success Criteria

The platform is successful if the Product Team can answer the following without consulting multiple systems:

|Question|Answered by|
|---|---|
|What should we build next?|Projects — Backlog|
|What are we currently working on?|Projects — Board|
|Where does my design work stand on this?|Item detail — checklist|
|What have we changed recently?|Changelog / Roadmap|
|How does this product or process work?|Knowledgebase|
|Why was this decision made?|Decision items + Related panels|
|What technical work corresponds to this feature?|DevOps link|

And if the rest of the company can answer, without asking anyone:

|Question|Answered by|
|---|---|
|What is Product working on?|Roadmap|
|What can we expect next?|Roadmap — Coming Up|
|How does this part of the product work?|Knowledgebase|
|How do I request something?|Roadmap — Submit an idea|

Additional health measures:

- Percentage of Released items with a published changelog entry.
- Percentage of Knowledgebase articles verified within the last six months.
- Time from capture to first tag applied (should be near zero).

---

## 22. Guiding Principle

The platform should behave less like a traditional project-management application and more like a **living map of the Product Team's work and knowledge** — with two windows cut into it for everyone else.

The goal is not to answer:

> "What tasks do we have?"

It is to answer:

> **"What are our products, what are we changing about them, why are we changing them, what have we learned, and where are we going next?"**

— and to answer it for the whole company, not just for us.