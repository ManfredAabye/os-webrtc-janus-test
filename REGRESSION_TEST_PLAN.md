# os-webrtc-janus Regression Test Plan

## Scope

Focused runtime regression checks for the recent fixes:

- Session creation and teardown stability
- Signaling response passthrough
- ICE trickle handling (array + single candidate)
- Leave/teardown log noise reduction

## Preconditions

- Build succeeded for:
  - `addon-modules/os-webrtc-janus/WebRtcVoice/WebRtcVoice.csproj`
  - `addon-modules/os-webrtc-janus/WebRtcVoiceRegionModule/WebRtcVoiceRegionModule.csproj`
  - `addon-modules/os-webrtc-janus/Janus/WebRtcJanusService.csproj`
- Janus reachable and configured in `JanusWebRtcVoice`
- `WebRtcVoice.Enabled = true`

## Test 1: Initial voice connect

1. Start grid/services and log in with a viewer.
2. Trigger voice connect in a local parcel.

Expected log indicators:

- `Handle_ProvisionVoiceAccountRequest`
- `JanusViewerSession created ...`
- `CreateSession. Created. ID=...`
- `AudioBridgePluginHandle created`
- `ProvisionVoiceAccountRequest: connected ...`
- `VoiceSignalingRequest: <n> candidates`
- `VoiceSignalingRequest: candidate completed`

## Test 2: Region crossing with active voice

1. Stay in voice and cross to a neighboring region.
2. Observe old-session teardown and new-session setup.

Expected log indicators:

- Exactly one `ProvisionVoiceAccountRequest: disconnected by logout ...` per crossing for old session
- New session logs (`JanusViewerSession created`, `CreateSession. Created`, `connected ...`)

Must NOT appear:

- `LeaveRoom. Failed ... janus=ack`
- Repeated error spam for normal teardown path

Acceptable debug behavior:

- `LeaveRoom. Ack accepted ...` can appear
- `DestroySession: Handle not found` should only appear when `MessageDetails=true`
- `EventLongPoll: Task canceled` / `Exiting long poll loop` should only appear when `MessageDetails=true`

## Test 3: Logout while connected to voice

1. Log out while voice is active.
2. Ensure teardown is clean and no false errors are logged.

Expected:

- One disconnect info line
- Session shutdown/detach/destroy sequence
- No `LeaveRoom. Failed` for ack/event/left success cases

## Test 4: Candidate variants

1. Verify standard `candidates[]` trickle flow.
2. Verify single `candidate` payload flow (if viewer/network path emits single candidate messages).

Expected:

- Array flow: `VoiceSignalingRequest: <n> candidates`
- Single flow: `VoiceSignalingRequest: single candidate`
- Completion flow: `VoiceSignalingRequest: candidate completed`

## Pass criteria

- All four tests complete without new error-level logs in normal connect/crossing/logout flow.
- Voice remains connected after crossing and reconnects reliably.
- No regressions in signaling behavior.
