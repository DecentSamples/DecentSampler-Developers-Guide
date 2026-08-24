The &lt;arpeggiator&gt; element
=================================

As of 1.26.0, DecentSampler has a built-in arpeggiator. Unlike the [note sequencer](the-noteSequences-element), which plays back a fixed, pre-authored pattern, the arpeggiator reacts live to whichever notes are currently held down: hold a chord and it generates a stream of notes from that chord, in the order and octave range you choose, for as long as you hold it.

The `<arpeggiator>` element is how you configure the arpeggiator. There should be exactly one `<arpeggiator>` element in each `<DecentSampler>` file, as a direct child of the root `<DecentSampler>` element (a sibling of `<groups>`, `<midi>`, `<noteSequences>`, etc.):

```xml
<DecentSampler>
  <groups>...</groups>
  <arpeggiator enabled="true"
               arpOrder="up"
               arpOctaveRange="2"
               arpOctaveMode="replayPerOctave"
               arpStepCount="16"
               arpGateLength="0.75"
               arpFollowGlobalTempo="true"
               arpSyncDivision="noteOneSixteenth"
               arpRateMultiplier="1.0"
               arpOverrideBpm="120.0"/>
</DecentSampler>
```

Every attribute on `<arpeggiator>` can also be set from a knob, button, menu, or modulator (LFO/MIDI CC) using the `arpeggiator` binding type — see [Appendix B](appendix-b-the-binding-element) and the [how-to guide](topic-how-to-use-the-arpeggiator) for details.

## Attributes

- `enabled` (optional): Whether the arpeggiator is armed. When armed, it takes over **every** note played anywhere on the keyboard — the raw notes are not also passed through to the sample playback engine. Valid values are `true` and `false`. Default: `false`.
- `arpOrder` (optional): The order in which held notes (and their octave-expanded copies) are played. See [Order Modes](#order-modes) below for the full list and worked examples. Default: `up`.
- `arpOctaveRange` (optional): How many octaves the held notes are spread across, counting the octave you're actually holding. `1` means no octave duplication at all — just the notes you're holding. Whole number from 1 to 8. Default: `1`.
- `arpOctaveMode` (optional): How the octave-expanded copies are woven into the pattern. `replayPerOctave` repeats the held-note pattern once per octave (the octave you're holding, then the whole pattern again an octave up, and so on) — this is how most hardware/software arpeggiators behave. `interleaveByPitch` instead merges every note across every octave into one pitch-sorted list before applying the order. Default: `replayPerOctave`.
- `arpStepCount` (optional): A hard ceiling on how many notes make up one cycle of the pattern, from 1 to 16. If the held chord × octave range would produce more notes than this, the pattern is truncated to the first `arpStepCount` notes (built according to `arpOctaveMode`) — it is never padded. Default: `16`.
- `arpGateLength` (optional): How long each note rings out, as a fraction of one step's duration. `0.5` means each note is cut roughly halfway through the step, leaving a gap before the next one; `1.0` means the note rings for the full step and (depending on the sample's own release/loop behavior) may audibly overlap with the next note. Floating point number, 0 or greater. Default: `0.75`.
- `arpFollowGlobalTempo` (optional): Whether the arpeggiator's rate tracks the host/DAW's tempo. When `true`, the rate is computed from the current tempo and `arpSyncDivision`. When `false`, the rate is computed from `arpOverrideBpm` and `arpSyncDivision` instead, ignoring whatever the host reports. Valid values are `true` and `false`. Default: `true`.
- `arpSyncDivision` (optional): The musical note value of one step, e.g. `noteOneSixteenth` for a 16th-note arpeggio. Valid values: `noteOneSixtyFourthTriplet`, `noteOneSixtyFourth`, `noteOneThirtySecondTriplet`, `noteOneSixtyFourthDotted`, `noteOneThirtySecond`, `noteOneSixteenthTriplet`, `noteOneThirtySecondDotted`, `noteOneSixteenth`, `noteOneEighthTriplet`, `noteOneSixteenthDotted`, `noteOneEighth`, `noteOneFourthTriplet`, `noteOneEighthDotted`, `noteOneFourth`, `noteOneHalfTriplet`, `noteOneFourthDotted`, `noteOneHalf`, `noteOneTriplet`, `noteOneHalfDotted`, `noteWhole`, `noteWholeDotted`. Default: `noteOneSixteenth`.
- `arpRateMultiplier` (optional): A continuous multiplier applied on top of `arpSyncDivision`'s rate — higher values play faster. This is the attribute you'd bind a knob or an LFO to if you want a live, sweepable rate control rather than snapping between discrete note values. Floating point number, 0.01 to 100. Default: `1.0`.
- `arpOverrideBpm` (optional): The tempo used to compute the rate when `arpFollowGlobalTempo="false"`. Floating point number, 1 to 1000. Default: `120.0`.

## Order Modes

Given the chord C4-E4-G4 (MIDI notes 60, 64, 67) with `arpOctaveRange="2"` and `arpOctaveMode="replayPerOctave"`, the expanded note pool is **60, 64, 67, 72, 76, 79** (the chord, then the same chord an octave up). Here's what each `arpOrder` value produces:

| `arpOrder` | One full cycle |
|:---|:---|
| `up` | 60, 64, 67, 72, 76, 79 → loops back to 60 |
| `down` | 79, 76, 72, 67, 64, 60 → loops back to 79 (the exact reverse of `up`) |
| `up_down` | 60, 64, 67, 72, 76, **79**, 76, 72, 67, 64 → loops back to 60 (top/bottom note plays once per cycle) |
| `up_down_inclusive` | 60, 64, 67, 72, 76, 79, **79**, 76, 72, 67, 64, **60** → loops back to 60 (top/bottom note repeats at each turnaround) |
| `down_up` | 79, 76, 72, 67, 64, 60, 64, 67, 72, 76 → loops back to 79 |
| `down_up_inclusive` | 79, 76, 72, 67, 64, 60, 60, 64, 67, 72, 76, 79 → loops back to 79 |
| `as_played` | Whatever order you actually pressed the notes in, replayed per octave. If you pressed G, then C, then E, this plays 67, 60, 64, 79, 72, 76. (If you happened to press them low-to-high, this is identical to `up`.) |
| `random` | A note is picked at random from the pool on every step. The same note can repeat back-to-back. |
| `random_no_repeat` | Same as `random`, but the same note never plays twice in a row. |

## Example

This example arms the arpeggiator at 16th notes, tracking the host tempo, spread across 2 octaves:

```xml
<DecentSampler>
  <groups>
    <group>
      <sample path="Samples/Piano-C4.wav" rootNote="60" loNote="0" hiNote="127"/>
    </group>
  </groups>
  <arpeggiator enabled="true"
               arpOrder="up_down_inclusive"
               arpOctaveRange="2"
               arpOctaveMode="replayPerOctave"
               arpGateLength="0.6"
               arpFollowGlobalTempo="true"
               arpSyncDivision="noteOneSixteenth"/>
</DecentSampler>
```

For a full discussion of how to bind knobs and modulators to the arpeggiator, see the tutorial on [how to use the arpeggiator](topic-how-to-use-the-arpeggiator).
