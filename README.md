# VenueTracker

A single-file, offline tool for managing academic paper submissions. Track which venues fit your papers, monitor deadlines, manage submission status through the review pipeline, and discover special issues -- all inside one HTML file with zero dependencies.

No server, no account, no installation. Download one file, open it in your browser, done.

## Screenshots

> *Coming soon -- screenshots of all 5 tabs.*

## The 5 Tabs

### Tab 1: Paper-Venue Match

Select one of your papers and see all matching venues ranked by fit. The target venue is highlighted with a green checkmark. Each venue card shows tier rating, impact factor, acceptance rate, review cycle, and your custom fit notes.

> *Screenshot placeholder: Paper-Venue Match view*

### Tab 2: Special Issues / CFP Tracker

Track calls for papers and special issues with deadline countdowns. Cards are color-coded by urgency (red = less than 30 days, yellow = 30--90 days, green = more than 90 days, gray = expired). Add new special issues via the "Add Special Issue" button.

> *Screenshot placeholder: Special Issues view*

### Tab 3: Deadline Calendar

A chronological timeline of all deadlines, grouped by month. Combines submission deadlines, special issue deadlines, and custom timeline events. Past deadlines are dimmed. Each entry shows a countdown badge.

> *Screenshot placeholder: Deadline Calendar view*

### Tab 4: Submission Status

A table showing each paper's target venue, current status, last update, and deadline. Click a row to expand its timeline and see the full history. Click the status badge to cycle through stages: Idea > Experimenting > Writing > Ready > Submitted > Under Review > Revision > Accepted > Rejected.

> *Screenshot placeholder: Submission Status view*

### Tab 5: Venue Database

A searchable, filterable grid of all venues. Filter by type (Journal / Conference). Click any venue card to expand it and see full details: publisher, field, tier, impact factor, acceptance rate, review cycle, frequency, and website link.

> *Screenshot placeholder: Venue Database view*

## Quick Start

1. **Download** `index.html` (or clone this repository).
2. **Double-click** the file to open it in any modern browser (Chrome, Firefox, Safari, Edge).
3. **That's it.** You will see 3 demo papers and 8 demo venues. Read below to replace them with your own data.

## How to Add Your Papers

Open `index.html` in a text editor (VS Code, Sublime Text, Notepad++). Search for `MY_PAPERS` (Ctrl+F or Cmd+F). You will find it around **line 240**. It looks like this:

```javascript
const MY_PAPERS = [
  {id:"PaperA", label:"Paper A: Survey on X", ...},
  ...
];
```

Here is the full template with every field explained:

```javascript
{
  id: "PaperA",                      // Short unique identifier. Referenced by other arrays.
  label: "Paper A: Survey on X",     // Short label shown in buttons and badges.
  title: "Full Paper Title Here",    // Full title shown in the Paper-Venue Match view.
  status: "writing",                 // Initial status. Options:
                                     //   "idea"          -- concept stage
                                     //   "experimenting" -- running experiments
                                     //   "writing"       -- drafting the manuscript
                                     //   "ready"         -- ready to submit
                                     //   "submitted"     -- sent to venue
                                     //   "under_review"  -- being reviewed
                                     //   "revision"      -- revising after review
                                     //   "accepted"      -- accepted for publication
                                     //   "rejected"      -- rejected
  target_venue: "nature",            // The `id` of your target venue from the VENUES array.
  deadline: "2026-09-01",            // Target submission deadline (YYYY-MM-DD), or null if none.
  color: "#e74c3c",                  // Color for badges and highlights (any hex color).
  notes: "Draft in progress, ..."    // Free-text notes shown in the match view.
}
```

### Important

- The `id` value is used to link papers across all data arrays (`VENUES.my_papers`, `SUBMISSIONS.paper`, `DEADLINES.paper`). Keep it consistent.
- The `target_venue` must match a venue `id` in the `VENUES` array.

## How to Add Venues

Search for `VENUES` (around **line 229**). Here is the full template:

```javascript
{
  id: "nature",                        // Short unique identifier.
  name: "Nature",                      // Short name shown in cards and tables.
  full_name: "Nature",                 // Full journal/conference name.
  type: "journal",                     // "journal", "conference", or "workshop".
  publisher: "Springer Nature",        // Publisher name.
  field: "Multidisciplinary Science",  // Research field.
  tier: "A+",                          // Tier rating: "A+", "A", "B", or "C".
                                       //   A+ = 3 stars, A = 2 stars, B = 1 star.
  impact_factor: "64.8",              // Impact factor (string). Use "-" for conferences.
  acceptance_rate: "~8%",              // Approximate acceptance rate.
  review_cycle: "2-6 months",          // Typical time from submission to decision.
  frequency: "Weekly",                 // Publication frequency (e.g., "Monthly", "Annual, June").
  website: "https://www.nature.com/",  // Official website URL.
  my_papers: ["PaperA"],              // Which of YOUR papers fit this venue.
                                       //   Use the `id` values from MY_PAPERS.
  fit_notes: "Very competitive...",    // Your notes on why this venue fits (or doesn't).
  reviewer_goal: false                 // true if you want to become a reviewer here.
}
```

