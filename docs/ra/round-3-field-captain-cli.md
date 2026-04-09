# Round 3: field-captain-cli

---

# field-captain: The Technician's Instrument

## I. The Philosophy of the Tool

A technician's tool is not an interface. It is not a dashboard. It is not a "user experience." A technician's tool is a **hand** — an extension of cognition into material reality. When a carpenter reaches for a chisel, she does not think "I am now operating a chisel." The chisel becomes transparent. The wood is what she touches. Field-captain must achieve this transparency or it is waste.

This means: no spinners that say "processing." No progress bars without meaning. No abstractions that distance the technician from the machine standing in front of them. Every byte on screen must be a byte the technician *acts on*. Everything else is noise, and noise on a rolling deck at 0200 in the rain is not annoyance — it is danger.

Field-captain is a CLI because the terminal is the only honest computing environment. It shows you what is there. It does not render. It does not animate. It presents state and accepts command. This is the correct relationship between a technician and their instrument: declarative, stateful, immediate.

---

## II. Architecture: Local-First, Sync-When-Able

```
field-captain/
├── bin/fc                    # the executable
├── lib/
│   ├── core/                 # deploy, diagnose, monitor, update, rollback
│   ├── fleet/                # list, status, health, logs
│   ├── equip/                # camera, sensor, motor, comms
│   ├── customer/             # report, document, sign-off
│   ├── session/              # git-backed session logging
│   ├── journal/              # accumulated technician wisdom
│   └── yellow-wire/          # the unhandleable handler
├── store/                    # local sqlite + git repo
│   ├── fleet.db              # local fleet state cache
│   ├── sessions/             # git repo of all sessions
│   ├── journal/              # markdown journal entries
│   └── yellow-wire/          # incident reports for the unhandleable
├── templates/                # report templates, checklists
└── fc.toml                   # configuration
```

The store is a **local SQLite database plus a git repository**. Every session is a commit. Every diagnostic run is a commit. The journal is a commit. When connectivity exists, `fc sync` pushes to the shared repository. When it doesn't — which is most of the time on a vessel — everything lives locally. The technician never waits for a cloud. The cloud is a *backup*, not a prerequisite.

The `fc.toml` configuration file:

```toml
[identity]
technician_id = "tcn-0441"
name = "M. Vasquez"
certification = "cocapn-level-2"

[deckboss]
url = "https://deckboss.cocapn.internal"
sync_interval = "on-connect"  # never automatic

[fleet]
default_vessel = "msv-horizon"
cache_ttl = "24h"

[session]
auto_commit = true
require_notes = true        # cannot close session without notes
photo_required = true       # before/after for every intervention

[offline]
max_local_age = "72h"       # warn if store older than this
emergency_mode = false      # flag for extreme situations

[yellow_wire]
enabled = true
notify_deckboss = "on-sync" # don't try to call home
incident_template = "standard"
```

---

## III. Core Commands: The Five Verbs of Intervention

### `fc deploy <target> <config>`

Deploys a configuration to a target system. But "deploy" in the field is not what it is in devops. There is no CI/CD pipeline. There is a technician with a laptop and a machine that needs to become a different machine.

```bash
$ fc deploy msv-horizon/cam-fore config:navcam-v3.2

DEPLOY: msv-horizon/cam-fore
  config: navcam-v3.2
  source: local cache (verified sha256:a3f2...c891)
  target state: running
  
Pre-deploy checks:
  ✓ Target reachable (latency: 12ms)
  ✓ Current config: navcam-v3.1 (rollback available)
  ✓ Sensor calibration current (3 days old)
  ✓ No active alerts on target
  ⚠ Motor firmware 2.1.4 — recommended 2.1.5 (non-blocking)

Proceed? [y/n/e(edit)] y

Deploying... 
  [====>          ] Writing config to device
  [========>      ] Restarting sensor daemon
  [============>  ] Validating stream
  [===============] COMPLETE

Post-deploy validation:
  ✓ Stream active at 30fps
  ✓ Latency within spec (45ms avg)
  ✓ No new alerts generated
  ✓ Rollback snapshot: snap-20240315-0241

Session commit: deploy/cam-fore/navcam-v3.2 @ 2024-03-15T02:41:33Z
```

The key design principle: **every deploy creates a rollback snapshot automatically.** The technician never deploys without a way back. This is not a feature. It is an ethical obligation.

### `fc diagnose <target> [symptom]`

