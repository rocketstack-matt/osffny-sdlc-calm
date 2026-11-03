# Trade capture, governed by its architecture

The live demo from *Governing the Agentic SDLC* (Open Source in Finance Forum, New York, 4 November 2026; Matthew
Bain and Aaron Searle). Agents write and review the code here. Which of their changes need a person is decided by
the architecture, not by a reviewer's memory.

## The system, and one rule on it

A small trade capture system: a **Trade API**, a **Position Service**, a **Trade Store**, and the **Client Ledger**,
the book of record. Its architecture is in [`architecture/`](architecture/), written in
[FINOS CALM](https://calm.finos.org). The SDLC controls from the FINOS SDLC Common Controls Catalog are written on
that architecture, and one of them sits on the Client Ledger:

> Any change to a service that connects to the Client Ledger needs a human approver.

Today only the Position Service connects to the Client Ledger.

## Who does what

- **`calm-trade-agent`**, a coding agent, writes the changes and opens the pull requests.
- **`calm-review-bot`**, a review agent, reviews every commit.
- **`calm-merge-gateway`** posts the **Architecture controls** check. It reads the architecture on `main`, decides
  each pull request against the controls written on it, explains the decision in a comment, and records it on the
  architecture in CALM Hub. `main` takes a pull request only when this check has passed.
- **@aaronsearle** approves when a person is needed.
- **@rocketstack-matt** is the architect, who changes the architecture.

## The journey, one pull request at a time

Open the [pull requests](https://github.com/rocketstack-matt/osffny-sdlc-calm/pulls?q=is%3Apr) and read them in the order they were opened. On each one, look at
the **Architecture controls** check and the gateway's comment beneath it.

1. **Account summary endpoint on trade-api.** A harmless change. The review agent approves it, the check goes green
   on its own, and it merges. No person was needed: the Trade API does not connect to the Client Ledger.
2. **Position total endpoint on position-service.** Just as small, but the Position Service writes to the Client
   Ledger. The check goes red and names the person who must approve. Aaron approves on GitHub, the check goes green,
   and it merges. The rule was not in a CODEOWNERS file or anyone's head. The gateway found it on the architecture.
3. **Read positions straight from the ledger file in trade-api.** A shortcut: the Trade API reaching into the ledger
   directly. The check goes red because that connection is not in the architecture, so it does not exist. The pull
   request is closed.
4. **Model: trade-api writes end-of-day snapshots to the Client Ledger.** The architect changes the architecture
   itself: the Trade API now connects to the Client Ledger, through a connection that is designed and approved. A
   change to the architecture needs a person, so Aaron approves it and it merges. The architecture is now version
   1.1.0.
5. **Trade count endpoint on trade-api.** The same kind of harmless change as the first. This time the check goes red
   and asks for a person, because the Trade API now connects to the Client Ledger. Nobody changed the pipeline, the
   check or its configuration. The architecture changed, and the control followed it.

Every decision, allowed or not, is recorded on the architecture as a CALM decorator: which control, on which part of
the system, for which commit, and who reviewed or approved. During the talk CALM Hub ran on the presenter's laptop,
and the slides show it.

## Run it yourself

This repository is put back to its start before every run, from
[agentic-sdlc-blueprint](https://github.com/rocketstack-matt/agentic-sdlc-blueprint). Everything behind the demo is
there: the architecture and its controls, the merge gateway, the specs, a version you can run on your laptop with
Node and git, and the slides. Fork that repository, not this one.
