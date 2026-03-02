# os-webrtc-janus – Changelog Snippet

## DE

### Highlights

- Stabilisierung des Janus/WebRTC-Session-Lifecycles (Connect, Crossing, Logout).
- Deutlich reduziertes Log-Rauschen im Normalbetrieb bei erhaltenem Diagnosewert.
- Vollständige Abarbeitung der TODO-Punkte (P0/P1/P2 + optionaler Regressionstest-Plan).

### Behobene Probleme

- `VoiceViewerSession`-Member vollständig implementiert (kein `NotImplementedException` mehr in regulären Pfaden).
- Vertauschte Parameter in der Session-Erzeugung korrigiert.
- `m_nonSpatialVoiceService` bei Same-Module-Konfiguration korrekt initialisiert.
- Thread-Safety für Janus-Outstanding-Requests hergestellt (`ConcurrentDictionary`).
- Sync-over-async-Hotspots entschärft (kritische `.Result`-Pfadnutzung entfernt/eingehegt).
- `VoiceSignalingRequest` liefert jetzt die echte Service-Antwort an den Viewer.
- ICE-Trickle verbessert: Array- und Single-Candidate-Pfad unterstützt.
- `LeaveRoom()`-Erfolgspfad korrigiert (`left`/`event`/`success`/`ack` werden korrekt behandelt).

### Logging/Operations

- Disconnect-Handling dedupliziert: pro Session nur ein primärer Disconnect-Infoeintrag.
- Hangup-Logs im Normalmodus kompakt (`sessionId`, `reason`), Raw-JSON nur bei `MessageDetails=true`.
- Erwartbare Teardown-Meldungen (`Handle not found`, `task canceled`, `long poll exit`) auf Detailmodus begrenzt.

### Validierung

- Mehrfache erfolgreiche Builds der betroffenen Projekte (`WebRtcVoice`, `WebRtcVoiceRegionModule`, `WebRtcJanusService`).
- Laufzeit-Logs aus Login/Crossing/Logout geprüft; keine regressiven Fehlerbilder im Voice-Flow.
- Reproduzierbarer manueller Testplan vorhanden: `REGRESSION_TEST_PLAN.md`.

## EN

### Highlights (EN)

- Stabilized Janus/WebRTC session lifecycle (connect, crossing, logout).
- Significantly reduced normal-operation log noise while preserving diagnostics.
- Completed all TODO items (P0/P1/P2 + optional regression test plan).

### Fixes (EN)

- Fully implemented `VoiceViewerSession` members (no `NotImplementedException` in normal paths).
- Corrected swapped parameters in viewer session creation.
- Ensured proper `m_nonSpatialVoiceService` initialization for same-module configuration.
- Made Janus outstanding request tracking thread-safe (`ConcurrentDictionary`).
- Reduced sync-over-async risk in hot paths (contained/removed critical `.Result` usage).
- `VoiceSignalingRequest` now returns actual signaling payload to the viewer.
- Improved ICE trickle handling: both array and single-candidate flows supported.
- Fixed `LeaveRoom()` success semantics (`left`/`event`/`success`/`ack` handled as expected).

### Logging/Operations (EN)

- Deduplicated disconnect handling: one primary disconnect info entry per session.
- Compact hangup logs in normal mode (`sessionId`, `reason`), raw payload only with `MessageDetails=true`.
- Expected teardown logs (`Handle not found`, `task canceled`, `long poll exit`) limited to detail mode.

### Validation (EN)

- Repeated successful builds of affected projects (`WebRtcVoice`, `WebRtcVoiceRegionModule`, `WebRtcJanusService`).
- Runtime logs from login/crossing/logout verified; no regressive voice-flow error pattern observed.
- Reproducible manual test checklist available: `REGRESSION_TEST_PLAN.md`.
