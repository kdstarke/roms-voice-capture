# ROMS iPad microphone repair

The reported iPad log shows audio input beginning, followed immediately by a speech-recognition `aborted` event. The existing page opens a separate getUserMedia stream, stops its tracks, then starts speech recognition. This handoff is a possible contributor; the log alone does not establish the underlying browser cause.

The repair starts speech recognition directly from the user's tap, gives each retry a fresh recognizer, prevents overlapping starts, retains the transcript across interruptions, and preserves the exact error instead of overwriting it with an idle state. Clear cancels the current recognizer and ignores its late events. When a recording ends, the user chooses Review or taps the microphone to continue. The existing enrollment and submission functions are unchanged.

Validation: `node test-microphone.cjs` passes 10 behavioral checks with a mocked browser speech engine. These cover direct starts, overlapping taps, aborted recordings, stale events, duplicate speech events, manual retries, unsupported browsers, enrollment retention, and transcript preservation. JavaScript parses successfully. No hardware microphone, live iPad recording, or end-to-end task save has been tested by these checks.

After publication, reload the existing GitHub voice page on the iPad. Verify the device name, record one sentence, stop, tap again to append another sentence, then review the retained wording. If recording still aborts, collect the new Voice System Status details; permission, browser, or operating-system interruption remains possible.

The separate Google Apps Script sign-in error and the previously pending voice-save/branding updates are outside this microphone repair.
