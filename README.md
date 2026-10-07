# Barriers by SENTRY — first local proof

An executable simulation of defensive barriers, bounded authority, timed human intervention, and SENTRY self-containment. **No production integration, network mitigation, operating-system sandbox, or general intrusion detection is implemented.** There are no external dependencies or network adapters.

**Barriers by SENTRY does not replace traditional firewalls or endpoint security.** It is intended as an additional layer for protecting AI models and governing defensive containment. Existing network and endpoint protections remain necessary; deploying SENTRY is not a reason to disable or remove them. Integration with those protections is proposed, not implemented in this local prototype.

**Intended role:** SENTRY is a containment mechanism designed to detect and contain exfiltration and poisoning attempts that pass conventional security controls. Intelligence may identify suspicious activity and propose countermeasures; an independent controller limits consequential responses to pre-authorized actions and records evidence and reconciliation. This is the product's intended purpose, not a claim that the prototype reliably detects real-world attacks missed by other tools. Exfiltration is currently represented by trusted simulated events, and poisoning detection has demonstrated blind spots in synthetic tests.

## Brainstorming enhancements in this prototype

All eight enhancements below are included in this JavaScript prototype as **local experiments**. They were added together directly to `main` in [commit a0717b3](https://github.com/spikediegel-prog/SENTRY/commit/a0717b3752d3d5d27552cbbc4672cc82d5707869); they do not have separate pull-request review records.

| Enhancement | Implemented behavior | Limit |
| --- | --- | --- |
| **AI action checkpoint** | Checks simulated read/write/send proposals against fixed scopes, valid leases, uncertainty, access holds, and required permissions | No live AI tool, file, or message integration |
| **Exfiltration tripwires** | An exact synthetic marker in a send proposal holds the affected session and can trigger timed escalation | No semantic or encoded exfiltration coverage |
| **Memory quarantine** | Submitted memory starts quarantined; admission requires permitted provenance, content-hash review, and independent signed approval | Finite injection screening has blind spots; approval does not prove content is safe |
| **Countermeasure rehearsal** | Evaluates advisory challenge/contain candidates against finite attack and legitimate-text examples, recording misses and disruption | No LLM-generated executable defenses or automatic installation |
| **Incident replay** | Authenticates encrypted journal records against a trusted head/count and matches action intents with reconciliation receipts | Recorded receipts are not independent proof of incident truth |
| **Containment health checks** | Compares simulated adapter observations with the expected session/action/resource/content; failure closes authority and holds access | No independent production sensor |
| **Recovery rehearsal** | Requires clean replacement, rotates worker capabilities, preserves access holds, and requires separately authorized incident-bound health checks before release | No durable recovery service, real attestation, or OS sandbox |
| **Operator dashboard** | Displays an offline interactive snapshot of authority state, provenance, holds, rehearsal results, and authenticated replay | No live countdown or execution controls |

The prototype also includes **expiring, one-use scoped permissions** and **learning provenance/source withdrawal**. Intelligence cannot issue permissions, promote candidates, change the boot policy, or restore its own authority.

These enhancements live in a separate trusted AI-boundary fixture, not a fully integrated production service. See [architecture, limits, and claim classifications](docs/ai-boundary-exploration.md).

Run the focused adversarial proofs and generate the dashboard:

```text
node --test tests/ai-boundary.test.js
node src/ai-boundary-demo.js
```

The recorded full suite passed **104 tests**, including **27 AI-boundary tests**. The finite rehearsal deliberately records both a missed attack and a false alarm; successful assertions do not establish general attack detection or host-compromise resistance. See [validation record](boundary-validation.json).

For the narrower Rust authority core, native Windows/Linux CI, and preserved JavaScript reference, use [SENTRY-External-Testing](https://github.com/spikediegel-prog/SENTRY-External-Testing). Not all JavaScript enhancements have been ported into Rust. Real integrations, production isolation, real attestation, and authenticated durable Rust storage remain **Proposed**.

## Run the original simulations

Requires Node.js 22 or later and Windows with the current user's profile loaded for recoverable key protection. Protected saved artifacts require the separate private vault and that user's profile to decrypt; the ZIP intentionally contains no recovery secrets. From this directory:

```text
node --test tests/*.test.js
node src/guardian.js
node src/demo.js
node src/load-demo.js
node src/learning-demo.js
```

The first command runs the adversarial protocol tests and controller failure tests. Guardian writes `guardian-report.json` and per-scenario evidence under `runs/`. The demo writes `demo-report.json` and a durable evidence journal. Runs create new directories; reports contain the latest run. Test-only journals use temporary directories. No install step or package manager is needed. Equivalent npm scripts are included for environments with npm installed.

The demo inserts credential misuse into a noise stream, contains the affected session, preserves an unrelated legitimate session and the fixed control channel, closes the Intervention Window, expires SENTRY's lease, denies further execution, and recovers with a replacement instance. Guardian deliberately attacks protocol inputs; its trusted fixture signs observations and recovery operations to exercise those trusted paths. Guardian is not part of the production authority architecture.

## What exists

The [AI boundary exploration](docs/ai-boundary-exploration.md) adds a separate local fixture for action checks, expiring one-use permissions, synthetic exfiltration tripwires, memory quarantine, learning provenance, finite countermeasure rehearsal, authenticated replay, containment health checks, and recovery exercises. Run `node src/ai-boundary-demo.js` to generate `operator-dashboard.html`, an offline interactive operator snapshot. This fixture is not wired into the existing controller's production interface; no live AI, tool integration, persistent recovery service, or additional process sandbox is implemented.

- Separate controller and SENTRY subprocesses; the supervisor forwards worker requests only to the worker lane.
- An immutable boot policy, fixed session scope, authenticated observation/human/supervisor lanes, replay counters, and a rotating instance capability.
- Deterministic junk drop, challenge flags, simulated containment routes, isolation, credential revocation, and case-scoped human cancellation.
- Independent lease expiry and integrity failure handling with recorded degraded/quarantine/safe/recovery transitions.
- Intent and reconciliation records, authenticated hash-chain evidence, preserved state, and an emergency reconciliation path.
- Lossless delta evidence encoding with periodic full checkpoints; no action receipts or security transitions are suppressed.
- Two analysis processes with two threads each, bounded queues, reserved signal capacity, and independent authenticated control routing. Configuration is in `workload.json`.
- Bounded counter learning and administrator recommendations; an optional frozen, trained logistic poisoning-risk model runs in shadow mode. No LLM or fine-tuning is implemented.
- Background candidate fitting in a separate bounded process; fixed CHALLENGE/CONTAIN advisory buttons pass through controller evidence, uncertainty and lease checks. See `docs/learning.md`.
- Finite countermeasure development, encrypted controller logs/training artifacts and authenticated encrypted local transport. Windows profile access is required for recoverable key wrapping; the private vault is excluded from the ZIP. See `docs/protection-countermeasures.md`.
- Separate signed attack-permutation training and held-out combination evaluation, with explicit order/semantic blind-spot probes. Candidates remain uninstalled; see `docs/permutations.md`.
- Disabled-by-default simulated endpoint bridge for Defender/Falcon-labeled fixtures, with separate host permissions, suspicion-only alerts, timed escalation and audit receipts. No real vendor connection is implemented; see `docs/endpoint-integrations.md`.
- Local operator failure alerts and affected-session access holds. Acknowledgment or clean-instance recovery never automatically grants access; external notifications and a real sacrificial execution cage remain proposed. See `docs/operator-containment-alerts.md`.
- A versioned, searchable CVE reference library; authenticated/minimized telemetry export; separately signed labels and Guardian datasets; campaign-separated training/evaluation.
- Executable negative tests covering the requested attack categories.

## Read before interpreting a result

**Verified** means a named executable assertion passed for this local model. It is not a universal security proof. **Recorded** means a run or observation was stored without establishing a broader claim. **Inferred** means a conclusion follows from design/code inspection but lacks a dedicated proof. **Proposed** means future work. **Unknown** means insufficient evidence.

See [architecture](docs/architecture.md), [threat model](docs/threat-model.md), [state machine](docs/state-machine.md), [policy schema](schemas/policy.schema.json), and [claims](docs/claims.md). Those documents limit each claim explicitly.

The Pulpo derivation here is the principle supplied in the referenced discussion: intelligence must remain separate from authority. No Pulpo repository, implementation, or independently validated security properties were supplied or imported.

The demo derives misuse from signed raw session-use signals: an unexpected simulated device plus missing session proof. One anomalous signal causes challenge; expected device plus valid proof is legitimate. Device bindings are seeded as `bound-device-<session ID>` by this lab convention. These are simulation values, not real device attestation or authentication.

Evidence uses `full-v1` and `delta-v1` rows. Decode a journal for inspection with `node src/read-evidence.js <path-to-evidence.jsonl>`. This reader reconstructs data; it does not authenticate the journal without the controller's signing key and trusted head. See [delta logging](docs/delta-logging.md).

The load demo writes `load-report.json`: a concurrent burst, explicit overload refusals, preserved control traffic, a signal admitted through reserved capacity, and dead-man expiry. See [parallel workload handling](docs/parallel-workloads.md). Analysis can use multiple logical processors; no production throughput or speedup is established.

The learning demo works offline with the three bundled official CVE snapshots. It writes `learning-report.json` and a new `runs/learning-*` directory containing the signed dataset, model, evaluation, telemetry export, independent labels, and abstract CVE-inspired local scenario evidence. Run `node src/search-cves.js authentication` to search reference data. To explicitly refresh the small library, run `node src/import-cves.js CVE-2023-4966 CVE-2023-42793 CVE-2024-3094`; this performs bounded read-only HTTPS downloads from the official CVE API. It never follows advisory links or executes exploits.

Read [learning boundaries and results](docs/learning.md) before interpreting the candidate model. The first evaluation missed unfamiliar poisoning families and the model remains advisory; it does not decide training admission or authorize model deployment.
