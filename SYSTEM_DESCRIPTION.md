# System Description — POD Split Route

## 1. Overview

**POD Split Route** is a module of the **POD Update Application**, a back-office tool used in parcel delivery / courier operations to manage Proof of Delivery (POD) data and route allocation. The Split Route screen allows a depot operator to move (split) parcels from one delivery route to another — for example, when a route is overloaded, a driver is unavailable, or deliveries need to be rebalanced across drivers before dispatch.

This project is a **standalone UI prototype** of that screen, implemented as a single self-contained HTML file ([prototype.html](prototype.html)) with embedded CSS and vanilla JavaScript. It uses in-memory sample data and requires no backend, build step, or dependencies — it runs directly in any modern browser. A reference screenshot is included as [pod_split_route_verify.png](pod_split_route_verify.png).

## 2. Purpose and Scope

The prototype demonstrates the end-to-end workflow of splitting a route:

1. Select the **user** (operator/driver code), the **From Route** (source), and the **Allocate to Route** (destination).
2. **Search** to load the parcels currently assigned to the source route.
3. Select one or more parcels and **move** them to the destination route.
4. Optionally **reverse** moved parcels back to the source route before committing.
5. **Save** to confirm the reallocation.

Out of scope for the prototype: authentication, persistence, server integration, and the other application modules shown in the tab strip (Route Allocation, Extra Route Allocation, Manual Delivery Password, Driver List, Load Data, Summary, Missing Parcels, etc.), which appear for navigational context only.

## 3. User Interface

The screen is composed of:

| Area | Description |
|---|---|
| **Header bar** | Application title ("POD Update Application") with Logout and Exit buttons. |
| **Tab strip** | Navigation across the application's 13 modules; "POD Split Route" is the active tab. |
| **Search Route card** | Dropdowns for Search User, From Route, and Allocate to Route, plus a live selection-count badge and Search / Clear / Save buttons. |
| **POD Split Route card** | A dual-panel (master/target) layout: left panel lists parcels assigned to the source route; right panel lists parcels allocated to the destination route; a center column holds the → (move) and ← (reverse) arrow buttons. |
| **Footer status bar** | Displays the result of the last action (e.g. "Moved 3 parcels from Route 101 to Route 105."). |

Each parcel row shows **Parcel**, **Consignment number**, **Delivery Area**, and **Status**, with a per-row checkbox and select-all checkboxes in both panels. The layout is responsive: below 860 px the panels stack vertically and the move arrows rotate.

## 4. Functional Behavior

### 4.1 Search
- Search is enabled only when User, From Route, and Allocate to Route are all selected.
- The destination dropdown automatically excludes the currently selected source route, preventing a route from being split onto itself.
- Changing the user or source route resets the loaded data and prompts a new search; changing the destination route clears any pending (unsaved) allocations.

### 4.2 Move / Reverse
- **Move (→)**: transfers all checked parcels from the source list to the destination (allocated) list. Enabled only when a route is loaded, a destination is chosen, and at least one parcel is selected.
- **Reverse (←)**: returns checked parcels from the allocated list back to the source route. Enabled only when at least one allocated parcel is selected.
- Both panels support row-click or checkbox selection, plus select-all with an indeterminate state for partial selections.

### 4.3 Save / Clear
- **Save** commits the pending split and is enabled only when there are unsaved moved parcels (`hasUnsavedChanges`). In the prototype the "commit" clears the unsaved flag and reports the count via the status bar; in a production system this would call a backend API.
- **Clear** resets all dropdowns, selections, and both tables to their initial empty state.

### 4.4 Status Feedback
Every action (search, move, reverse, save, clear, validation failures) writes a human-readable message to the footer status bar.

## 5. Data Model (Prototype)

All data is held in JavaScript memory:

- **Users** — 25 numeric operator codes (e.g. `001`, `005`, … `089`).
- **Routes** — 20 route numbers, `101`–`120`.
- **Parcels** — keyed by route (`routeParcels`); each parcel has `id`, `name`, `consignment` (e.g. `CN101001`), `area` (UK West Midlands delivery areas such as Birmingham Central, Solihull, Walsall, Coventry), and `status` (`Assigned`). Sample data is seeded for routes 101–104.

Session state tracks the active user/route, the two selection sets (source and allocated), the pending allocated-parcel list, and an unsaved-changes flag that gates the Save button.

## 6. Technical Characteristics

- **Stack:** HTML5, CSS3 (custom properties / design tokens, CSS Grid and Flexbox), vanilla ES6+ JavaScript. Inter font loaded from Google Fonts (the only external dependency).
- **Architecture:** single-file, no framework, no build tooling; state-driven rendering where every state change re-renders the affected table and recomputes button enable/disable states through a single `updateActionState()` function.
- **Accessibility:** semantic landmarks (`header`, `nav`, `main`, `section`, `aside`), `aria-label`s on tables, checkboxes, and controls.
- **Design system:** tokenized color palette (brand blue `#1e40af` on a slate background), card-based sections, sticky table headers, status pills, and hover/selected row styling.

## 7. Intended Evolution

As a prototype, this file serves as the visual and behavioral specification for the production POD Split Route screen. Productionizing it would involve replacing the in-memory `users`, `routes`, and `routeParcels` data with API calls, implementing the Save action against the routing backend, and wiring the tab strip to the other application modules.
