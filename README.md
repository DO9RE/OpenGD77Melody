# OpenGD77Melody

A tiny, client-side HTML / JAVA-Script tool to convert human-readable melodies into the numeric boot-melody format used by OpenGD77 firmware — and to preview the melody in your browser.



## Purpose

Make it easy for amateur radio users to create and test boot melodies for radios running OpenGD77 firmware. Enter simple note/duration tokens, click "Play & Convert", copy the generated numeric string and paste it into your OpenGD77 CPS.



## Quick usage

1. Open `OpenGD77Melody.html` in a modern browser.

2. Enter a sequence of tokens in the "Human-readable sequence" field.

3. Adjust BPM (playback tempo) and Units per quarter-note (granularity).

4. Click **Play & Convert** to generate the OpenGD77 numeric string and hear a preview.

5. Click **Copy** to copy the numeric string to the clipboard for use with OpenGD77.



## Input format

- Tokens are NAME:DURATION separated by spaces or commas.

- NAME: note (A–G, optional `#` or `b`, and octave number, e.g. `C4`, `F#3`, `Bb2`) or rest `R` / `Rest`.

- DURATION: words (`whole`, `half`, `quarter`, `eighth`, `sixteenth` or `w/h/q/e/s`) or numeric quarter-note units (e.g. `1`, `0.5`, `2`).

- The tool multiplies durations (in quarters) by `unitsPerQuarter` and rounds to the nearest integer to produce the numeric lengths.



## Example

Input:

```

C4:quarter D4:quarter E4:half R:quarter G4:quarter

```

With `unitsPerQuarter = 6` the output will be:

```

60,6,62,6,64,12,0,6,67,6

```

(`60,62,64,67` = MIDI note numbers for C4/D4/E4/G4; `0` = rest)



## Notes

- Playback (BPM) is only for preview and does not affect the numeric output.

- Malformed tokens are ignored; check the status area for parsing feedback.

- Default suggested `unitsPerQuarter` is 6 for good granularity with OpenGD77.



## MIDI import

- Quick overview  
  Upload a standard SMF (.mid) file, select one track, and click "Import track". The chosen track is converted to the human-readable token format (NAME:DURATION) and inserted into the textarea for manual editing.

- Intended use & important rules  
  - This importer is designed for monophonic tracks only (OpenGD77 plays one note at a time). For reliable results, export and trim a single melodic track from your DAW before importing.  
  - If multiple notes start at the same tick, the importer keeps only the lowest (deepest) MIDI note and ignores the rest. Velocity and other MIDI controllers are ignored.  
  - The textarea shows the original pitches as found in the MIDI file (so you can edit in familiar pitch). The numeric OpenGD77 output (and the playback preview) is automatically transposed down by two octaves (−24 semitones) when you press “Play & Convert”.

- How durations are handled  
  - The parser reads note start/end ticks and uses the file’s ticks-per-quarter value to convert durations into quarter-note units (1 = quarter). Those quarter values are then multiplied by the UI’s `unitsPerQuarter` and rounded to produce integer length units for the OpenGD77 numeric output.  
  - The implementation enforces a minimum numeric length of 1 unit (no 0-length pairs are produced).

- Limitations & tips  
  - The built-in SMF parser is lightweight — it handles Note On/Off, running status and basic meta events (track name, end-of-track), but it is not a full-featured MIDI library. Unusual or heavily edited MIDI files may produce suboptimal results.  
  - If your track contains chords or overlapping polyphony, prepare a monophonic version (mute other instruments or export a single monophonic line) before import.  

## Author

Richard Emling — DO9RE



