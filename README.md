# Robovox IPA Translator

**Version 1.1** — A browser-based tool that converts English text into IPA, maps IPA to Robovox SC-02 phonemes, and sends the sequence as MIDI notes to a connected synth or exports it as a `.mid` file.

Live page: https://deladriere.github.io/Robovox_translator/

![Robovox IPA Translator interface](interface.png)

## What It Does

- Converts typed text to an approximate IPA transcription.
- Maps IPA symbols to Robovox SC-02 phoneme tokens.
- Shows consonants as blue badges and vowels as red dropdowns (for quick vowel swapping).
- Builds per-phoneme controls for:
  - duration (ms)
  - velocity
  - pitch (MIDI CC2)
- Sends the phoneme sequence over Web MIDI, one note per phoneme.
- **Download MIDI** saves a Standard MIDI File (`Robovox.mid`) for use in a DAW; no MIDI device is required. Export uses the same per-phoneme duration, velocity, and CC2 pitch as the on-screen controls (timing is based on 120 BPM in the file).

## Project Structure

- `index.html`: single-file app (UI, mapping tables, MIDI logic, controls).

## How To Run

1. Open `index.html` in a modern browser with Web MIDI support (Chrome/Edge recommended), or visit the GitHub Pages site URL if the repo has Pages enabled.
2. Connect your MIDI output:
   - Click **Connect MIDI Device**
   - Select your output device in the dialog
   - Click **Connect**
3. Type text in the input field.
4. Optionally adjust phoneme duration/velocity/pitch controls.
5. Output the sequence:
   - Click **Send MIDI Notes** (requires a connected MIDI device).
   - Or click **Download MIDI** to save `Robovox.mid` (works without a device).
   - Or use the keyboard shortcut below for live send.

## Keyboard Controls

- `Space`: send/play the phoneme MIDI sequence.
  - Works when the text input is **not focused**.
  - If the text input is focused, Space inserts a normal space character.

## Notes On Mouse-Free Use

- Playback itself can be launched with `Space`.
- Initial MIDI connection currently uses a click-based device picker dialog, so full mouse-free setup is not fully implemented in the current UI.

## Implementation Notes

- The app includes two conversion stages:
  - `textToIPA(...)`: dictionary + simple fallback rules.
  - IPA to SC-02 mapping via `ipaToSC02`.
- During MIDI send:
  - CC2 pitch is sent first.
  - Then Note On.
  - Then Note Off after the phoneme duration.
- MIDI file export mirrors that order (CC2, Note On, Note Off) in a single-track SMF; tempo meta event is 120 BPM.
- A `PA0` phoneme is inserted between words for separation.