Diagnosis is the core act of the technician. It is where the ontology of maintenance lives — the encounter between the designed and the actual.

```bash
$ fc diagnose msv-horizon/cam-fore "intermittent frame drop in heavy seas"

DIAGNOSE: msv-horizon/cam-fore
  Symptom: intermittent frame drop in heavy seas
  Started: unknown (first reported 2024-03-14)

Gathering telemetry...
  ✓ Last 24h logs (847 entries, 3 anomalies)
  ✓ Hardware status (all nominal — suspect)
  ✓ Environmental correlation (sea state > 4)
  ✓ Cable integrity (last test: 6 days ago — STALE)
  ✓ Power supply logs (voltage dip detected at 03:22)

Pattern match against journal:
  → Similar issue on msv-atlas/cam-aft (2024-01-22)
    Resolution: "vibration loosening on connector J7. 
     Reseated with threadlock. Recurring — recommend 
     cable clamp retrofit."
    Confidence: 72%

Hypothesis queue:
  1. [72%] Connector vibration (J7 or J12) — check physical
  2. [31%] Power supply intermittent — check PSU rails under load
  3. [18%] Firmware race condition — check daemon logs for mutex

Recommended next steps:
  1. fc equip camera check msv-horizon/cam-fore --physical
  2. fc equip sensor calibrate msv-horizon/cam-fore --under-load
  3. fc logs msv-horizon/cam-fore --filter "mutex|timeout" --last 48h

Add to session? [y/n/j(journal)] y
```

The `--physical` flag is critical. It tells field-captain: "I am going to look at the physical machine now. Prompt me for what to check." This is the tool acknowledging that diagnosis is not purely informational. It is *embodied*.

### `fc monitor <target> [duration]`

```bash
$ fc monitor msv-horizon/cam-fore 30m

MONITOR: msv-horizon/cam-fore (30 min window)
  02:41:33 — Stream: 30fps | Latency: 45ms | Temp: 38°C | Vibration: 0.3g
  02:41:43 — Stream: 30fps | Latency: 44ms | Temp: 38°C | Vibration: 0.3g
  02:42:03 — Stream: 28fps | Latency: 67ms | Temp: 39°C | Vibration: 1.2g  ←
  02:42:04 — Stream: 30fps | Latency: 46ms | Temp: 39°C | Vibration: 0.4g
  ...
  02:55:12 — Stream: 22fps | Latency: 89ms | Temp: 41°C | Vibration: 2.1g  ←←
  02:55:13 — Stream: 30fps | Latency: 48ms | Temp: 41°C | Vibration: 0.8g

Correlation detected: frame drops coincide with vibration > 1.0g
  Pearson r = 0.84 (p < 0.001)
  
  This supports hypothesis: connector vibration

Save correlation to session? [y/n] y
```

Monitor doesn't just display. It **observes with the technician.** It correlates. It notices what the technician might miss because they are also watching the physical machine, feeling the deck move, listening to the motor. The tool and the technician form a dual observation system.

### `fc update <target> [version]`

Update is the dangerous verb. It changes the machine's identity. So it demands confirmation, shows exactly what will change, and always — *always* — creates a rollback path.

```bash
$ fc update msv-horizon/cam-fore 3.2.1

UPDATE: msv-horizon/cam-fore
  Current: navcam-v3.2 (deployed 2024-03-15T02:41:33Z)
  Target:  navcam-v3.2.1

Changelog (3.2 → 3.2.1):
  • FIX: mutex race on stream buffer under high vibration
  • FIX: memory leak in long-run diagnostic daemon
  • CHG: vibration threshold adjusted (1.0g → 0.8g)

⚠ This update is RELEVANT to your current diagnosis.
  The mutex race fix may address hypothesis #3.
  The vibration threshold change may affect monitoring.

Pre-update requirements:
  ✓ Target reachable
  ✓ Rollback snapshot available (snap-20240315-0241)
  ⚠ Target is under active diagnosis — recommend resolving first

Force update? [y/n/a(abort)] 
```

The tool knows the context. It knows there's an active diagnosis. It warns. It does not prevent — the technician is the authority — but it ensures the technician *knows*.

### `fc rollback <target> [snapshot]`

