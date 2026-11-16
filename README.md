# Persistent computation mechanism

`Kernels(transport, observe, runtimes)` maintains Python or Node variables across
cells in an owned process. The package uses only the standard library. It does
not launch processes itself, evaluate submitted code in the host interpreter,
select accounts or grant permissions. Callers provide authorized process
operations, a durable observation sink and trusted absolute runtime paths.

The transport owns `list`, `start`, `write`, `wait` and `terminate` effects. The
observer records kernel and cell identities, generations, output and honest
outcomes. One cell executes at a time. Reset requires observed termination;
lost ownership or uncertain delivery never replays code or reconstructs variables.
The adjacent Python and Node runners execute in those owned child processes.
Node's VM is a persistent namespace, not a security boundary. Existing process
authority and cleanup still apply.

## Build and test independently

Use an owned Python 3.13 or newer environment. From this directory:

```sh
uv build
uv venv .venv
uv pip install --python .venv/bin/python "dist/amplifier_kernels-0.1.0-py3-none-any.whl[test]"
```

Before running the tests, install the public `operations-v0.1.0` wheel
(`amplifier-operations` 0.1.0, SHA-256
`05675519079bbfb75be9ce0bc6fcba8c4a4ba6fd06dc8105ca706aec5ac43a1a`),
`amplifier-module-tool-bash` from public commit
`d5bb46d24dcae8e260211bf8fb5c22de3a67f406`, and the compatible public
`amplifier-core` 2.0.2 wheel into this same environment. BashTool imports Core
without declaring it; this is a test-peer requirement, not a kernels runtime
dependency. Include the peers' declared dependencies. Do not install Unified to
obtain these peers or build Core from source. The
[package workflow](.github/workflows/package.yml) pins the Linux
publication-test wheels by SHA-256 and uses Node 22.23.3. It installs the original
kernels wheel outside the checkout and requires all ten cases without skips.

Run `.venv/bin/python -m pytest -q tests` after installing those prerequisites.
Python and Node must both be available to exercise all ten mechanism cases.
A missing BashTool or Node causes existing fixture skips, which are limitations,
not successful real-process qualification. The cases cover persistent state,
runtime identity, reset, interruption, bounded output, damaged protocol, unknown
termination, denied start and durable finalization under reader cancellation.
No account or model call is required. This preparation is qualified on Linux;
it does not establish Windows process transport support.

To verify the installed artifact, copy only `tests/` and this `pyproject.toml`
outside the source checkout and run there without a source `PYTHONPATH`. The wheel
contains the four runtime files, including `node_runner.cjs`; the source archive
also carries these tests. Standalone editable development uses this same source.

## Ownership and provenance

This history-preserving project is prepared as the public author home at
`bkrabach/amplifier-platform-kernels`, with a later transfer to Microsoft planned.
[PROVENANCE.json](PROVENANCE.json) records the original public Unified source,
pre-move history, and exact imported runtime/test bytes. Original MIT attribution
is preserved. The repository name does not rename the `amplifier-kernels`
distribution or `amplifier_kernels` import.

This import changes no runtime behavior, dependency requirement, package version,
or maintained test. Unified retains its host integration and policy. Authoring
handoff and consumer selection are separate reviewed changes; retire Unified’s
authored project at handoff rather than maintain two authoring copies.

Use a separate environment for this distribution. Do not coinstall it with
`amplifier-unified`, which currently owns its selected `amplifier_kernels` files.
This source import is not independent pip replacement inside a Unified prefix.

## Existing delivery and future publication

Unified continues to select the immutable [kernels-v0.1.0 artifacts](https://github.com/microsoft/amplifier-unified/releases/tag/kernels-v0.1.0)
from their original public producer. The original source receipt, revision,
subdirectory and hashes remain historical identities; this repository does not
republish or relabel those artifacts. Unified’s generated `amplifier_kernels/`
is selected by `components/kernels.source.json` and verified offline during
normal builds. Do not edit that generated copy by hand.

The [package workflow](.github/workflows/package.yml) builds one wheel and source
archive, then tests that wheel outside the checkout. It does not publish a release
or change any consumer. A future publication needs an exact source/artifact
receipt and review of Unified’s selector for this repository-root project layout;
the existing selector identifies historical monorepo subdirectories. Source moves
do not reserve a new version or make the original receipts invalid.

ACP’s separate public Git selection also remains unchanged; updating this author
repository does not update ACP or Unified. The package workflow reuses the exact
public BashTool, operations and Core wheel test peers from the existing producer.
Core is a test-peer dependency only; no Core source build, model or account is used.

## Project guidance

The original MIT license and Microsoft open-source conduct, security and support
guidance are retained for this eventual Microsoft home. This repository is
currently hosted under `bkrabach`; it does not claim Microsoft organization
automation, CLA enforcement, release service connections or support coverage.
The existing security-reporting guidance is retained; do not post vulnerabilities
in public issues.
