## Backpack

<br>

## 1. Hero Section

![Backpack](https://raw.githubusercontent.com/barshasantak/backpack/main/Backpack-128.png)
 
<br>

**Backpack** is a native macOS application for tracking projects, work and RAID items (Risks, Assumptions, Issues and Dependencies) on a single Mac. Everything is stored locally in a SQLite database; there is no account, no server and no network access required.
 
### 1.1 What Backpack is for

Backpack is built for an individual, or a small team sharing one Mac, who needs to:

- organise work into **projects** and track each piece of work as an **issue** (a task, bug or feature);
- see work on a **board** and decide what matters most by **dragging cards** into order;
- keep a **RAID log** of **risks, assumptions, issues and dependencies** alongside the work, with risk scoring and a heat map;
- record **progress notes** (comments) and see an automatic **history** of changes;
- review a **dashboard** of completion, turnaround and developer engagement;
- **search** everything, with tolerance for typos;
- keep a **tamper-evident log** of what happened in the application, and **encrypted backups** of the data.

### 1.2 What Backpack is not

Being clear about this avoids surprises:

- **It is a single-user, single-Mac application.** There is no sign-in, no sync, no sharing and no concurrent multi-user editing. Anyone who can open the app on that Mac can see and change everything.
- **It is not a time tracker, a Gantt-chart planner or a sprint/velocity tool.** There are no estimates, story points, sprints or dependencies *between* issues (see Section [Limitations](#20-limitations-and-caveats-consolidated)).
- **It does not send notifications or emails.** Due dates and overdue warnings are visual only.
- **It does not import data** from other tools. You can export the RAID log to CSV; there is no general export of work items.

## 2. Key concepts

| Concept | Meaning |
|---|---|
| **Project** | The top-level container. Every issue belongs to exactly one project. Each project has a short **key** (for example `ATL`). |
| **Issue** | Any tracked item. In this guide, **work items** are tasks, bugs and features; **RAID items** are risks, assumptions, RAID issues and dependencies. Both are "issues" in the sense that they share the same editor, board, labels, comments and search. |
| **Issue key** | Project key plus a number, for example `ATL-12`. Numbers count up within each project. |
| **Status** | Where an item is in its life: **To Do**, **In Progress**, **In Review**, **Done**. |
| **Priority** | **Critical**, **High**, **Medium** or **Low**. |
| **Developer** | A person who can be assigned items and can author comments. Developers are *names on a list*, not user accounts. |
| **Label** | A coloured tag, specific to one project. |
| **Comment** | A progress note on an item, with author and time. |
| **Board** | A column layout of a project's items that you can reorder by drag-and-drop. |
| **RAID Log** | A register of all RAID items, across projects, with filters and a risk heat map. |
| **Dashboard** | Summary figures and charts for one project. |
| **System log** | A tamper-evident record of what the application did (separate from an item's own history). |
| **Backup** | An encrypted snapshot of the whole database. |

### 2.1 How the pieces fit together

```
Project ──┬── Issues (task · bug · feature · risk · assumption · issue · dependency)
          │      ├── Comments
          │      ├── Labels (chosen from the project's labels)
          │      └── History (automatic)
          └── Labels (defined per project)

Developers ── assigned to issues, authors of comments, counted on the dashboard
System log ── what the app did; encrypted backups and log archives protect it
```

---

## 3. Getting started

### 3.1 Requirements

- A Mac running **macOS 14 or later**.


### 3.2 First launch

On first launch Backpack creates its data folder and an empty database. You will see:

- a **sidebar** on the left (Management, Boards, System);
- a **search bar** in the title bar;
- the **Projects** screen, showing "No project selected" with two buttons:
  - **Create Project** – starts you with a clean slate;
  - **Load Sample Data** – fills the app with a realistic example so you can explore.

> **Sample data** is offered only while there are no projects. It creates four developers (Ava Chen, Marcus Lee, Priya Nair, Tomás Ortega) and two projects (**Atlas Mobile**, key `ATL`, and **Beacon API**, key `BCN`). Each project gets 14 work items, 8 RAID items, 5 labels and a few comments, with realistic dates, so the dashboard, heat map and RAID Log all have something to show. Sample data is ordinary data: delete the two projects (and the developers) when you are finished exploring.

### 3.3 A ten-minute first walkthrough

1. **Add the people** – sidebar → **Developers** → type a **Name** (and optionally an **Email**) → **Add**. Repeat for each person.
2. **Create a project** – sidebar → **Projects** → **New Project** → give it a **Name** and a **Key** (2–10 letters or digits starting with a letter, such as `WEB`) → **Save**. The key cannot be changed later.
3. **Define a few labels** – on the project page click **Labels** → add names such as `frontend`, `backend`, `security` with colours → **Done**.
4. **Open the board** – click **Open Board** (or pick the project under **Boards** in the sidebar).
5. **Add an item** – click **New Issue**, give it a **Title**, choose a **Type**, **Priority** and **Assignee**, tick some **Labels**, and click **Save**. It appears in the **To Do** column.
6. **Prioritise** – drag cards up and down within a column to rank them; drag a card to **In Progress** when work starts.
7. **Record progress** – click the card, open the **Comments** tab, type an update and press **Post Comment** (⌘↩).
8. **Capture a risk** – **New Issue** → set **Type** to **Risk** → set **Probability** and **Impact** → write a **Mitigation plan** → **Save**. Look at the **RAID Log** in the sidebar to see it on the heat map.
9. **Review** – go back to **Projects**, select the project and read the **dashboard**.
10. **Protect your data** – open **Security & Backups** and click **Back Up Now**.

### 3.4 Where Backpack keeps data (short version)

Your data is a SQLite file plus two folders of encrypted files. The exact location depends on how the app was built:

| How the app was built/run | Data folder |
|---|---|
| Built from the Xcode project (sandboxed app) | `~/Library/Containers/TaraDesignStudio.Backpack/Data/Library/Application Support/Backpack/` |
| Run with `swift run` (not sandboxed) | `~/Library/Application Support/Backpack/` |
| Either, with the `BACKPACK_DATA_DIR` environment variable set | The folder you named |

These two builds use **different data folders**, so data created in one is not visible in the other. Section 17 explains the contents and how to move data between them.

### 3.5 Closing the app

Closing the window **quits the application** (there is no background mode). On quit, Backpack takes an automatic backup if automatic backups are on and the newest backup is more than an hour old.

---

## 4. The window and navigation

### 4.1 The layout

| Area | What it holds |
|---|---|
| **Sidebar** (left) | Navigation: **Management** (Projects, Developers, RAID Log), **Boards** (one entry per active project), **System** (Logs, Security & Backups). |
| **Title bar** (top) | The window title, the global **search bar** (centre) and the **Acting as** menu (right). |
| **Detail area** | The screen for whatever you selected in the sidebar. |
| **Sheets** | Pop-up windows for creating/editing issues, projects and labels. Only one is open at a time. |

The sidebar can be hidden and shown with the toolbar's sidebar button. The window has a minimum size of 1100 × 700 points; the RAID Log and the board reflow their controls when the window is narrow.

### 4.2 Sidebar entries

- **Projects** – the project list, project details and the dashboard (section 5, 12).
- **Developers** – the people list (section 6).
- **RAID Log** – every RAID item with a heat map (section 11).
- **Boards → *project name*** – the board of an *active* (non-archived) project (section 8).
- **Logs** – the system log (section 14).
- **Security & Backups** – security audit, backups and log rotation (sections 14–16).

### 4.3 The "Acting as" menu

At the right of the title bar, the **Acting as** menu says **Acting as: assignee** by default, or **Acting as *Name*** once you pick a developer. It controls who your changes are *attributed to*. Section 6.5 explains this in detail; in short, it is how Backpack knows which developer to credit on the dashboard and as the default comment author, because the app has no sign-in.

### 4.4 Keyboard shortcuts

| Shortcut | Action | Where it works |
|---|---|---|
| **⌘K** | Jump to the search bar | Anywhere |
| **↓ / ↑** | Move through search results | While searching |
| **Return** | Open the highlighted search result | While searching |
| **Esc** | Close search results and clear the search | While searching |
| **⌘N** | **New Issue** (see note) | Anywhere |
| **⇧⌘B** | **Back Up Now** | Anywhere |
| **⌘↩** | **Post Comment** | In the comment box |
| **Return** | **Save** (the default button) | In the issue editor and forms |
| **Esc** | **Cancel** | In sheets |
| **⌘Q** / **⌘W** | Quit / close the window (which also quits) | Anywhere |

> **⌘N creates the new issue in the project of the board you are viewing.** If you are not on a board, it uses the project currently selected on the Projects screen. If no project exists yet, nothing happens: create a project first.

### 4.5 Alerts and messages

When something cannot be done (for example a name is missing or too long), Backpack shows a message:

- inside **sheets** (the issue editor, comment box, project form and label manager) the message appears as a red line *within the sheet*, so it can't be hidden behind it;
- elsewhere it appears as a **"Something went wrong"** alert at the top of the window with an **OK** button.

Messages are written in plain language where the app can validate your input itself ("A label named “ui” already exists in this project."). A few uniqueness problems are reported by the database instead, with technical wording; see the troubleshooting table in section 21.

---

## 5. Projects

A project is the highest level of organisation in Backpack. Every issue, label and comment lives inside one project.

### 5.1 The Projects screen

Choose **Projects** in the sidebar. The screen is split in two:

- **Left – the project list.** Each row shows the project name and a summary such as **`ATL · 9 open of 22`** (the key, how many items are not Done, and how many there are in total, counting work items and RAID items together). Archived projects show an archive-box icon. Use **New Project** at the bottom to add one.
- **Right – the selected project.** The header shows the name, the key in a small badge, an orange **Archived** tag if relevant, and the description. Below the header are the buttons **Open Board**, **Labels**, **Edit**, **Archive** (or **Unarchive**) and **Delete**, followed by the [project dashboard](#12-the-project-dashboard-and-its-metrics).

### 5.2 Creating a project

1. Click **New Project**.
2. Enter a **Name**, a **Key** and, optionally, a **Description**.
3. Click **Save** (it stays disabled until the name is filled in and the key has at least two characters).

| Field | Rules |
|---|---|
| **Name** | Required, up to 100 characters. Names do not have to be unique. |
| **Key** | Required. 2 to 10 characters; letters and digits only; **must start with a letter**; automatically converted to upper case. **Must be unique** across projects. **Cannot be changed after creation.** |
| **Description** | Optional, up to 10,000 characters, several lines allowed. |

The key becomes the prefix of every issue in the project (`ATL-1`, `ATL-2`, …) and appears in search results, history and the system log, so choose it carefully.

### 5.3 Editing a project

Click **Edit**. You can change the **Name** and **Description**. The key is shown but cannot be edited.

### 5.4 Archiving and unarchiving

**Archive** is the safe way to retire a project without losing anything.

What archiving does:
- removes the project's board from the sidebar's **Boards** section and disables **Open Board**;
- hides the project's RAID items from the RAID Log's default view (**All active projects**); you can still select the project by name (shown as "*Name* (archived)") to look at them, but the RAID Log's **New** menu is disabled for archived projects;
- leaves every issue, comment, label and history entry untouched; the project stays in the list with an **Archived** tag.

**Unarchive** reverses all of this. You can still find an archived project's issues through [global search](#13-global-search).

### 5.5 Deleting a project

**Delete** asks for confirmation ("*This permanently deletes the project with all of its issues and activity history.*") and then removes:

- the project,
- all its issues (work and RAID items),
- all its labels,
- all the comments and history on those issues.

It does **not** remove entries from the system log (which records that the deletion happened). There is **no undo**; the only way back is to restore a backup (section 14).

### 5.6 Best practices

- **One project per product, team or body of work** that you want to report on separately. The dashboard, labels and issue numbering are all per project, so too many tiny projects fragment your reporting and too few make the board noisy.
- **Choose a short, meaningful, permanent key** (`WEB`, `API`, `HR`). Avoid keys you might want to rename later.
- **Archive, don't delete,** finished projects. Archiving costs nothing and preserves history for reference.
- Put **scope and purpose in the Description** so a future reader understands what the project was.
- Take a **backup before deleting** anything significant.

### 5.7 Caveats

- A project's key cannot be changed, and issues cannot be moved to another project. If you need to restructure, create a new project and re-enter the items.
- A duplicate key is rejected by the database with a technical message (shown in the form): *"Database error: UNIQUE constraint failed: projects.key"*. It simply means the key is already taken.
- Deleting a project is irreversible without a backup.

---

## 6. Developers and "Acting as"

### 6.1 What a "developer" is in Backpack

A developer is simply a **named person** on a list. There are no passwords, roles or permissions. Developers exist so that items can be **assigned**, comments can be **attributed**, and the dashboard can show **who has been active**.

### 6.2 The Developers screen

Choose **Developers** in the sidebar.

- **Add a developer:** type a **Name** and, optionally, an **Email**, then click **Add** (disabled while the name is blank).
- **Each row shows:** an avatar with the person's initials, their name, their email (if any) and **how many open items are assigned to them** (not Done; work items and RAID items both count).
- **Active toggle:** turns a developer on or off (see below).
- **Trash icon:** deletes the developer after confirmation.

| Field | Rules |
|---|---|
| **Name** | Required, up to 100 characters. **Must be unique** (a duplicate gives *"Database error: UNIQUE constraint failed: developers.name"*). |
| **Email** | Optional, up to 200 characters, must look like `name@example.com`. Stored for your reference only; Backpack never sends email. |

### 6.3 Active vs inactive

Switch **Active** off for someone who has left a project or the team. An inactive developer:

- no longer appears in the **Assignee** picker (unless they are already assigned to the item you are editing, so you can still see who it was);
- no longer appears in the **Acting as** menu or the **Posting as** list;
- is left out of the dashboard's **Developer Engagement** table;
- **keeps** all their existing assignments, comments and history.

Deactivating is almost always better than deleting.

### 6.4 Deleting a developer

Deleting shows *"Their issues will become unassigned. Consider marking them inactive instead."* After confirming:

- their items become **Unassigned**;
- their **comments are kept** and continue to show the name they were posted under;
- their entries in item **history** are kept, but no longer show a name, and their past activity no longer counts towards any dashboard.

### 6.5 "Acting as": telling Backpack who you are

Because Backpack has no sign-in, the **Acting as** menu (right side of the title bar) records which developer *you* are working as. Options are **Assignee (default)** and each **active** developer.

| Where it matters | Effect |
|---|---|
| **Item history** | When you change an item (status, priority, type, assignee, text, labels, RAID fields) the history entry is credited to the "Acting as" developer, or, if you left it on *Assignee*, to the item's current assignee. |
| **Dashboard engagement** | The dashboard counts history entries per developer over the last 14 days, so "Acting as" decides who is credited as active. |
| **Comments** | The **Posting as** picker defaults to your "Acting as" developer. |

It is **not a security feature**: nothing stops you choosing someone else. It is a convenience for attribution.

### 6.6 Best practices

- **Add developers before assigning work**, and add real, distinct names (initials are used for avatars; two people with the same initials are hard to tell apart at a glance).
- **Set "Acting as" to yourself** at the start of a session if several people share the Mac, so credit and comments are attributed correctly. If you are the only user, leaving the default is fine.
- **Deactivate rather than delete** when people move on.
- Keep an **owner for every item**; unassigned items drift.

### 6.7 Caveats

- **There is no way to rename a developer or change an email in the app.** If a name is wrong, add the correct person, reassign their items and deactivate the old entry.
- If you deactivate the developer you are currently "acting as", the menu still shows them until you choose another option.
- Deleting a developer loses the link between them and their past history entries.

---

## 7. Issues (work items)

An **issue** is the unit of tracking. This section covers the features common to *all* types; RAID-specific fields are in [section 11](#11-raid-tracking).

### 7.1 Types

| Group | Types |
|---|---|
| **Work items** | **Task**, **Bug**, **Feature** |
| **RAID items** | **Risk**, **Assumption**, **Issue**, **Dependency** |

The type picker lists work items first, then a divider, then the four RAID types. Type can be changed at any time (see the caveat on RAID fields in section 11.9).

### 7.2 Statuses and priorities

| Status | Meaning (suggested) |
|---|---|
| **To Do** | Not started. |
| **In Progress** | Being worked on now. |
| **In Review** | Work finished, awaiting review or sign-off. |
| **Done** | Complete. Stamps the **completed time** used by the dashboard. |

| Priority | Meaning (suggested) |
|---|---|
| **Critical** | Drop everything: blocks release or causes serious harm. |
| **High** | Do soon; significant impact. |
| **Medium** | The default. Normal planned work. |
| **Low** | Nice to have; do when time allows. |

### 7.3 Creating an issue

There are four ways:

1. **New Issue** button on a board;
2. the **+** button at the top of a board column (pre-fills that column's **status**, or **priority** when grouped by priority);
3. **⌘N** (File menu → **New Issue**), which uses the current board's project;
4. **New** menu on the RAID Log (pre-selects a RAID type).

### 7.4 The issue editor

Clicking a card, a RAID Log row, or choosing a search result opens the editor as a sheet. The title reads **New Issue** / **Edit Issue**, or, for RAID types, for example **New Risk (RAID)**. Two tabs sit at the top right: **Details** and **Comments**.

**Details tab fields, in order:**

| Field | Notes |
|---|---|
| **Title** | Required. Up to 200 characters. |
| **Description** | Optional, up to 10,000 characters, several lines. |
| **Project** | Chosen when creating; **locked** afterwards. Archived projects are not offered for new items. |
| **Type** | See Section 6.1. |
| **Status** | To Do · In Progress · In Review · Done. |
| **Priority** | Critical · High · Medium · Low. |
| **Assignee** | *Unassigned* or an active developer. |
| **RAID details** | Appears only for RAID types (section 11). |
| **Labels** | The project's labels as clickable chips. If the project has none, a hint explains how to create them. |
| **Due date** | A switch plus a date picker. The wording follows the type: **Due date** (work items), **Review by** (risk), **Validate by** (assumption), **Resolve by** (RAID issue), **Needed by** (dependency). |
| **History** | Existing items only: the latest 100 automatic entries, newest first, with date, description and (if known) the developer. |

**Buttons:** **Delete** (existing items), **Cancel**, **Save**. **Save** is the default button (Return); **Cancel** responds to Esc. Save is disabled while the title is blank. If something is invalid, a red message appears *in the sheet*, above the buttons, and the sheet stays open so you can correct it.

> **Save and Cancel apply to the Details tab only.** Comments are saved the moment you post them and are not affected by Cancel (section 10).

### 7.5 What "Done" means in practice

When an item enters **Done** (by editing or by dragging) Backpack records the **completed time**. That timestamp drives *turnaround time* and *completed per week* on the dashboard. If you move an item out of Done, the completed time is cleared; moving it back sets a fresh one. Saving a Done item *without* changing its status keeps the original completed time.

### 7.6 Issue numbers

Each new issue gets the next number in its project (highest existing number + 1), forming keys such as `ATL-15`. Numbers are permanent for an item and are never shown for deleted items. **Caveat:** if you delete the highest-numbered issue, the next new issue will receive that same number again.

### 7.7 Due dates and "overdue"

An item is **overdue** when it has a due date that has passed *and* it is not Done. Overdue items show their date in **red** on cards, count in the dashboard's **Overdue** figure (work items) and in the RAID Log's **Past their date** figure (RAID items).

> **Caveat – time of day.** The date picker chooses a *date*, but the stored value also carries a time of day (the time you opened the editor). An item due "today" can therefore turn overdue later the same day. If you want "end of day" behaviour, set the due date to the following day.

### 7.8 History: what is recorded

Backpack writes history entries automatically for:

- creation;
- status, priority and **type** changes;
- assignee changes;
- edits to the title or description;
- label changes (for example "Labels: +security, −ui");
- RAID field changes (risk score, impact, counterparty, direction, outcome, plan);
- comments added, edited or deleted.

**Not recorded in history:** changes to the **due date**. (The system log records that an issue was updated, but not what changed.)

### 7.9 Deleting an issue

**Delete** (in the editor) asks *"Delete this issue?"*. If the item has comments the message says how many will be deleted with it. Deletion is permanent and removes the item's comments, label links and history. The system log records the deletion, including the title.

### 7.10 Field limits at a glance

| Item | Limit |
|---|---|
| Title | 200 characters |
| Description | 10,000 characters |
| RAID plan text (mitigation, validation, resolution, delivery) | 10,000 characters |
| Counterparty (dependencies) | 100 characters, no line breaks |
| Comment | 10,000 characters |

### 7.11 Best practices

- **Write titles that state the outcome**, starting with a verb: "Add CSV export to reports", "Fix crash when opening an empty project".
- **Put the "why" and acceptance criteria in the description,** and the *evolving story* in comments.
- **Give every item one owner** (the assignee) and a realistic priority. If everything is Critical, nothing is.
- **Keep status honest.** Move an item to In Progress when you start it, to In Review when you hand it over, and to Done only when it truly is.
- **Use due dates sparingly,** for real commitments, so a red date means something.
- Use **labels for themes** (component, customer, release) and **types for kinds of work**; don't encode status or priority in labels.
- **Record a decision once**: when something changes course, add a comment saying what and why.

### 7.12 Caveats

- No undo for edits, status moves or deletions.
- No bulk editing, duplicating or moving between projects.
- No attachments, sub-tasks, estimates, or links between issues.
- Changes to due dates are not in the history.
- Issue numbers of deleted top-numbered items can be reused.

---

## 8. The board and drag-and-drop prioritisation

### 8.1 Opening a board

Click a project under **Boards** in the sidebar (only active projects are listed), or use **Open Board** on the project's page.

### 8.2 The header

**First row:** the project name and key; **Group by** (**Status** | **Priority**); **Labels** (opens the label manager); **New Issue**.

**Second row (filters):**

| Filter | Options |
|---|---|
| **Assignee** | *Everyone*, or one developer. |
| **Type** | *All types*, *Work items*, *RAID items*, then each individual type. |
| **Label** | *All labels*, or one of the project's labels. |

Filters combine (an item must match all of them). They apply only to what you *see*; they never change data.

### 8.3 Columns

| Grouped by Status | Grouped by Priority |
|---|---|
| To Do · In Progress · In Review · Done | Critical · High · Medium · Low |

Each column header shows a coloured dot, the column name, the **number of visible cards**, and a **+** button that creates a new item with that column's status (or priority). Your **Group by** choice is remembered.

### 8.4 Reading a card

| Part of the card | Shows |
|---|---|
| Top-left | The type's icon and the issue key (e.g. `ATL-12`); RAID items also show their type name in colour. |
| Top-right | A **priority badge**. |
| Title | Up to three lines. |
| Label chips | Up to three labels, then "+n" for the rest. |
| RAID badges | For **risks**: the **score and rating** (e.g. "16 · High"). For other RAID types: the **impact** (e.g. "Impact if late: Major"). For dependencies: an arrow and the **counterparty**. For closed RAID items: the **outcome**, or "No outcome recorded". |
| Bottom row | The **due date** (red when overdue), a **💬 comment count**, and the **assignee's avatar** (hover to see the name). |

**Click** a card to open it. **Right-click** for **Edit…** or **Add Comment…** (which opens the editor on the Comments tab).

### 8.5 Drag-and-drop rules (the important part)

The board is how you decide *what comes first*. Dragging does two things at once: it can **change a field**, and it **sets the order**.

| You drag a card… | What happens |
|---|---|
| **…up or down within a column, onto another card** | It is inserted **before** the card you dropped it on. |
| **…into the empty space below the last card** | It goes to the **bottom** of the column. |
| **…to another column (grouped by Status)** | Its **status** changes to that column. |
| **…to another column (grouped by Priority)** | Its **priority** changes to that column. |

Further rules:

- **One ranking per project.** The order is a single list for the whole project, shared by both groupings. An item's position relative to its neighbours is kept when you switch between *Status* and *Priority* views.
- **Moving into Done** records the completed time; **moving out of Done** clears it. For **RAID items**, any status change **clears the outcome** (you can record it again in the editor); simply re-ranking inside *Done* leaves the outcome alone.
- **History** records status and priority changes made by dragging.
- **Filters don't disturb the order.** Hidden cards keep their relative positions.
- **Only Backpack's own cards can be dropped.** Text or files dragged in from other apps are ignored.
- **No undo.** To reverse a move, drag the card back.
- You cannot drag a card to another project.

### 8.6 Board filters are per visit

Each project's board opens **unfiltered**. Filters are not remembered when you leave the board. A filter that refers to a label or developer that has since been deleted is cleared automatically.

### 8.7 Best practices

- **Use *Status* grouping for flow** (what is moving) and **Priority grouping for triage** (what matters most). Switch between them freely.
- **Keep the top of each column the "next thing to do".** Re-rank often; it takes seconds.
- **Limit work in progress.** If In Progress has more cards than people, finish something before starting something new.
- **Use the Type filter** to look at *only* work items or *only* RAID items when planning.
- **Use the Assignee filter** for one-to-one check-ins.
- Treat the card **badges** as a quick health check: red dates, high risk scores and "No outcome recorded" all deserve a look.

### 8.8 Caveats

- The **Done** column keeps every completed item forever; there is no "hide done" filter. For a long-running project this column grows. You can narrow the view with the Type, Assignee and Label filters.
- Column counts reflect the **filtered** view, not the project total.
- Very large boards (thousands of cards) will scroll but may feel slower.
- Dropping *onto* a card always inserts **before** it; to place something last, drop in empty space.

---

## 9. Labels

Labels are coloured tags you define **per project**.

### 9.1 Managing labels

Open the **Labels** sheet with the **Labels** button on a board header or on a project's page.

- **Add:** type a **name**, click a **colour** swatch, then **Add** (or press Return).
- **Edit:** click **Edit** on a label; its name and colour load into the form; change them and click **Save** (or **Cancel**).
- **Delete:** click the trash icon → *"Delete label?"* with the number of issues it will be removed from. The issues themselves are unaffected.
- Each row shows the label and **how many issues use it**.

| Rule | Detail |
|---|---|
| Name | 1–30 characters, no control characters. |
| Uniqueness | Unique **within a project**, ignoring upper/lower case ("UI" and "ui" clash). Different projects can reuse names. |
| Colours | Ten fixed colours: gray, red, orange, yellow, green, teal, blue, indigo, purple, pink. |

### 9.2 Using labels

- **Apply** them in the issue editor by clicking the chips (a ticked chip is applied).
- They appear as **chips on cards** (the first three, then "+n").
- **Filter** a board by label with the **Label** filter.
- Label names are **searchable**, and appear in search results as `#name`. They are also included in the RAID Log search and the CSV export.
- An issue can only carry labels from **its own project** (enforced by the database as well as the app).
- When you change the project while creating a new issue, the chosen labels are cleared (they would belong to the wrong project).

### 9.3 Best practices

- **Keep the vocabulary small and stable** (6–12 labels per project). Too many labels stop being useful.
- **Use consistent naming**: lower-case, singular nouns (`security`, `billing`, `ios`).
- Use labels for **cross-cutting themes**: component, customer, release, theme. Don't duplicate what **type**, **status** and **priority** already say.
- **Assign colours by meaning**, for example red for urgent themes, blue for components.
- **Review unused labels** from time to time (the manager shows how many issues use each).

### 9.4 Caveats

- Labels are **not shared between projects**; there is no global label list.
- There is no bulk apply, merge or reorder. Renaming a label renames it everywhere it is used.
- Deleting a label removes it from every issue that had it.

---

## 10. Comments

Every issue type (task, bug, feature, risk, assumption, RAID issue and dependency) can carry **comments** to record progress and updates.

### 10.1 Where to find them

- Open an issue and click the **Comments** tab (it shows the count, for example **Comments (3)**).
- **Right-click** a board card or a RAID Log row → **Add Comment…**.
- Choose a **comment** in the search results; the issue opens on its Comments tab.
- A **💬 number** on cards and RAID Log rows shows how many comments an item has.

A brand-new issue that has not been saved yet shows *"Save the issue first"* on its Comments tab; comments become available once the item exists.

### 10.2 Posting a comment

1. Type in the box (placeholder: *"Add a progress update or note…"*).
2. Choose **Posting as** (**Not specified** or an active developer; it defaults to your **Acting as** developer).
3. Click **Post Comment** or press **⌘↩**.

A character counter (*n / 10,000*) appears when you get close to the limit (9,000 or more) and turns red when you exceed it; the button is disabled while the text is blank or too long.

### 10.3 Reading, editing and deleting

- Each comment shows an avatar, the **author**, the **date and time**, and, if changed, **"edited" with the date**.
- A header above the list shows the count (*"No comments yet"*, *"1 comment"*, *"5 comments"*). When there is more than one comment you can switch between **Newest first** (the default, good for progress updates) and **Oldest first**; the choice is remembered.
- **Edit** turns the comment into an editable box with **Save** and **Cancel**. If you save identical text, it is *not* marked as edited.
- The **trash** icon asks *"Delete this comment?"* and deletes it permanently.

### 10.3.1 Rules for comment text

| Rule | Detail |
|---|---|
| Length | 1–10,000 characters. Counted the way the database counts (Unicode scalars), so text full of emoji or accents can reach the limit sooner than it looks; the counter shows the true figure. |
| Blank comments | Rejected (spaces, tabs and blank lines only count as blank). |
| Line breaks | Kept; Windows/old-Mac line endings are converted to normal new lines. |
| Control characters | Invisible control characters (other than new line and tab) are silently removed. |
| Surrounding spaces | Trimmed. |

### 10.4 Authors

Because there is no sign-in, the author is whoever you pick in **Posting as**.

- The author's **name is stored with the comment**. If the developer is later **renamed**, the new name is shown; if they are **deleted**, the comment keeps the name it was posted under.
- A comment with no author shows as **Unknown**.
- If the developer you chose is removed before you post, the comment is posted without an author.

### 10.5 What else happens when you comment

- A history entry is added to the item (*Added a comment*, *Edited a comment*, *Deleted a comment*).
- The comment counts as the author's **activity** on the dashboard.
- The system log records that a comment was added, edited or deleted, and on which item, but **never the text**.
- Comments are **searchable** (by text or author).
- Deleting an issue (or its project) deletes its comments.
- The RAID Log CSV export includes each item's **comment count** and its **latest comment**.

### 10.6 Best practices

- **Write short, dated-by-the-app updates.** State what changed, what is blocked, and what is next: "Root cause found in the sync job; fix in review; deploy Friday."
- **One idea per comment** so the thread reads like a timeline.
- **Record decisions and reasons** in comments rather than silently editing the description, so the history of the thinking survives.
- **For RAID items,** note review outcomes, new information and any trigger events: "Vendor confirmed delay of two weeks; raising probability to Likely."
- Use the **description for the stable spec** and **comments for the evolving story**.
- **Never paste passwords, keys or personal data** into comments. The live database is stored unencrypted on disk (section 16).

### 10.7 Caveats

- **Plain text only**: no formatting, links are not clickable, no @-mentions, replies/threads or attachments.
- **No edit history**: editing replaces the old text; the previous version is not kept.
- **No permissions**: anyone using the app can edit or delete any comment (deletions are recorded in the system log, without the text).
- Search matches only the **first 300 characters** of each comment (section 13).
- A very long thread loads in full when you open the tab; this is fine for hundreds of comments but you may notice it with many thousands.

---

## 11. RAID tracking

**RAID** stands for **Risks, Assumptions, Issues and Dependencies**: the four kinds of thing that, besides the work itself, decide whether a project succeeds. Backpack treats them as additional **issue types**, so they share the board, drag-and-drop ordering, labels, assignees, due dates, comments, history, search and backups, but each type also has its own fields, and there is a dedicated **RAID Log** for reviewing them across projects.

### 11.1 The four types

| Type | What it records | Example |
|---|---|---|
| **Risk** | Something that *might* happen and would hurt. Scored by **probability × impact**. | "Vendor API may be rate-limited at launch." |
| **Assumption** | Something you are *treating as true* but have not confirmed. Tracked until validated or disproved. | "Users are on the latest OS release." |
| **Issue** (RAID issue) | A problem that *has already happened* and needs resolving. | "Staging environment is unstable." |
| **Dependency** | Something you need from (or owe to) *someone else*. Names a **counterparty** and a **direction**. | "Design tokens from the Brand team." |

> **A note on naming.** The fourth type is simply called **Issue**, matching the RAID acronym, but in Backpack every item is loosely an "issue". To avoid confusion, wherever RAID items are shown next to ordinary work, the category is spelled out separately: the editor title reads **New Issue (RAID)**, its section is headed **RAID details · Issue**, and search results read **RAID · Issue · …**.

### 11.2 Fields by type

| | **Risk** | **Assumption** | **Issue** | **Dependency** |
|---|---|---|---|---|
| **Probability** (1–5) | ✔ | – | – | – |
| **Impact** (1–5) – label | **Impact** | **Impact if wrong** | **Impact** | **Impact if late** |
| **Plan** text – label | **Mitigation plan** | **Validation approach** | **Resolution plan** | **Delivery plan** |
| **Direction** | – | – | – | ✔ *We depend on them* / *They depend on us* |
| **Counterparty** | – | – | – | ✔ required (team, vendor or system), up to 100 characters |
| **Date** – label | **Review by** | **Validate by** | **Resolve by** | **Needed by** |
| **Outcome** when closed | Mitigated · Occurred · Accepted | Validated · Invalidated | Resolved · Accepted | Delivered · Cancelled |

Everything else (title, description, status, priority, assignee, labels, comments) works exactly as for work items. Choosing a RAID type reveals a **RAID details · *Type*** section in the editor containing the fields above.

### 11.3 Scoring risks

**Probability** and **Impact** each use a five-point scale. The editor shows both the number and its meaning (for example "**3 – Possible**").

| Level | Probability | Impact |
|---|---|---|
| 1 | Rare | Negligible |
| 2 | Unlikely | Minor |
| 3 | Possible | Moderate |
| 4 | Likely | Major |
| 5 | Almost certain | Severe |

The **risk score** is **probability × impact** (from 1 to 25), shown live in the editor and as a **score chip** (for example "**16 · High**") on cards and in the RAID Log. Ratings use fixed bands:

| Score | Rating | Colour |
|---|---|---|
| 1–4 | **Low** | green |
| 5–9 | **Medium** | yellow |
| 10–16 | **High** | orange |
| 17–25 | **Critical** | red |

The score grid (rows are probability, columns are impact; **L**ow, **M**edium, **H**igh, **C**ritical):

| Probability ↓ / Impact → | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|
| **5** | 5 M | 10 H | 15 H | 20 C | 25 C |
| **4** | 4 L | 8 M | 12 H | 16 H | 20 C |
| **3** | 3 L | 6 M | 9 M | 12 H | 15 H |
| **2** | 2 L | 4 L | 6 M | 8 M | 10 H |
| **1** | 1 L | 2 L | 3 L | 4 L | 5 M |

New risks start at **3 × 3 = 9 (Medium)** so that you consciously adjust them. A risk's **priority** (Critical/High/Medium/Low) is set separately and is **not** derived from its score; use priority for "how urgently must we act", and the score for "how big is the exposure".

### 11.4 Outcomes: how a RAID item ended

When a RAID item's status is **Done**, an **Outcome** picker appears in the editor: choose one of the outcomes for that type, or leave **Not recorded**.

- **Risks:** *Mitigated* (we dealt with it), *Occurred* (it happened; consider raising a RAID **Issue**), *Accepted* (we chose to live with it).
- **Assumptions:** *Validated* (confirmed true), *Invalidated* (proved false; usually means a new risk or issue).
- **Issues:** *Resolved*, or *Accepted* (we will live with it).
- **Dependencies:** *Delivered*, or *Cancelled*.

Rules to know:

- **Outcome is optional.** Dragging a RAID item into **Done** on the board closes it *without* inventing an outcome; the card then shows **"No outcome recorded"** and the RAID Log counts it under **Closed, no outcome** so you can fill it in.
- **Any status change clears the outcome** (reopening an item removes its outcome; closing one starts without it). Re-ranking an item that stays in Done keeps it.
- **Changing the type** removes an outcome that no longer fits (a "Validated" outcome cannot stay on a risk).
- The **invalidated assumptions** count on the dashboard comes from these outcomes.

### 11.5 Creating and editing RAID items

- From a **board**: **New Issue** (or a column **+**) and set **Type** to Risk, Assumption, Issue or Dependency.
- From the **RAID Log**: the **New** menu offers the four types directly (see Section 11.6).
- Existing items can be **re-typed** at any time (see Section 11.9).

On the board, RAID cards carry a coloured **type tag**, the risk **score chip** or the **impact**, a dependency's **counterparty** with an arrow (down-left for inbound, up-right for outbound), and, once closed, the **outcome**.

### 11.6 The RAID Log

Choose **RAID Log** in the sidebar. It lists every RAID item, across projects, and is designed for **review meetings**.

**Header and filters**

| Control | What it does |
|---|---|
| **Project** | *All active projects* (the default; archived projects are excluded) or a single project (archived ones are listed as "*name* (archived)"). |
| **New** | Menu of the four RAID types. Items are created in the chosen project, or in the only active project. The menu is **disabled** when it cannot tell which project (several projects and *All active projects* chosen, or an archived project chosen). Hover for the reason. |
| **Export CSV** | Writes the list currently shown (section 11.7). Disabled when nothing is shown. |
| **Type** (segmented) | *All*, *Risks*, *Assumptions*, *Issues*, *Dependencies*. |
| **Open only** | On by default: hides Done items. Switch off to include closed items. |
| **Sort** | **Exposure** (highest risk score first, non-risks after; ties by priority), **Due date** (soonest first; undated last), **Newest** (most recently created first), **Type** (Risks, Assumptions, Issues, Dependencies). |
| **Search the RAID log** | The same fuzzy search as global search, limited to RAID items. It looks at the key, title, description, plan, counterparty, type name, assignee and labels. |

On narrow windows the controls **wrap onto a second line** instead of overlapping.

**Left panel**

- **Risk heat map.** A 5 × 5 grid of **open risks** (probability up the side, impact across the bottom). Each square is coloured by its rating and shows how many open risks sit there. **Click a square** to filter the list to those risks (a note appears with **Clear**); click it again to remove the filter.
- **Open items.** The number of open Risks, Assumptions, Issues and Dependencies in scope.
- **Risk exposure.** The sum of the scores of all open risks: a single number to watch trend over time.
- **Past their date.** Open RAID items whose date has passed.
- **Closed, no outcome.** Appears only when there are closed RAID items with no outcome recorded.

The heat map and the three counts respond to the **Project** choice only, not to the Type, Open only or Search controls.

**The list**

Each row shows the type icon, the title, a subtitle (`KEY-n · Type`, for dependencies `· from/for Counterparty`, and the impact in words), and on the right the risk rating chip, the status, the outcome (or **No outcome** for closed items), the comment count, the assignee, and the date (red if overdue). **Click** a row to open it; **right-click** for **Edit…** or **Add Comment…**.

### 11.7 Exporting the RAID log to CSV

**Export CSV** saves exactly what the list is showing (so filter first). You choose the location in a save dialog; the suggested name is `raid-log.csv`. Columns:

`Project, Key, Type, Title, Status, Priority, Owner, Probability, Impact, Score, Rating, Counterparty, Direction, Response, Outcome, Due, Labels, Created, Updated, Comments, Latest comment`

Details:
- **Probability, Impact, Score** are numbers; **Rating** is Low/Medium/High/Critical (risks only).
- **Direction** is *Inbound* or *Outbound*; **Response** is the plan text; **Labels** are separated by "; ".
- **Due** is a date (`2026-11-30`); **Created** and **Updated** are full ISO-8601 timestamps.
- **Comments** is the number of comments; **Latest comment** is "*date · author: text*", cut to 500 characters.
- The file is UTF-8 with a byte-order mark (so Excel shows accents correctly) and uses CRLF line endings.
- **Spreadsheet safety:** any text starting with `=`, `+`, `-`, `@`, a tab or a carriage return is prefixed with an apostrophe so a spreadsheet will not run it as a formula. You may therefore see a leading `'` in such cells; this is intentional.
- Each export is recorded in the system log (how many items, not their contents).

### 11.8 RAID on the dashboard

If a project has any RAID items, its dashboard shows a **RAID Summary**: open **Risks** (with how many are High or Critical and the total exposure), **Assumptions** to validate (and how many were proved wrong), open **Issues**, open **Dependencies**, a line saying how many RAID items are **past their date**, and an **Open RAID Log** button.

RAID items are **excluded from the delivery numbers** (completion rate, turnaround, throughput, status chart and the Overdue card). Closing a risk is not "delivering work" and would distort those figures.

### 11.9 Changing an item's type

You can re-type any item. When you save:

- fields the new type does not use are **cleared** (for example, changing a Dependency to a Task deletes its counterparty and direction; changing a Risk to an Assumption deletes its probability);
- an outcome that does not suit the new type is removed;
- the change is recorded in the item's history ("Type: Task → Risk"), along with the risk score or other RAID fields that changed.

Until you press **Save**, you can switch back and nothing is lost.

### 11.10 Best practices

**Run a regular RAID review** (weekly or fortnightly) from the **RAID Log**:
1. Sort by **Exposure**; read the top risks aloud. Is the score still right? Is the mitigation working? Update probability and impact; the history records every change.
2. Switch **Sort** to **Due date** and look at anything **past its date**.
3. Look at **Dependencies**: has each counterparty confirmed? Add a comment with their latest answer.
4. Check **Assumptions**: who is validating each, and by when (**Validate by**)? Close them as *Validated* or *Invalidated*.
5. Clear **Closed, no outcome** by recording how each item ended.

**Quality tips**

- **Write risks as a cause and effect:** "Because the vendor has a quota of N calls/min, we may be throttled at launch, delaying go-live."
- **Score honestly,** and re-score when circumstances change rather than editing the history away. Use the **Mitigation plan** for what you will *do*; use **comments** for what has *happened*.
- **Give every RAID item an owner** (Assignee) and a **review/needed-by date**; items without either are rarely looked at.
- **Always name the counterparty and direction** of a dependency, for dependencies in *both* directions: what you need and what others need from you.
- **Convert, don't overwrite.** When a risk *occurs*, close it as **Occurred** and create a new **Issue** (mention the risk's key in its description or a comment). When an assumption is **Invalidated**, create the risk or issue it implies. Backpack does not link items automatically, so cross-reference by key.
- **Use labels** (for example `security`, `vendor`, `release-1`) to slice the RAID Log by theme.
- **Record outcomes** for closed items; they are the raw material for lessons learned.

### 11.11 Caveats

- RAID items cannot be **linked** to the tasks they affect or to one another; cross-reference using issue keys in text.
- There are **no automatic escalations, reminders or conversions** (risk → issue).
- The **5 × 5 scales and rating bands are fixed**; they cannot be customised.
- Dragging a RAID item to Done leaves the outcome unrecorded (by design); record it in the editor.
- The heat map shows **open risks only** and only those with both probability and impact.
- A risk's **priority** is independent of its score; keep them consistent yourself.
- The RAID Log and CSV export cover **RAID items only**; there is no CSV export for work items.

---

## 12. The project dashboard and its metrics

The dashboard sits under the project's details on the **Projects** screen. It refreshes by itself whenever data changes. The heading reads **Project Dashboard** with a note, *"Engagement window: last 14 days"*.

### 12.1 The four summary cards

| Card | What it shows | How it is calculated |
|---|---|---|
| **Completion Rate** | A percentage and a progress bar; "*x of y issues done*". | **Done work items ÷ all work items** in the project. |
| **Avg. Turnaround** | The typical time from creation to completion; "*Created → done, n issue(s)*". | The **average** of (completed time − created time) over work items that are Done and have a completed time. Shown in minutes, hours (under 48 h) or days. "—" when nothing is complete. |
| **Active Developers** | "*x of y*" and a progress bar; "*active in last 14 days*". | *y* = active developers who have **open assigned items** in this project *or* any **history activity in the last 14 days**; *x* = those of them with **activity in the last 14 days**. |
| **Overdue** | A count (red when above zero); "*n open issue(s)*". | **Open work items** whose due date has passed. "Open issues" is work items that are not Done. |

### 12.2 RAID Summary

Appears only when the project has RAID items; see section 11.8.

### 12.3 Charts

- **Issues by Status** – a horizontal bar per status (To Do, In Progress, In Review, Done) with counts. Work items only.
- **Completed per Week** – a bar for each of the **last six weeks**, counting work items that were completed that week. Weeks follow your Mac's calendar settings.

### 12.4 Developer Engagement

A table with one row per developer who is **active** and either has **open assigned items** or **history activity in the last 14 days**:

| Column | Meaning |
|---|---|
| **Developer** | The name. |
| **Activity (14d)** | Number of history entries credited to them in the last 14 days (status moves, edits, assignments, comments, and so on). |
| **Open** | Items assigned to them that are not Done (**work and RAID**). |
| **Done** | Items assigned to them that are Done (**work and RAID**). |
| **Status** | **Active** (some activity in the window) or **No recent activity** (they have open work but have done nothing recorded in 14 days: a prompt to check in, not a verdict). |

Rows are sorted by activity, highest first.

### 12.5 Interpreting the numbers (and their limits)

- **Delivery numbers are for work items only.** RAID items are summarised separately.
- **"Activity" depends on attribution.** An action is credited to your **Acting as** developer, or if that is left on *Assignee*, to the item's assignee. If one person updates everyone's items without changing "Acting as", the numbers describe the *assignees*, not the person at the keyboard.
- **Small samples mislead.** An average turnaround over three items says little; a completion rate for a new project is noisy.
- **Reopening resets the clock.** Moving an item out of Done discards its completed time; moving it back gives a new one, so turnaround is measured to the *latest* completion.
- **Due-date and overdue figures** have the time-of-day caveat in section 7.7.
- **Deleting a developer** removes their past activity from the dashboard (their history entries lose the link to them).
- **Turnaround measures elapsed calendar time,** including time spent waiting in To Do; it is not "hands-on" effort.

### 12.6 Best practices

- Use **Completion Rate** and **Completed per Week** to see trend, not for comparing people.
- Treat **No recent activity** as a conversation starter.
- Watch **Overdue** together with the **RAID Summary's** "past their date".
- Review the dashboard at the same time each week so changes are noticeable.

---

## 13. Global search

### 13.1 Using search

Click the search bar in the title bar (or press **⌘K**) and start typing. Results appear in a panel under the bar after a very short pause, grouped as **PROJECTS**, **ISSUES**, **COMMENTS** and **LOGS**.

| Action | Result |
|---|---|
| **↓ / ↑** | Move the highlight. |
| **Return** (or click) | Open the highlighted result. |
| **Esc**, the **✕** button, or a click outside the panel | Close and clear. |

What happens when you open a result:

| Result type | Opens |
|---|---|
| **Project** | The **Projects** screen with that project selected. |
| **Issue** | The project's **board**, with that issue's editor open. |
| **Comment** | The project's board with the issue open on its **Comments** tab. |
| **Log entry** | The **Logs** screen, centred on that entry (see Section 14.2). |

### 13.2 What is searched

| Kind | Fields searched (higher weight = counts more) |
|---|---|
| **Projects** | name (high), key (high), description. |
| **Issues** | key such as `ATL-12` (high), title (high), description, assignee, labels. **RAID items** additionally: the words "RAID *Type*", counterparty (high), and the plan text. |
| **Comments** | the comment text, and (less) the author's name. The newest 5,000 comments are searched. |
| **Logs** | the message and category. The newest 5,000 live log entries are searched. |

Issue results show their status, project, assignee and `#labels`; RAID items start with **RAID · *Type***.

### 13.3 How the fuzzy matching works

You do not need to type exactly. Backpack is forgiving in four ways:

1. **Several words, any order.** Every word you type must match *something* on the item (they can match different fields). `vendor risk` finds a risk titled "Vendor delay" as well as a comment mentioning the vendor on a risk.
2. **Exact pieces beat guesses.** Text that contains what you typed ranks highest, especially at the start of the text or of a word.
3. **Initials and gaps.** If your letters appear in order and reasonably close together, it matches: `dbmig` finds "Database migration tooling"; `flw` finds "flow". Letters scattered far apart do not count.
4. **Typos.** Words of four letters or more tolerate a slip: one wrong, missing, extra or swapped letter (two for words of eight letters or more): `databse` finds "database".

Case is ignored. Within each group, results are ranked best-first.

### 13.4 Examples

| You type | Finds |
|---|---|
| `ATL-12` | Issue 12 of project ATL. |
| `raid` | Every RAID item (the type name is searchable). |
| `raid issue` | RAID issues (and anything else containing both words). |
| `ava` | Items assigned to Ava, comments written by Ava, and any text containing "ava". |
| `security` | Items labelled *security* (label names are searched; results display them as `#security`). |
| `rolback` | Items or comments mentioning "rollback" (typo tolerated). |
| `vendor delay` | Items where both words appear. |
| `backup` | Log entries about backups, plus anything else containing "backup". |

### 13.5 Limits and caveats

- Only the **first 300 characters** of each field are matched; words that appear only later in a long description or comment will not be found.
- Results are **capped per group**: 6 projects, 20 issues, 10 comments, 12 log entries. Type more to narrow.
- **Rotated** log entries (in log archives) are not searched, only the live log.
- There are **no operators**, quoted phrases or field filters; one- or two-letter queries are matched very loosely and return many results.
- Searching does **not** look in item *history*, developer names as such, or label lists on their own; labels and developers are found through the items they belong to.
- Archived projects' items are still found.

---

## 14. Activity history, the system log and log rotation

Backpack keeps **two separate records** of what happens. Knowing the difference avoids confusion.

| | **Item history** | **System log** |
|---|---|---|
| **Answers** | "What changed on *this issue*, and who did it?" | "What did the *application* do, and when?" |
| **Where** | Editor → Details tab → **History** | Sidebar → **Logs** |
| **Content** | Status/priority/type/assignee changes, text edits, label and RAID changes, comments added/edited/deleted. | Creating/updating/deleting projects, issues, labels, developers and comments; backups and restores; security audits; log rotation; exports; errors. |
| **Detail** | Shows what changed (e.g. "Status: To Do → Done"). | Says *that* something happened (e.g. "Updated ATL-12"), not the full content. |
| **Protection** | Normal data; deleted with its issue. | Tamper-evident hash chain, append-only, survives deletion of issues and projects. |

### 14.1 Item history

Described in section 7.8. It is capped to the latest 100 entries per item in the editor; older entries remain stored but are not shown. Due-date changes are not recorded.

### 14.2 The Logs screen

Choose **Logs** in the sidebar.

- **Level** menu: *All levels*, *Debug*, *Info*, *Warning*, *Error*, *Audit*.
- **Filter text or category** box: matches (ignoring case) anywhere in the message or the category.
- **Live log *X* / *N* MB**: the current size of the live log against your rotation limit (section 14.6).
- **Verify Integrity**: checks the tamper-evident chain (section 14.4).
- The list shows the **time**, a coloured **level** tag, the **category** and the **message**, newest first, up to **1,000 matching entries**. Message text can be selected and copied.
- When you arrive from a **search result**, the screen shows a window of about 400 entries around that entry, highlights it and scrolls to it. A **Show latest** button returns to the normal view.

### 14.3 Levels and categories

| Level | Meaning |
|---|---|
| **Debug** | Developer detail (rarely used). |
| **Info** | Normal operation: created, updated, moved, started, terminated. |
| **Warning** | Something unexpected but not fatal (for example, a cleanup step failed). |
| **Error** | An operation failed (also summarised in the security audit's *Recent errors* check). |
| **Audit** | Security-relevant or destructive actions: deletions, backups, restores, permission repairs, security audits, log rotation, exports, archive deletion. |

Common categories: `app`, `project`, `developer`, `issue`, `label`, `comment`, `raid`, `backup`, `security`, `logs`, `database`, `store`, `metrics`, `sample`.

**What the log never contains:** comment text, descriptions or plan text. Messages are single-line, stripped of control characters and capped at 1,000 characters (so nobody can forge a log line by pasting a line break). Messages are also written to macOS's unified log (visible in **Console.app**), with their content marked private.

### 14.4 Tamper-evidence and "Verify Integrity"

Each log entry stores a **fingerprint (SHA-256)** that depends on the entry itself *and the fingerprint of the entry before it*, forming a chain. The database also refuses ordinary attempts to change or delete log rows. Click **Verify Integrity** to re-compute the chain:

- **"✓ *n* entries verified"**: nothing has been altered.
- **"⚠︎ Chain broken at #*id*"**: the entry with that number (or an earlier edit) does not match; the log may have been altered or damaged. Check **Security & Backups → Run Security Audit** and consider restoring a backup.

What this protects against, and what it does not:

| It will detect | It will not detect |
|---|---|
| Editing or deleting an entry in the middle of the log. | Someone deleting the *newest* entries from the end of the log. |
| Removing entries from the start of the live log. | A determined person with full access to the files who rebuilds the entire chain. |
| Tampering with a rotated archive. | Anything that happened before logging was switched on (nothing is lost; logging is always on). |

The log is **evidence, not a lock**: its value is that tampering becomes visible.

### 14.5 Why logs need rotation

The log only ever grows. A busy installation can accumulate many megabytes, which slows backups and checks. **Log rotation** moves old entries into an encrypted archive and starts a fresh log, without breaking the chain.

### 14.6 Log rotation: settings and how it works

Open **Security & Backups** and scroll to **Log Rotation**.

| Control | Meaning | Default |
|---|---|---|
| **Live log bar** | "*Live log: X of Y MB · n entries*" with a progress bar (turns orange when over the limit) and a refresh button. | – |
| **Rotate automatically when the live log exceeds the limit** | Master switch. | **On** |
| **Rotate above** | Size limit: 1, 5, 10, 25, 50 or 100 MB. | **10 MB** |
| **Keep last** | How many archive files to keep: 3, 5, 10, 20 or 50. | **5** |
| **Rotate Now…** | Rotates immediately, after confirmation. Disabled when the live log is empty. | – |

**When it runs automatically:** at launch, then every hour while the app is open, and immediately after you turn it on or change the limit.

**What a rotation does**
1. Reads every live entry and **checks the chain** (if it was already broken, that is noted in the archive record and the log).
2. **Compresses and encrypts** them into a `.bplog` file in the `LogArchives` folder (using the same Keychain key as backups).
3. **Empties the live log** and starts a new chain whose first entry **links to the archive's final fingerprint**, so continuity can still be proved.
4. Records the rotation permanently (number of entries, date range, file fingerprint). These records cannot be edited or deleted.
5. **Compacts the database** to give the space back, then writes an **Audit** entry ("Log rotated…").
6. **Deletes archives beyond your *Keep last* number** (oldest first). The *record* of a deleted archive remains; only its file is removed.

If any step fails, nothing is changed: the archive file is removed and the live log is left intact.

### 14.7 Managing archives

Each archive is listed with its date, the range of entry numbers and dates it holds, the number of rows and its size. Buttons:

| Button | Does |
|---|---|
| **Verify** | Decrypts the archive and checks that it is intact, matches its recorded fingerprint, contains the expected number of entries, forms an unbroken chain and links to the previous archive. Shows ✔ or ✖ with a reason. |
| **Export…** | Saves the **decrypted** entries as a JSON-lines file you choose (for reading, searching or audit). **The exported file is plain, unencrypted text**: store it carefully. |
| **Reveal** | Shows the encrypted file in Finder. |
| **Trash** | Deletes the archive file after confirmation (the rotation record stays). |

An archive whose file has been removed (by the retention limit or by you) shows **"file removed"** and can no longer be verified or exported.

### 14.8 Best practices

- **Leave automatic rotation on** with the defaults (10 MB, keep 5) unless you have a reason to change them. Raise **Keep last** if you must retain a longer history.
- **Verify the live chain occasionally** (monthly) with **Verify Integrity**, and verify an archive after a rotation you care about.
- **Back up the `LogArchives` folder** if the history matters to you; archives are *not* part of a database backup.
- Use **Export…** deliberately and delete the plain-text export when finished.

### 14.9 Caveats

- **Search covers only the live log.** Older entries are in archives; export one to read it.
- Rotation briefly **holds the database** (about a second for 10 MB); you may notice a short pause.
- Archives are encrypted with the **Keychain key of this Mac**: lose the key and they cannot be read (section 15.5).
- **Restoring an older backup** brings back that backup's log, so archives created *after* it will still be on disk but will not be listed.
- Entries removed by retention are gone for good; the audit trail keeps only the fact that a rotation happened and the archive's fingerprint.

---

## 15. Backups and restore

### 15.1 What a backup contains

A backup is an **encrypted snapshot of the entire database**: projects, issues, comments, labels, developers, history, the live system log and rotation records. It does **not** include the `LogArchives` folder or the `Backups` folder itself.

- Files are named `backup-YYYYMMDD-HHMMSS-mmm.bpbak` and stored in the `Backups` folder of the data folder.
- They are encrypted with **AES-256-GCM** (confidential and tamper-evident) using a key held in your **macOS Keychain**.
- The folder and files are readable only by you (permissions 700 / 600).

### 15.2 When backups are made

| Trigger | Details |
|---|---|
| **Manual** | **Back Up Now** (in Security & Backups) or **⇧⌘B**. |
| **Daily, automatic** | At launch, and then checked hourly while the app is open: a backup is made if the newest one is **older than 24 hours**. |
| **When you quit** | If automatic backups are on and the newest backup is **older than one hour**. |
| **Before an upgrade** | When a new version of the app needs to change an existing database, an encrypted backup is made first, then the upgrade runs. |
| **Before a restore** | A **"pre-restore safety copy"** of the current data is made automatically. |

The **Automatic daily backup** switch controls the daily and quit backups only; manual, pre-upgrade and pre-restore backups always happen.

### 15.3 The Backups section

- **Back Up Now**: make one immediately.
- **Automatic daily backup**: on/off (default **on**).
- **Keep *n***: how many backups to keep, 1–90 (default **14**). After each new backup the oldest beyond this number are deleted. Pre-upgrade and pre-restore copies count towards the total.
- **Show in Finder**: opens the `Backups` folder.
- **List**: each backup's date/time, file name and size on disk, with **Restore…** and a **trash** icon.

### 15.4 Restoring

1. Go to **Security & Backups → Backups** and click **Restore…** beside the backup you want.
2. Read the confirmation, *"The current database will be replaced with the backup from …. A safety backup of the current state is taken first."*, and click **Restore**.

What Backpack does: decrypts the file → checks it is intact and not from a *newer* version of the app → takes a **safety backup** of what you have now → replaces the database → upgrades the restored data to the current structure if it was older → reloads the screens. The action is recorded in the system log.

**Consequences to understand**
- **Everything created or changed after the backup is gone** from the live data (it remains in the safety backup, which you can restore to undo the restore).
- The live **system log** is also replaced by the backup's version.
- A restore **cannot be done while a restore is running**; wait for the progress indicator to finish.
- If the file is damaged, tampered with, or encrypted with a different key, the restore **fails with a message and changes nothing**.

### 15.5 The encryption key: the most important caveat

The key is created automatically the first time you make a backup, and stored in your **login Keychain** as *"com.example.Backpack.backup"*, marked **this device only**. macOS may ask you to allow access; choose **Always Allow** so automatic backups can run unattended.

- **Backups and log archives can only be decrypted on this Mac, with this key.** If you erase the Mac, create a new user, delete the Keychain item or move to another Mac, **existing backups and archives become unreadable**.
- There is **no key export** feature.
- Therefore, **backups protect you from mistakes and corruption, not from losing the Mac or its Keychain.** To protect against that, also keep a copy of the *live data folder* (which is not encrypted) in your normal backup system (see Section 15.6 and 17.5).

### 15.6 Best practices

- **Keep automatic backups on,** and keep **Keep n** at 14 or more.
- **Back up manually before** deleting a project, restoring, doing a big clean-up, or upgrading the app.
- **Include the data folder in Time Machine** (or your usual backup). It gives you an off-device copy of the live database *and* the backup files.
- **Check freshness:** *Run Security Audit* warns if the newest backup is older than 48 hours or none exists.
- **Rehearse a restore** at least once on a copy of your data (for example in a separate data folder; see Section 17.4) so you know it works before you need it.
- Treat the `Backups` folder as **confidential**: although encrypted, it is a complete copy of your data.

### 15.7 Caveats

- The very first backup creates the Keychain key and may prompt for permission.
- A backup is a point-in-time snapshot; there is no continuous protection between backups.
- Backups cannot be opened outside Backpack (no manual extraction).
- Because backups are encrypted per Mac, copying them to another Mac does not make them restorable there.

---

## 16. Security and privacy

### 16.1 The big picture

- **Local only.** Backpack makes no network connections, has no accounts and sends no telemetry.
- **No sign-in.** Anyone who can open the app on your Mac can read and change everything. Protect the Mac itself (login password, screen lock, **FileVault**).
- **The live database is not encrypted** on disk. It is protected by macOS file permissions (your account only) and, in sandboxed builds, by the App Sandbox. **Backups and log archives are encrypted.**

### 16.2 What the application does for you

| Protection | How |
|---|---|
| **File permissions** | The data folder and subfolders are accessible to you only (700); files are 600. **Repair Permissions** re-applies this. |
| **Safe handling of input** | All database access uses bound parameters (no injection); all text is validated and length-limited; invisible control characters are stripped from comments and log messages. |
| **Integrity** | Foreign keys are enforced; the database refuses invalid combinations (for example a RAID outcome on an open item, or a label from another project). |
| **Secure delete** | Deleted content is overwritten rather than left in free space. |
| **Tamper-evident log** | Section 14.4. |
| **Encrypted backups and archives** | Section 15.1 and 14.6. |
| **Spreadsheet-safe export** | CSV cells that begin with formula characters are neutralised. |
| **App Sandbox** (Xcode builds) | The app can only touch its own container and files you pick in a save dialog. |

### 16.3 The Security Audit

Open **Security & Backups** and click **Run Security Audit**. Findings are sorted worst-first, each with an icon: ✔ **Pass**, ℹ **Info**, ⚠ **Warning**, ✖ **Critical**. A summary shows **All checks passed** or **Review findings** (Info items count as "review").

| Check | What it tests | If it's not a pass |
|---|---|---|
| **Data directory / Database file / Backup directory / Log archive directory permissions** | Only your account can access them. | **Warning** – click **Repair Permissions**. |
| **SQLite integrity check** | The database file is structurally sound. | **Critical** – restore a recent backup. |
| **Foreign-key consistency** | No record points at something that no longer exists. | **Critical** – restore a backup. |
| **Foreign keys enforced / Secure delete / Journal mode** | The safety settings of the database are on. | Warning/Info – report if persistent. |
| **Audit log chain** | The live log's chain is unbroken. | **Critical** – see Section 14.4. |
| **Log rotation** | Rotation is on and the live log is not far over the limit (Warning when over **twice** the limit with rotation on, or over the limit with rotation off). | Turn rotation on or rotate now. |
| **Log archives** | No archive was made from an already-broken log. Reports counts. | **Critical** if any. |
| **Backup freshness** | A backup exists and the newest is under **48 hours** old. | **Warning** – back up now. |
| **Plaintext leftovers** | No unencrypted snapshot is lying in the backup folder. | **Warning** – investigate and delete the stray file(s). |
| **Backup encryption key** | The Keychain key exists. | **Info** before your first backup. |
| **Recent errors** | No *Error*-level log entries in the last 24 hours. | **Warning** – read them in **Logs** (filter by Error). |
| **App Sandbox** | Whether the app is sandboxed. | **Info** for builds run outside Xcode (normal for `swift run`). |

Each audit is itself recorded in the system log.

### 16.4 What Backpack does *not* protect you from

- Someone using your logged-in Mac.
- Malware running as you, or anyone with administrator access to the machine.
- Deliberate deletion by someone who can use the app (deletions are logged, but there is no per-user permission).
- Loss of the Keychain key (backups become unreadable).
- Truncation of the *end* of the log (section 14.4).

### 16.5 Best practices

- Turn on **FileVault**, use a strong login password and lock the screen.
- **Do not store passwords, API keys or personal data** in titles, descriptions or comments.
- Run **Security Audit** monthly and after any unexpected behaviour.
- Treat **plaintext exports** (CSV, log archive export) as sensitive; delete them when done.
- Keep macOS up to date.
- If you distribute the app to colleagues, build it with the **App Sandbox** and sign it with your own developer team.

---

## 17. Where your data lives, settings, and the database

### 17.1 The data folder

| Item | What it is |
|---|---|
| `backpack.sqlite` | Your database: everything except backups and log archives. You may also see `backpack.sqlite-wal` and `backpack.sqlite-shm`; these are normal SQLite working files. **Do not delete them while the app is running**, and keep them together with `backpack.sqlite` when copying. |
| `Backups/` | Encrypted backups (`backup-….bpbak`). |
| `LogArchives/` | Encrypted rotated-log archives (`logs-…-<first>-<last>.bplog`). |


### 17.2 Settings

Backpack has no Preferences window; settings live in the screens where they apply and are stored in your macOS preferences for the app.

| Setting | Where | Default |
|---|---|---|
| Automatic daily backup | Security & Backups → Backups | On |
| Backups to keep | Security & Backups → Backups (**Keep *n***) | 14 (range 1–90) |
| Automatic log rotation | Security & Backups → Log Rotation | On |
| Rotate above | Same | 10 MB |
| Archives to keep | Same (**Keep last**) | 5 |
| Board grouping | Board header (**Group by**) | Status |
| Comment order | Comments tab | Newest first |

### 17.3 Starting over

To wipe everything: **quit Backpack**, delete its data folder, and relaunch. You will get an empty database (and the *Load Sample Data* option). **This permanently deletes all data and backups in that folder.** The Keychain key remains and will be reused.

### 17.4 Using a separate data folder

For rehearsing a restore or trying things out, set the environment variable **`BACKPACK_DATA_DIR`** to a folder before launching (for example, `BACKPACK_DATA_DIR=~/BackpackTest swift run`, or add it to the scheme's environment in Xcode). Backpack then uses that folder instead. This works with builds that are **not sandboxed**; a sandboxed build may be refused access to folders outside its container (it will then report that it cannot open its database).

### 17.5 Moving to another Mac

1. **Quit** Backpack on the old Mac.
2. Copy the **entire data folder** (including any `-wal`/`-shm` files) to the same location on the new Mac, with the same kind of build.
3. Launch Backpack on the new Mac.

Your data comes across because the live database is not encrypted. **Backups and log archives will not open** on the new Mac (different Keychain key); new backups will be encrypted with the new Mac's key. Keep the old Mac's backups until you are satisfied.

### 17.6 The database

Backpack uses **SQLite** with these tables:

| Table | Holds |
|---|---|
| `projects` | Projects (name, unique key, description, archived flag). |
| `developers` | The people list (unique name, email, active flag). |
| `issues` | All issue types, with the RAID columns (probability, impact, response, counterparty, direction, outcome). |
| `activity` | Item history and developer activity. |
| `labels`, `issue_labels` | Per-project labels and the link between issues and labels. |
| `comments` | Comments on issues. |
| `logs` | The live, hash-chained system log. |
| `log_archives` | One permanent record per log rotation. |


A database created by a **newer** version cannot be opened by an older app: it reports *"Database was created by a newer version of the app"* and quits, leaving your data untouched.

**Reading the database yourself.** You can open a **copy** of `backpack.sqlite` with any SQLite tool to explore or report. **Never edit the live file while the app is running**: changes made outside Backpack bypass its validation, history and logging, and can corrupt it.

---

## 18. Recommended ways of working

These are suggestions, not requirements; adapt them to your team.

### 18.1 Setting up a new project

1. **Create the project** with a short, permanent key and a description of scope and purpose.
2. **Add the people** (Developers screen) and decide who will be **Acting as** whom.
3. **Define 6–12 labels** that reflect how you will want to slice the work (component, customer, release).
4. **Agree your definitions** and write them in the project description:
   - what each **priority** means;
   - what **Done** means (merged? deployed? accepted?);
   - how you will **score risks** (what does a "4" for impact mean in money or days?).
5. **Seed the RAID log** early: capture the known risks, the assumptions you are making and the dependencies you already know about.
6. **Make a backup** once the structure is in place.

### 18.2 A personal routine (one person)

| When | Do |
|---|---|
| **Start of day** | Open the board grouped by **Priority**. Re-rank so the first card in each column is what you will do next. Move one or two cards to **In Progress**. |
| **During the day** | Post a **comment** when something changes (blocked, decision made, found the cause). Drag cards as their status changes. |
| **End of day** | Move finished work to **In Review/Done**. Glance at **Overdue**. Let the app quit normally so it can take its daily backup. |
| **Weekly** | Review the **dashboard** and the **RAID Log**; tidy labels; check **Completed per Week**. |

### 18.3 A small team sharing one Mac

- Each person sets **Acting as** to themselves when they sit down.
- Use the **Assignee filter** for one-to-one check-ins and the **Type filter** to switch between "work" and "RAID" conversations.
- Hold a short **weekly review** around the RAID Log (see Section 11.10) and the dashboard's **Developer Engagement** table (a prompt for conversation).
- Because there is no sign-in or per-person permissions, agree **norms**: do not edit others' comments; always add a comment when you change someone else's item.

### 18.4 Two ways to look at the same board

| View | Use it to… |
|---|---|
| **Group by Status** | See **flow**: what is queued, in progress, waiting for review, finished. Limit work in progress. |
| **Group by Priority** | **Triage**: decide what is most important, regardless of status. Drag between priorities to re-prioritise; drag within a column to order. |

The order within a column is shared, so a re-ranking you make in one view stays meaningful in the other.

### 18.5 Preparing for a release or milestone

1. Filter the board by **RAID items**; confirm no unowned items.
2. Open the **RAID Log**, sort by **Exposure**: do all Critical and High risks have a plan, an owner and a review date?
3. Sort by **Due date**: are any **dependencies** past their *Needed by* date?
4. Check **assumptions** still open: validate or convert them into risks.
5. **Export CSV** of the RAID log to share with stakeholders.
6. Run the **Security Audit** and make a **manual backup** before and after the release.

### 18.6 Handling bugs

- **Title:** the symptom, not the guess ("Crash when opening an empty project").
- **Description:** steps to reproduce, expected vs actual result, environment.
- **Priority:** use your agreed definitions; reserve **Critical** for release blockers.
- **Comments:** log investigation notes and the eventual cause.
- Move to **In Review** when a fix exists and to **Done** only when verified.
- Use a label such as `regression` or `customer-reported` for reporting.

### 18.7 Learning from the past

- Close RAID items with an **outcome** so you can later count how many risks actually *occurred* and how many assumptions proved *invalidated*.
- Use **Completed per Week** and **Avg. Turnaround** to set realistic expectations, remembering their limits (11.5).

### 18.8 Housekeeping calendar

| Frequency | Task |
|---|---|
| **Daily** | Let backups run (quit the app at the end of the day, or leave **Automatic daily backup** on). |
| **Weekly** | Review overdue items, the Done column, the RAID Log and "Closed, no outcome". Prune stale labels. |
| **Monthly** | **Run Security Audit**; **Verify Integrity** on the log; confirm Backups exist and are recent; check the free disk space where the data folder lives. |
| **Quarterly** | Archive finished projects; review developers (deactivate leavers); **rehearse a restore** on a copy; review log rotation settings. |
| **Before risky actions** | **Back Up Now** (deleting a project, restoring, upgrading the app, bulk clean-up). |

### 18.9 Naming conventions that help

- **Project keys:** 2–5 letters, upper case, meaningful.
- **Titles:** verb-first for work ("Add…", "Fix…", "Remove…"); cause-and-effect for risks; a clear statement for assumptions ("We assume…"); the thing needed and from whom for dependencies.
- **Labels:** lower-case, singular, no spaces if possible.
- **Developers:** full, distinct names (avatars show initials).

---

## 19. Best-practice checklist

**Setup**
- [ ] Short, permanent project keys; a description of scope.
- [ ] Developers added (real names); *Acting as* set per person.
- [ ] 6–12 labels per project, consistently named.
- [ ] Definitions of priorities, Done and risk scoring written down.

**Daily use**
- [ ] Every item has an owner and a realistic priority.
- [ ] Status reflects reality; cards are dragged when work changes hands.
- [ ] Progress and decisions are recorded as comments, not buried in edits.
- [ ] Due dates only for real commitments.

**RAID**
- [ ] Every RAID item has an owner and a review / validate / resolve / needed-by date.
- [ ] Risks are scored honestly and re-scored when things change.
- [ ] Dependencies name the counterparty and direction.
- [ ] Closed items have an outcome; occurred risks and invalidated assumptions are followed by new items.
- [ ] A regular RAID review is in the calendar.

**Data safety**
- [ ] Automatic daily backup is on; at least 14 kept.
- [ ] Data folder is included in Time Machine (or equivalent).
- [ ] A manual backup is taken before deletions, restores and upgrades.
- [ ] A restore has been rehearsed on a copy.
- [ ] You understand that backups are tied to this Mac's Keychain.

**Security**
- [ ] FileVault on; screen locks automatically.
- [ ] No secrets or personal data in titles, descriptions or comments.
- [ ] Security Audit run monthly; findings acted on.
- [ ] Plain-text exports deleted when finished.

**Housekeeping**
- [ ] Finished projects archived (not deleted).
- [ ] Inactive developers deactivated (not deleted).
- [ ] Log rotation on; the log verified occasionally.

---

## 20. Limitations and caveats (consolidated)

### 20.1 Product scope

| Area | Limitation |
|---|---|
| **Users** | Single-user, single-Mac. No accounts, permissions, sync, sharing or concurrent editing. |
| **Platform** | macOS 14 or later only. No iPhone/iPad/web version. |
| **Notifications** | None: no reminders, emails or badges. |
| **Import** | No import from other tools or files. |
| **Export** | CSV export of the **RAID log** only (and plain-text export of log archives). No export of work items or reports. |
| **Undo** | None. Edits, moves and deletions are immediate; backups are the safety net. |
| **Attachments** | Not supported. |
| **Bulk operations** | No bulk edit, move, delete or label. |
| **Relationships** | No links between issues, sub-tasks or parent/child; no automatic risk→issue conversion. |
| **Estimates, sprints, time tracking** | Not supported. |
| **Customisation** | Statuses, priorities, types, risk scales and rating bands are fixed. |

### 20.2 Behaviours that can surprise you

| Behaviour | Explanation |
|---|---|
| **Project key can't be changed**; issues can't change project. | Keys form issue keys. Create a new project and re-enter if you must restructure. |
| **Developers can't be renamed**. | Add a new developer, reassign, deactivate the old one. |
| **Issue numbers of deleted top items can be reused.** | The next number is "highest existing + 1". |
| **Due dates carry a time of day.** | An item due "today" may turn overdue later that day (6.7). |
| **Due-date changes aren't in history.** | Other changes are. |
| **Dragging a RAID item to Done leaves the outcome blank.** | Record it in the editor. Any status change clears an outcome. |
| **RAID items aren't counted in delivery metrics.** | Completion, turnaround, throughput and Overdue are for work items only. |
| **Dashboard "activity" follows attribution.** | It depends on *Acting as* / assignee, not on who typed. |
| **The Done column never shrinks.** | There is no "hide done" filter. |
| **Changing type deletes unused fields on Save.** | E.g. a dependency's counterparty. |
| **Search sees only the first 300 characters** of each field, the latest 5,000 comments and live log entries, and has result caps. | Section 13.5. |
| **Closing the window quits the app.** | By design. |
| **Comment editing keeps no previous version.** | Plain text only; anyone can edit/delete. |

### 20.3 Data protection caveats

- The live database is **not encrypted**; rely on FileVault and your login password.
- **Backups and log archives depend on this Mac's Keychain key**; there is no key export.
- The log chain cannot reveal removal of the *latest* entries.
- Restoring an old backup also reverts the live log and hides later archives from the list.
- Exports (CSV, log archive export) are **plain text**.

### 20.4 Size and performance

Backpack keeps the project's issues in memory and recalculates summaries on demand. It is comfortable with thousands of issues and many thousands of comments; it is not designed for very large, enterprise-scale data. The log is bounded by rotation; backups copy the whole database each time, so they grow with your data.

---

## 21. Troubleshooting and FAQ

### 21.1 Messages you may see

| Message | Meaning | What to do |
|---|---|---|
| *"Title can't be empty."* / *"Project name can't be empty."* | A required field is blank. | Fill it in. |
| *"Title must be 200 characters or fewer."* | Too long. | Shorten it; move detail to the description. |
| *"Description must be 10,000 characters or fewer."* | Too long. | Shorten or split into comments. |
| *"Project key must be 2–10 letters or digits and start with a letter."* | Invalid key. | Use something like `WEB` or `API2`. |
| *"Database error: UNIQUE constraint failed: projects.key"* | That key is already used. | Choose a different key. |
| *"Database error: UNIQUE constraint failed: developers.name"* | A developer with that name exists. | Use the existing one, or make the name distinct. |
| *"That doesn't look like a valid email address."* | Email format. | Correct or leave blank. |
| *"A label named “x” already exists in this project."* | Duplicate (case-insensitive). | Choose another name or edit the existing label. |
| *"A dependency needs a counterparty: the team, vendor or system involved."* | Dependencies must name who they depend on. | Fill in **Counterparty**. |
| *"Write something before posting a comment."* | Blank comment. | Type text. |
| *"Comments can be up to 10,000 characters; this one is N."* | Too long. | Shorten or split. |
| *"This issue no longer exists."* / *"This comment no longer exists."* | It was deleted while you had it open. | Close and refresh your view. |
| *"Backpack can't open its database"* (at launch) | The database can't be opened (permissions, damage, or created by a newer app version). | See Section 21.3. |
| *"Keychain error (…). The backup encryption key is unavailable."* | macOS refused access to the key. | See Section 21.4. |
| *"Backup is unreadable: authentication failed (wrong key or tampered file)."* | The file isn't encrypted with this Mac's key, or is damaged. | Use a backup made on this Mac; otherwise it cannot be recovered. |
| *"Could not decrypt: …"* (log archive) | Same cause for an archive. | Same. |
| *"⚠︎ Chain broken at #n"* | Log integrity problem. | See Section 21.5. |

### 21.2 "I can't see…" / "Nothing happens when…"

| Symptom | Likely cause and fix |
|---|---|
| **⌘N does nothing** | There is no project to put the issue in; create one first. Otherwise open a board or select a project on the Projects screen. |
| **A project's board is missing from the sidebar** | The project is **archived**. Unarchive it on the Projects screen. |
| **The board looks empty** | Check the three **filters** (Assignee, Type, Label); check the **Group by** setting; check you are on the right project. |
| **A developer isn't in the Assignee list** | They are **inactive**. Switch **Active** on in the Developers screen. |
| **No labels to choose in the editor** | The project has none; use **Labels** to create some. |
| **The Outcome picker is missing** | It shows only for **RAID** types with status **Done**. |
| **The RAID Log's New menu is greyed out** | Several active projects exist and *All active projects* is chosen, or an archived project is chosen. Pick a specific, active project. |
| **Export CSV is greyed out** | No rows are showing. Change the filters. |
| **Search can't find text I know is there** | Only the first 300 characters of each field are searched; the item may be in a rotated log archive; or results were capped—type more. |
| **The dashboard shows "—" or zeros** | The project has no completed work items yet (RAID items are not counted there). |
| **Completion % seems low after adding RAID items** | It shouldn't change: RAID items are excluded. Check the work-item counts in the chart. |
| **"Load Sample Data" has disappeared** | It is offered only when there are no projects. |
| **My data vanished after switching how I run the app** | Xcode builds and `swift run` use different data folders (See Section 3.4). |
| **Security Audit says "Review findings" with nothing red** | Informational items (for example, "App Sandbox" for builds run outside Xcode, or "no key yet" before a first backup) count as "review". |
| **An item due today shows as overdue** | Due dates carry a time of day (See  Section 7.7). |
| **I see a `'` at the start of text in the CSV** | It protects spreadsheets from running text as a formula; it is intentional. |

### 21.3 The app won't start ("can't open its database")

1. Read the message: it states the cause.
2. If it says the database was created by a **newer version**, use the newer app (older apps cannot open it).
3. If it mentions **permissions** or a sandbox problem, check that the data folder is yours and, if you set `BACKPACK_DATA_DIR`, that the build is allowed to use it.
4. If the file is **damaged**: quit; in the data folder rename `backpack.sqlite`, `backpack.sqlite-wal` and `backpack.sqlite-shm` (leave `Backups` alone); relaunch (you get an empty database); then **Security & Backups → Restore…** the latest backup.

### 21.4 Backup or Keychain problems

- The first backup creates the Keychain key and macOS may ask for permission; choose **Allow / Always Allow**.
- If you are running a build that is not code-signed with a team, Keychain access can be refused. In Xcode, select your **Team** under *Signing & Capabilities*, rebuild and try again.
- If the Keychain item was deleted, new backups will create a new key, but **old backups cannot be read**.
- If **Backup freshness** keeps warning, check the **Logs** (filter *Error*) and that the disk is not full.

### 21.5 The log chain is reported broken

1. Don't panic: it means entries don't match their fingerprints. Causes range from tampering to damage.
2. Run **Security Audit** for related findings (integrity, permissions, errors).
3. Look at **Logs** around the reported number for anything unusual.
4. If you suspect damage, **restore** a backup made before the problem and review.
5. Note that the next **log rotation** records that the archived chain was already broken; the break remains visible in that archive record.

### 21.6 Frequently asked questions

**Can several people use it at once?** Not at the same time, and not from different Macs. It is one database on one Mac. People can take turns, using **Acting as** for attribution.

**Can I move an issue to another project?** No. Create it in the other project and delete the original.

**Can I change a project key?** No.

**Can I get my data out?** The RAID log exports to CSV. You can also open a *copy* of the database with a SQLite tool, or export a log archive. There is no export of work items.

**What happens if I delete a developer?** Their items become unassigned; their comments stay with their name; their past activity stops counting on the dashboard. Prefer *Active: off*.

**What happens to RAID items when I archive a project?** They stay, but disappear from the default RAID Log view (choose the project by name to see them).

**Is there a trash or undo?** No. Use backups.

**Do comments count as activity?** Yes: posting, editing or deleting a comment is credited to the author (or your *Acting as* developer).

**Why is there both "history" and "logs"?** History is the story of one issue; the system log is the application's tamper-evident audit trail (section 14).

**Where are my backups?** In the `Backups` folder inside the data folder; use **Show in Finder** in **Security & Backups**.

**Can I use a backup on another Mac?** No: it is encrypted with the original Mac's Keychain key. Copy the live data folder instead (17.5).

**Does Backpack phone home?** No. It makes no network connections.

---

## 22. Quick reference

### 22.1 Keyboard shortcuts

| Shortcut | Action |
|---|---|
| ⌘K | Focus the search bar |
| ↓ ↑ Return Esc | Navigate, open, close search results |
| ⌘N | New Issue |
| ⇧⌘B | Back Up Now |
| ⌘↩ | Post Comment |
| Return / Esc | Save / Cancel in sheets |
| ⌘Q / ⌘W | Quit / close window (also quits) |

### 22.2 Limits

| Item | Limit |
|---|---|
| Project name | 100 characters |
| Project key | 2–10 characters, A–Z and 0–9, starts with a letter, unique |
| Project / issue description | 10,000 characters |
| Issue title | 200 characters |
| RAID plan text | 10,000 characters |
| Counterparty | 100 characters, no line breaks |
| Developer name | 100 characters, unique |
| Developer email | 200 characters, valid format (optional) |
| Label name | 30 characters, unique per project (case-insensitive) |
| Comment | 10,000 characters (Unicode scalars) |
| Probability / Impact | 1–5 |
| Item history shown | latest 100 entries |
| Logs shown | latest 1,000 matching entries |
| Search | first 300 characters per field; 6 projects / 20 issues / 10 comments / 12 log entries per search; latest 5,000 comments and log entries scanned |

### 22.3 Defaults

| Setting | Default |
|---|---|
| New issue: status / priority / type | To Do / Medium / Task |
| New risk: probability × impact | 3 × 3 = 9 (Medium) |
| Dependency direction | We depend on them (inbound) |
| Board grouping | Status |
| Comment order | Newest first |
| Automatic daily backup | On |
| Backups kept | 14 |
| Log rotation | On, above 10 MB, keep 5 archives |
| Engagement window | 14 days |
| Throughput chart | last 6 weeks |
| RAID Log | All active projects · Open only · Sort by Exposure |

### 22.4 Types, statuses, priorities

| Types | Task · Bug · Feature · Risk · Assumption · Issue (RAID) · Dependency |
|---|---|
| **Statuses** | To Do · In Progress · In Review · Done |
| **Priorities** | Critical · High · Medium · Low |
| **Risk ratings** | Low 1–4 · Medium 5–9 · High 10–16 · Critical 17–25 |
| **Outcomes** | Mitigated · Occurred · Accepted · Resolved · Validated · Invalidated · Delivered · Cancelled |
| **Label colours** | gray · red · orange · yellow · green · teal · blue · indigo · purple · pink |
| **Log levels** | Debug · Info · Warning · Error · Audit |

### 22.5 Files and folders

| Path (inside the data folder) | Contents |
|---|---|
| `backpack.sqlite` (+ `-wal`, `-shm`) | The database |
| `Backups/backup-YYYYMMDD-HHMMSS-mmm.bpbak` | Encrypted backups |
| `LogArchives/logs-YYYYMMDD-HHMMSS-mmm-<first>-<last>.bplog` | Encrypted log archives |

### 22.6 Glossary

| Term | Meaning |
|---|---|
| **Active (developer)** | A developer who can be assigned work, act, and comment. |
| **Archive (project)** | Hide a project from active views without deleting anything. |
| **Assignee** | The developer responsible for an item. |
| **Audit (log level)** | A security-relevant or destructive action. |
| **Board** | The column view of a project's items. |
| **Chain (log)** | Each log entry's fingerprint depends on the one before. |
| **Counterparty** | The team, vendor or system a dependency involves. |
| **Dependency** | Something needed from (or owed to) someone else. |
| **Exposure** | The sum of open risks' scores. |
| **Heat map** | The 5 × 5 grid of open risks. |
| **Impact** | How bad it would be (1–5). |
| **Issue key** | Project key plus number (`ATL-12`). |
| **Keychain** | macOS's secure store; holds the backup encryption key. |
| **Outcome** | How a closed RAID item ended. |
| **Probability** | How likely a risk is (1–5). |
| **Rank** | An item's position in the project's single ordering. |
| **RAID** | Risks, Assumptions, Issues, Dependencies. |
| **Risk score** | Probability × impact. |
| **Rotation (log)** | Archiving the live log and starting a fresh chain. |
| **Sandbox** | macOS feature limiting what an app can access. |
| **Work item** | A task, bug or feature. |


 <br>
 
 <hr>
 
 <small>*© 2026 Santak Das, Tara Design Studio. All rights reserved.*</small>

 <br>