```bash
$ fc rollback msv-horizon/cam-fore

ROLLBACK: msv-horizon/cam-fore
  Current: navcam-v3.2.1 (deployed 4 min ago)
  Revert to: navcam-v3.2 (snapshot: snap-20240315-0241)

Reason for rollback (required): 
> vibration threshold change caused false alerts in current sea state

Rolling back...
  [===============] COMPLETE
  
Post-rollback validation:
  ✓ Stream active at 30fps
  ✓ Config matches snapshot
  ✓ No new alerts

Rollback reason logged to session and journal.
```

Rollback requires a reason. Not to punish the technician. To *learn*. Every rollback is data about the boundary between design intent and operational reality.

---

## IV. Fleet Management: Seeing the Whole

```bash
$ fc fleet list --vessel msv-horizon

FLEET: msv-horizon (12 systems)
  
  SYSTEM          TYPE        STATUS    CONFIG         LAST CHECK
  cam-fore        navcam      ● OK      v3.2           12 min ago
  cam-aft         navcam      ● OK      v3.2           2 days ago
  cam-bridge      navcam      ⚠ WARN    v3.1.4         2 days ago
  sonar-fwd       sonar       ● OK      v2.8           1 week ago
  sonar-aft       sonar       ● OK      v2.8           1 week ago
  radar-primary   radar       ● OK      v4.1           3 days ago
  radar-secondary radar       ○ OFFLINE v4.1           3 days ago ←
  ais-receiver    comms       ● OK      v1.2           1 week ago
  gps-primary     nav         ● OK      v5.0           1 week ago
  gps-secondary   nav         ● OK      v5.0           1 week ago
  imu-main        sensor      ● OK      v3.3           2 days ago
  engine-monitor  sensor      ⚠ WARN    v2.1           6 hours ago

● OK: 9  ⚠ WARN: 2  ○ OFFLINE: 1  ✗ ERROR: 0
```

```bash
$ fc fleet health --vessel msv-horizon

HEALTH: msv-horizon
  
  Overall: 83% (degraded)
  
  Active concerns:
    ⚠ cam-bridge: firmware v3.1.4 — two versions behind
      → Journal: v3.2 fixes intermittent sync loss on this model
    ⚠ engine-monitor: temperature sensor reading high
      → Last calibration: 47 days ago (overdue)
    ○ radar-secondary: not responding
      → Last seen: 3 days ago
      → Likely: power supply failure (see incident inc-2024-0291)
  
  Maintenance window in: 6 days (per schedule)
  Recommended priority: radar-secondary > engine-monitor > cam-bridge
```

The fleet health command is where the **journal** becomes powerful. It doesn't just show status — it shows status *informed by accumulated technician wisdom*. "v3.2 fixes intermittent sync loss on this model" — that came from another technician's journal entry, synced when connectivity was last available. The tool speaks across time and vessels.

---

## V. Equipment Testing: The Physical Conversation

```bash
$ fc equip camera check msv-horizon/cam-fore --physical

CAMERA CHECK: msv-horizon/cam-fore (physical mode)

Automated checks:
  ✓ Stream resolution: 1920x1080 @ 30fps
  ✓ Color calibration: within ΔE < 2
  ✓ Exposure range: 0.01ms - 33ms
  ✓ Auto-focus functional
  
Physical checks (prompted):
  
  1. Inspect lens housing for condensation
     Status? [ok/condensation/damage/other]: ok
  
  2. Check connector J7 for seating
     Status? [seated/loose/corroded/other]: loose
     ⚠ LOOSE DETECTED — this matches diagnosis hypothesis #1
     
     Action: Reseat connector. Apply threadlock if available.
     Completed? [y/n]: y
     Photo required (before/after):
     > fc photo msv-horizon/cam-fore --tag "J7-reseat-before"
     > fc photo msv-horizon/cam-fore --tag "J7-reseat-after"
  
  3. Check connector J12 for seating
     Status? [seated/loose/corroded/other]: seated
  
  4. Inspect cable run for chafing
     Status? [ok/chafing/worn/other]: ok
  
  5. Check mounting bolts (torque spec: 8 Nm)
     Torque reading: 7.2
     ⚠ BELOW SPEC — recommend tightening
     
     Action: Tighten to 8 Nm. Note if bolts resist or strip.
     Completed? [y/n]: y
     Notes: > bolts took torque normally, no thread damage

Physical check complete. 2 issues found and addressed.
  → Connector J7: reseated with threadlock
  → Mounting bolts: retorqued to spec

Add to session? [y/n/j(journal)] j
  → Added to session AND journal (pattern: connector vibration)
```

The `--physical` flag transforms the tool from an information display into a **checklist partner