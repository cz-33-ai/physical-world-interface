# PWI Roadmap

> Planned work is **not** a claim of current implementation. Progress is recorded using issues with acceptance criteria and links to evidence.

## Current baseline — verified in lab (18 November 2026)

- Local Mosquitto publish/subscribe, Zigbee2MQTT and ESP32-C3 Wi-Fi/MQTT test baseline works.
- **Home Assistant OS (ARM64, UTM/QEMU)** booted; browser dashboard reachable.
- HA discovered both existing Zigbee sensor devices; Mosquitto, Zigbee2MQTT and the USB coordinator were **migrated onto the M1 HAOS VM**, removing the M3 runtime dependency. Motion/light state changes and temperature/humidity live values were user-confirmed.
- After a full **HAOS operating-system reboot**, Mosquitto and Zigbee2MQTT Apps automatically restarted and both sensors resumed reporting. A new manual post-migration HAOS backup was created (user-confirmed; off-VM copy and Emergency Kit storage not confirmed).
- A Reolink camera's local live stream works inside a custom HA dashboard; a slow Auto preview improved to near-zero perceived lag with Live mode.
- Midea Auto Cloud exposed a robot-vacuum state/control UI; **Locate Robot command failed** and some attributes were Unknown. Do not claim all controls or mapping work.
- **Not yet validated:** simultaneous MQTT/HA timestamp freshness, offline/unavailable semantics, macOS host restart/sleep/user switching and unattended UTM startup, ESP32-C3 as HA entity, physical-outcome audit and PWI Gateway. A UTM cold start was not repeated in the 18 November migration session; do not confuse this with verified HAOS reboot.
- Public write-ups: [Experiment 04 — initial cross-Mac device hub](04-home-assistant-real-device-integration.md) and [Experiment 05 — consolidated M1 HAOS recovery](05-m1-haos-consolidation-recovery.md).

## M0 — Foundation and project structure

- [x] Establish public documentation repository and private raw lab notebook.
- [x] Define PWI vision, architectural boundaries, publication rules and roadmap.
- [x] Adopt the canonical public repository slug `physical-world-interface`.
- [x] Adopt [Apache License 2.0](../LICENSE) for public PWI code and documentation.
- [x] Establish issue conventions, contribution guide, public issue forms and review checklist.
- [ ] Create first architecture decision record (ADR) for Home Assistant integration.

**Exit:** every subsequent milestone has an issue, a measurable acceptance test and an owner.

## M1 — Unified device inventory and read-only perception

- [x] Deploy HAOS VM and verify **full HAOS OS reboot** restores Mosquitto, Zigbee2MQTT and both sensor readings (separate macOS/UTM unattended recovery still pending).
- [x] Move Mosquitto, Zigbee2MQTT and coordinator into the M1 HAOS VM; reconfigure HA MQTT Integration, verify Discovery and M3 independence.
- [x] Make both existing Zigbee sensor devices visible and observe their live state in HA.
- [ ] Decide whether ESPHome uses MQTT or Native API for the prototype.
- [ ] Validate device offline/unavailable transitions and quantify state freshness/timestamps.

**Exit:** read two device states through one consistent interface and demonstrate change propagation.

## M2 — Controlled actuation and feedback

- [ ] Establish a **repeatable, authorized actuation test** using available safe hardware (a robot-vacuum control was partially exercised; action result still needs verified feedback and failure handling).
- [ ] Command on/off (and brightness if supported) through Home Assistant.
- [ ] Distinguish command dispatch from confirmed state change.
- [ ] Exercise offline/unavailable-device failure and timeout reporting.

**Exit:** reliable action request → dispatch → observable result, including a failure case.

## M3 — Agent-executable environment action

- [ ] Define capability discovery, permissions and action schema.
- [ ] Expose a limited control interface to an external Agent (MCP or API, selected by testing).
- [ ] Implement “Prepare for Sleep” using only connected capabilities, with unavailable features reported explicitly.
- [ ] Support personal preference parameters supplied by the Personal Context Layer.

**Exit:** a test intent produces an auditable plan, authorized actions and per-device result summary.

## M4 — Physical-world context event handoff

- [ ] Define normalized observation and action-outcome events.
- [ ] Connect events to the **existing** Personal Agent OS ingestion boundary.
- [ ] Preserve source, timestamps, confidence/uncertainty and valid state.
- [ ] Demonstrate that physical events can inform later requests without duplicating a memory system inside PWI.

**Exit:** one event from a device and one result from an action are ingestible through a documented contract.

## M5 — Reliability, security, portability

- [x] Create a manual post-migration HAOS backup and keep credentials out of Git (user-confirmed).
- [ ] Verify off-VM recovery archive and Emergency Kit storage; test recovery restoration, not just backup creation.
- [ ] Verify network isolation, least-privilege access and recovery after restart.
- [ ] Add automated tests and CI checks for examples/contracts.
- [ ] Keep services portable to an always-on host.

## Curriculum track (parallel)

Turn **verified** experiments into lessons with concept, architecture, equipment, hands-on steps, expected/actual results, troubleshooting, exercises and grading criteria. See [course plan](course-plan.md).

## Working rule

For each task, keep: **Goal → Design → Implementation → Evidence → Acceptance → Learning → Public Release Review**.


## Current tracked implementation issues

| Work item | GitHub Issue |
|---|---|
| M1: Home Assistant + Mosquitto + Zigbee2MQTT | [#1](https://github.com/cz-33-ai/physical-world-interface/issues/1) |
| M1 research: ESPHome Native API vs MQTT | [#2](https://github.com/cz-33-ai/physical-world-interface/issues/2) |
| M2: First controllable actuator and feedback | [#3](https://github.com/cz-33-ai/physical-world-interface/issues/3) |
| M3: Prepare for Sleep | [#4](https://github.com/cz-33-ai/physical-world-interface/issues/4) |
| M4: Observation and action-outcome contracts | [#5](https://github.com/cz-33-ai/physical-world-interface/issues/5) |


## Reproducibility and community

- [x] Publish an offline, dependency-free simulated demonstration and tests.
- [x] Link a developer [Quick Start](../QUICKSTART.md) from the README.
- [ ] Confirm at least one **independent external reproduction** with feedback.
- [ ] Replace simulator-only claims with a genuine connected-hardware demo after M2.
- [ ] Publish a short, accurate demo video and accompanying instructions once verified.

See [community release gates](community/launch-plan.md) and the [flagship demo plan](demos/prepare-for-sleep.md).