### Linking papers to venues

A venue appears in the Paper-Venue Match tab for a given paper only if that paper's `id` is listed in the venue's `my_papers` array. For example, if you add `"PaperD"` to a venue's `my_papers`, that venue will show up when you select Paper D in Tab 1.

## How to Add Special Issues

Special issues are added through the UI:

1. Go to the **Special Issues** tab (Tab 2).
2. Click the **"+ Add Special Issue"** button in the top right.
3. Fill out the form:
   - **Journal / Conference**: select from your venue database.
   - **Title**: the name of the special issue or call for papers.
   - **Deadline**: submission deadline.
   - **Description**: topic description, guest editors, scope, etc.
   - **Link**: URL to the CFP page.
   - **Related Papers**: check which of your papers this issue is relevant to.
4. Click **Save**. The special issue appears immediately, and its deadline is added to the Calendar tab.

To **delete** a special issue, click the red "Delete" button on the card.

Special issues are stored in your browser's localStorage. They persist across sessions but are browser-specific (see Data Storage below).

## How to Track Submissions

### Setting up submission entries

Search for `SUBMISSIONS` (around **line 246**) to define your initial submission records:

```javascript
{
  paper: "PaperA",                // Must match a MY_PAPERS id.
  venue: "nature",                // Must match a VENUES id.
  status: "writing",              // Initial status (same options as MY_PAPERS status).
  timeline: [                     // List of events in chronological order.
    {date: "2026-01-10", event: "Started literature search"},
    {date: "2026-03-01", event: "Outline finalized"},
    {date: "2026-09-01", event: "Target submission deadline"}
  ]
}
```

### Cycling through status

In the **Submission Status** tab (Tab 4), click the colored status badge on any row to cycle through the stages:

Idea > Experimenting > Writing > Ready > Submitted > Under Review > Revision > Accepted > Rejected

The status change is saved in localStorage and reflected across all tabs.

### Adding timeline events via the UI

1. In the Submission Status tab, click **"+ Add Event"** in the top right.
2. Select the paper, enter a date and event description.
3. Click **Save**. The event appears in the submission timeline and in the Calendar tab.

### Deadlines

Search for `DEADLINES` (around **line 267**) to define calendar events:

```javascript
{
  date: "2026-05-22",                          // Date in YYYY-MM-DD format.
  label: "NeurIPS 2026 Abstract Deadline",     // Description shown in the calendar.
  type: "submission",                          // "submission", "si", or "event".
                                               //   submission = blue left border
                                               //   si         = orange left border
                                               //   event      = green left border
  paper: "PaperB"                              // Related paper id, or null.
}
```

## Deadline Color Coding

All deadlines across the app use consistent color coding:

| Color | Condition | Meaning |
|-------|-----------|---------|
| Red | Less than 30 days | Urgent |
| Yellow | 30--90 days | Approaching |
| Green | More than 90 days | Comfortable |
| Gray | Past deadline | Expired |

This applies to the Calendar tab countdown badges, Special Issues card borders, and deadline columns in the Submission Status table.

## Data Storage

VenueTracker uses two layers of storage:

| What | Where | Persists across sessions? |
|------|-------|--------------------------|
| Papers, venues, submissions, deadlines | Inside `index.html` (JavaScript arrays) | Yes -- part of the file itself. |
| Status changes (clicked via badge) | Browser `localStorage` (key: `venue_tracker_status`) | Yes, same browser only. |
| Special issues (added via UI) | Browser `localStorage` (key: `venue_tracker_si`) | Yes, same browser only. |
| Timeline events (added via UI) | Browser `localStorage` (key: `venue_tracker_events`) | Yes, same browser only. |

This means:
- **Preset data** (venues, papers, submissions, deadlines) is safe inside the HTML file. Back up the file to back up this data.
- **UI-added data** (special issues, timeline events, status changes) lives in localStorage. If you clear browser data or switch browsers, these will be lost. To make them permanent, add them directly into the HTML arrays.

## FAQ

**Can I use this offline?**
Yes. VenueTracker is 100% offline. No internet connection needed. It is a single HTML file with no external dependencies.

**What happens if I clear my browser data?**
Your preset data (papers, venues, submissions, deadlines defined in the HTML) is safe. However, any special issues added via the UI, timeline events added via the UI, and status changes made by clicking badges will be lost. To protect important entries, copy them into the HTML arrays directly.

**Can I share this with my lab?**
Yes. Share the `index.html` file. Each person's localStorage is independent, so status changes and UI-added special issues are per-person.

**What if the page loads blank?**
Open the browser developer console (F12 > Console tab) to see the error message and line number. The most common mistake is a missing comma between entries or a mismatched bracket in the data arrays.

## License

MIT
