# os-webrtc-janus TODO (EN/DE)

Es wird ausschließlich im Verzeichnis addon-modules\os-webrtc-janus bearbeitet.

This list is derived from the main findings in the addon review.
Diese Liste basiert auf den Hauptbefunden der Addon-Prüfung.

## P0 — Critical / Kritisch

- [x] **Implement `VoiceViewerSession` members instead of throwing**  
  **EN:** Replace `NotImplementedException` in `VoiceServiceSessionId` and `Shutdown()` with real behavior.  
  **DE:** `NotImplementedException` in `VoiceServiceSessionId` und `Shutdown()` durch echte Implementierung ersetzen.  
  **Done when / Fertig wenn:** No runtime throw in normal session lookup/disconnect paths.

- [x] **Fix swapped constructor arguments in service connector**  
  **EN:** In `WebRtcVoiceServiceConnector.CreateViewerSession`, pass `(regionId, agentId)` in the correct order.  
  **DE:** In `WebRtcVoiceServiceConnector.CreateViewerSession` die Parameterreihenfolge auf `(regionId, agentId)` korrigieren.  
  **Done when / Fertig wenn:** `RegionId` and `AgentId` in created sessions are correct.

- [x] **Ensure `m_nonSpatialVoiceService` is initialized for same-module config**  
  **EN:** If `SpatialVoiceService == NonSpatialVoiceService`, assign `m_nonSpatialVoiceService = m_spatialVoiceService`.  
  **DE:** Wenn `SpatialVoiceService == NonSpatialVoiceService`, `m_nonSpatialVoiceService = m_spatialVoiceService` setzen.  
  **Done when / Fertig wenn:** Non-spatial path cannot hit null reference in session creation.

## P1 — High / Hoch

- [x] **Make outstanding-request tracking thread-safe**  
  **EN:** Protect `_OutstandingRequests` consistently (or move to `ConcurrentDictionary`) for add/remove/read paths.  
  **DE:** `_OutstandingRequests` in allen Add/Remove/Read-Pfaden konsistent thread-safe machen (oder `ConcurrentDictionary` nutzen).  
  **Done when / Fertig wenn:** No race-prone unsynchronized dictionary access remains.

- [x] **Reduce sync-over-async blocking (`.Result`) in hot paths**  
  **EN:** Remove/contain `.Result` usage in startup/request paths and prefer async flow to prevent thread starvation/deadlock risk.  
  **DE:** `.Result` in Start-/Request-Pfaden entfernen/einhegen und asynchronen Ablauf bevorzugen, um Blockierungen/Deadlocks zu vermeiden.  
  **Done when / Fertig wenn:** Critical request flow does not block on `.Result`.

- [x] **Return actual signaling response to viewer**  
  **EN:** In `VoiceSignalingRequest`, serialize and return service response instead of always returning `<undef/>`.  
  **DE:** In `VoiceSignalingRequest` die echte Service-Antwort serialisieren/zurückgeben statt immer `<undef/>`.  
  **Done when / Fertig wenn:** Viewer receives signaling payload relevant to ICE negotiation.

## P2 — Medium / Mittel

- [x] **Handle single ICE candidate messages**  
  **EN:** In Janus signaling, process `candidate` payloads (not only `completed=true` and `candidates[]`).  
  **DE:** In Janus-Signaling auch einzelne `candidate`-Payloads verarbeiten (nicht nur `completed=true` und `candidates[]`).  
  **Done when / Fertig wenn:** Single-candidate trickle path forwards candidates to Janus.

- [x] **Fix `LeaveRoom()` return semantics**  
  **EN:** Return `true` on successful leave response (and validate response state).  
  **DE:** Bei erfolgreicher Leave-Response `true` zurückgeben (inkl. Response-Prüfung).  
  **Done when / Fertig wenn:** Caller can reliably detect successful room leave.

- [x] **Correct broken log string interpolation/placeholders**  
  **EN:** Fix log lines with literal placeholders (`{LogHeader}`, `{0}`, `{1}`) so diagnostics are usable.  
  **DE:** Log-Zeilen mit literalen Platzhaltern (`{LogHeader}`, `{0}`, `{1}`) korrigieren, damit Diagnosen verwertbar sind.  
  **Done when / Fertig wenn:** Log output shows intended values in affected paths.

## Optional follow-up / Optionaler Folgeschritt

- [ ] **Add targeted regression tests (if test harness available)**  
  **EN:** Add focused tests for viewer session creation, signaling response, and candidate handling.  
  **DE:** Gezielte Tests für Viewer-Session-Erstellung, Signaling-Response und Candidate-Handling ergänzen.
