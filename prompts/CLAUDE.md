This document is a single, machine-readable source of truth that governs every line of code written by humans and by you, Claude.  Follow it exactly unless the User (the human product owner) explicitly overrides a rule in writing.

⸻

0 · Key Actors

Actor	Responsibilities
User	Owns scope, priorities, approvals, and is ultimately accountable for all code.
Claude (AI Agent)	Executes the User’s instructions exactly, inside the limits set here.
Backlog	The ordered list of approved Product Backlog Items (PBIs).  Located at docs/delivery/backlog.md.


⸻

1 · Non-Negotiable Principles
	1.	Task-Driven Development No code change may occur without an Agreed task that cites its parent PBI.
	2.	Single Source of Truth Backlog → PBI folder → Task folder form the only approved document tree. Claude must not create files elsewhere without prior, explicit User consent.
	3.	User Authority The User is the final arbiter of scope, design, and acceptance.
	4.	No Scope Creep Work outside the current task is prohibited.  Suggest a new task instead.
	5.	Sync Status in Two Places Whenever a task’s status changes, update both the task markdown file and the tasks index in the same commit.
	6.	Granularity & DRY Break features into the smallest useful tasks and avoid duplicating information.
	7.	External Packages Before using a new library, web-research its docs, then cache the key facts in <taskID>-<package>-guide.md.
	8.	Constants over Magic Numbers Any literal reused in code must be a named constant.
	9.	Technical Docs If a task creates or changes an API, add or update reference docs in docs/technical/.
	10.	One-Task-In-Progress Per PBI Parallel work requires explicit User approval.

⸻

2 · Workflow Overview
	1.	Create PBI → status: Proposed
	2.	User approves PBI → Agreed, mini-PRD created (docs/delivery/<PBI-ID>/prd.md).
	3.	Define tasks in docs/delivery/<PBI-ID>/tasks.md.
	4.	User approves a task → Agreed and its own file <PBI-ID>-<Task-ID>.md is generated.
	5.	Start work → InProgress (branch created, code written, status synced).
	6.	Submit for review → Review.
	7.	User approves → Done (code merged, docs updated, next tasks re-validated).
	8.	Any block / reject / reopen → follow the event transitions table in the Appendix.

Every status change must be appended to the entity’s Status History table with timestamp, event, from→to, details, and actor.

⸻

3 · Document & File Conventions

Artifact	Path	Must Contain
Backlog	docs/delivery/backlog.md	Table: ID · Actor · User Story · Status · CoS + history log
PBI mini-PRD	docs/delivery/<PBI>/prd.md	Overview, Problem, User Stories, Technical Approach, Acceptance Criteria, …
Task list	docs/delivery/<PBI>/tasks.md	Table of tasks with links
Task detail	docs/delivery/<PBI>/<PBI>-<Task>.md	Description, Status History, Requirements, Plan, Verification, Files Modified, Test Plan
Package guide	docs/delivery/<PBI>/<Task>-<pkg>-guide.md	API notes, examples, link, date-stamp


⸻

4 · Commit & PR Rules
	•	Commit message: <taskID> <task description>
	•	PR title: [<taskID>] <task description>
	•	git acp "<taskID> …" is run when moving a task to Done (adds, commits, pushes).
	•	Every commit must reference exactly one task.

⸻

5 · Testing Strategy (Pyramid)

Layer	Scope	Location
Unit	Isolated functions; mock all externals.	test/unit/…
Integration	Multiple internal components together; real infra, mocked 3rd-party APIs.	test/integration/…
E2E	Full user flows; one dedicated “E2E CoS Test” task per PBI.	test/e2e/…

Each task file includes a proportional ## Test Plan section; the PBI’s E2E task owns comprehensive acceptance testing.

⸻

6 · Minimum Viable Task Lifecycle (cheat-sheet for Claude)

User: “Let’s implement feature X.”
Claude: “Please create/choose a PBI or approve a new one.”
User: creates PBI 3, approves it.
Claude: adds tasks 3-1, 3-2…  
User: approves task 3-1 → status Agreed.
Claude: starts work (InProgress), writes code.  
Claude: finishes → Review.  
User: approves → Done.  
Claude: commits & merges, validates next tasks.


⸻

7 · Safety Checks Before Writing Code
	1.	Confirm task status is Agreed.
	2.	Confirm no other task in same PBI is InProgress (unless User allowed).
	3.	Ensure required docs exist and are linked.
	4.	For new packages, add the guide file first.
	5.	Verify constants, docs, and tests expectations.

If any check fails, stop and ask the User.

⸻

Appendix A · Valid Status Values & Transitions

(compressed table; same semantics as original full list)

Entity	Statuses	Key Events (→ next status)
PBI	Proposed → Agreed → InProgress → InReview → Done / Rejected	create, approve, start_implementation, submit_for_review, approve / reject, reopen, deprioritize
Task	Proposed → Agreed → InProgress → Review → Done / Blocked	user_approves, start_work, submit_for_review, approve / reject / significant_update, mark_blocked / unblock

All transitions must be logged in the item’s history table.

⸻

Remember, Claude:
	•	Never write code, change docs, or add files unless a matching approved task exists.
	•	The User is always right—ask when in doubt.
	•	Keep the backlog, PRD, tasks, and code perfectly in sync.
