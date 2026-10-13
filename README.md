# Trade capture, governed by its architecture

The demo for *Governing the Agentic SDLC* (Open Source in Finance Forum, New York, 4 November 2026; Matthew Bain and
Aaron Searle).

A small trade capture system: a Trade API, a Position Service, a Trade Store and the Client Ledger, the book of
record. Its architecture is in [`architecture/`](architecture/), written in [FINOS CALM](https://calm.finos.org), and
the controls are written on it. One of them sits on the Client Ledger:

> Any change to a service that connects to the Client Ledger needs a human approver.

An agent opens the pull requests. A review bot reviews them. The **Architecture controls** check is the merge gateway:
it reads the architecture on `main`, decides every pull request against the controls on it, explains itself in a
comment, records each decision on the architecture in CALM Hub, and merges when the answer is yes. When the
architecture changes, the next decision changes with it.

Everything behind it is in [agentic-sdlc-blueprint](https://github.com/rocketstack-matt/agentic-sdlc-blueprint).
