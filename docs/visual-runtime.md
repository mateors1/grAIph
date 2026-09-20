# Host-Independent Visual Runtime

**Status:** Implemented and verified locally with limitations

**Reviewed:** 2026-09-20

The graph visualization is a core product surface, not a disposable rendering of hidden state. The development implementation allows one shared UI to attach through the native VS Code host or an authenticated local browser host, including the Codex browser surface.

MCP and interactive edits share the authoritative graph runtime, persistence, revisions, receipts, and publication path.

---

## Architecture

The visual runtime is divided into explicit responsibilities:

| Layer | Responsibility |
|---|---|
| Visual protocol | Versioned commands, queries, snapshots, events, errors, capabilities, and commit receipts |
| Graph runtime | Authoritative document, session, revision, persistence, replay, ownership, and command execution |
| Graph client | Host-independent synchronization, snapshot recovery, replay, conflict handling, and resync state |
| Local server | Authenticated loopback commands, snapshots, events, discovery, and bounded session lifecycle |
| Browser host | Local pairing, transport, safe provider/settings projection, and UI bootstrap |
| Shared UI | Graph rendering and host-independent command dispatch |
| VS Code host | Native transport, workspace authorization, runtime ownership, packaging, and host-specific effects |
| MCP integration | Bounded graph reads and mutations against the same runtime |

The package boundaries are implementation details; the public architectural invariant is that there is one authoritative graph document per runtime, not a graph copy per interface.

---

## Shared state versus local view state

The protocol separates durable project state from viewer presentation.

### Shared committed state

Examples include:

- adding, updating, removing, or connecting graph nodes;
- applying accepted generation results;
- importing a project or external specification;
- durable node metadata and manual-override state;
- safe provider/model/settings identity;
- job identity and committed completion state.

Each accepted graph command advances the authoritative revision and document hash. Other viewers observe the resulting commit through snapshot or replay.

### Viewer-local state

Examples include:

- selection and hover;
- viewport position and zoom;
- drill navigation;
- open panels;
- draft text before an explicit commit.

Viewer-local state is not echoed as graph authority. A Codex-browser viewport change must not move the VS Code viewport, and a local selection must not become a durable graph mutation.

---

## Origins and viewers

The acceptance model distinguishes mutation origins from rendering viewers.

Supported origin classes are:

- MCP;
- VS Code user;
- Codex-browser user;
- standalone-browser user.

Supported viewer classes are:

- VS Code webview;
- Codex browser;
- standalone browser.

For committed graph commands, the intended invariant is convergence on the same runtime identity, revision, document hash, and committed snapshot. That creates a twelve-cell origin/viewer matrix.

The latest exact-build public evidence covers one origin—Codex browser—observed in all three viewers. Earlier functional fixtures and historical live checks are retained separately; they are not silently promoted into a final-build performance claim.

---

## Authority and synchronization

### Runtime ownership

Exactly one owner is responsible for an authoritative graph runtime. Clients discover and attach to that owner; they do not create a second writable graph when attachment is expected.

### Command admission

A durable command is bound to:

- runtime and workspace identity;
- graph identity;
- expected revision;
- authenticated session and principal;
- command schema and policy;
- idempotency or receipt identity where applicable.

Stale compare-and-set mutations fail explicitly. They do not overwrite newer graph state.

### Replay and recovery

Clients may recover through bounded event replay or a fresh snapshot. If invalidations arrive during recovery, the client coalesces follow-up recovery rather than claiming a false live state. Bounded recovery eventually enters an explicit resync-required state instead of looping indefinitely.

### Large graph reads

Graph reads respect the shared output budget. When a full document cannot be projected safely, the response remains valid and explicitly partial, with the authoritative graph version included. The runtime does not return truncated invalid JSON as if it were a complete graph.

---

## Local pairing and secret boundary

Browser attachment uses authenticated loopback transport and local pairing. Credentials and provider secrets remain host-side and are not persisted in browser storage or published through discovery.

Browser-visible settings are safe projections, not the trusted settings store. Workspace paths, command authority, provider credentials, and effect approvals remain host-owned.

Session cleanup is authenticated and protected against cross-site request forgery. Reconnect and reload must not silently replay an effect request.

---

## Generation and other effects

Generation, project execution, dependency installation, and workspace import are effects rather than ordinary graph edits. They require host policy, request identity, and correlated results.

Progress and token streams may be shared across viewers, but they remain observational. A final accepted generation still requires the candidate and host-finalization boundary described in [Pipeline overview](./pipeline-overview.md).

---

## Current evidence

The latest reviewed exact-build run performed 35 actual Codex-browser Add Node actions on a graph growing from 500 to 535 nodes while native VS Code and standalone Chrome were attached.

All 35 observations matched:

- authoritative runtime, workspace, graph, and epoch identity;
- durable revision and document hash;
- command receipt;
- VS Code layout marker;
- Chrome layout marker;
- Codex-browser layout marker.

The native viewport remained unchanged throughout the run.

Receipt-to-layout results were:

| Viewer | Median | p95 | Minimum / maximum |
|---|---:|---:|---:|
| Native VS Code | 672 ms | 1266 ms | 544 / 1917 ms |
| Standalone Chrome | 776 ms | 1414 ms | 633 / 2014 ms |
| Codex browser | 820 ms | 1506 ms | 683 / 2045 ms |

The measurements include persistence and renderer layout. They are not pure network latency or physical display-presentation measurements.

---

## Limitations and open qualification

- Only one mutation origin was measured in the latest exact-build run.
- The graph grew during measurement; this is not a fixed-size comparison.
- No live provider generation was used.
- All three p95 results fail the unchanged 250 ms target.
- A complete final-build twelve-cell performance matrix remains open.
- Steady-state memory, a long soak, and production deployment evidence remain open.
- Correct synchronization does not imply acceptable interaction latency.

See [Current state](../CURRENT_STATE.md) and [September 2026 progress](./progress-2026-09.md).
