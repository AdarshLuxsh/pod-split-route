# POD Split Route

A standalone UI prototype of the **POD Split Route** screen from the POD Update
Application — a back-office tool used in parcel delivery / courier operations to
manage Proof of Delivery data and route allocation.

The screen lets a depot operator move (split) parcels from one delivery route to
another, for example when a route is overloaded, a driver is unavailable, or
deliveries need rebalancing before dispatch.

## Live demo

**https://adarshluxsh.github.io/pod-split-route/**

## Running locally

The prototype is a single self-contained HTML file with embedded CSS and vanilla
JavaScript. There is no backend, build step, or dependency to install — open
[prototype.html](prototype.html) directly in any modern browser.

## Workflow

1. Select the **user** (operator code), the **From Route** (source), and the
   **Allocate to Route** (destination).
2. **Search** to load the parcels currently assigned to the source route.
3. Select one or more parcels and **move** them to the destination route.
4. Optionally **reverse** moved parcels back to the source route.
5. **Save** to confirm the reallocation.

## Contents

| File | Description |
|---|---|
| [prototype.html](prototype.html) | The complete prototype — HTML, CSS and JavaScript in one file. |
| [SYSTEM_DESCRIPTION.md](SYSTEM_DESCRIPTION.md) | Full specification: UI breakdown, functional behavior, data model. |
| [pod_split_route_verify.png](pod_split_route_verify.png) | Reference screenshot. |
| [index.html](index.html) | Redirect so the GitHub Pages root opens the prototype. |

## Scope

All data is held in memory as sample data (25 operator codes, routes 101–120,
seeded parcels for routes 101–104). Authentication, persistence, server
integration, and the other application modules shown in the tab strip are out of
scope — those tabs are present for navigational context only.

See [SYSTEM_DESCRIPTION.md](SYSTEM_DESCRIPTION.md) for the full specification.
