# How to Use FM Synthesis (fm6op)

DecentSampler includes a built-in 6-operator FM synthesizer that implements the 32 classic algorithm topologies from the Yamaha DX7. You can use it alongside traditional samples, as a pure synthesizer, or anywhere else an `<oscillator>` element is valid.

## Quick Start

Set `waveform="fm6op"` on an `<oscillator>` element and configure the FM parameters on the parent `<group>`:

```xml
<group fmAlgorithm="5"
       fmOp1Ratio="1.0"  fmOp1Level="0.9"
       fmOp2Ratio="14.0" fmOp2Level="0.5"
       fmOp2Attack="0.001" fmOp2Decay="0.4" fmOp2Sustain="0.0" fmOp2Release="0.1"
       fmOp6Feedback="0.0"
       attack="0.001" decay="0.5" sustain="0.0" release="1.5">
  <oscillator waveform="fm6op"
              loNote="0" hiNote="127" rootNote="60"
              loVel="0" hiVel="127" volume="1.0"/>
</group>
```

## Concepts

### Operators

The fm6op oscillator has six *operators* numbered 1–6. Each operator is a sine-wave oscillator with:

- A **frequency ratio** (`fmOpNRatio`) — multiplies the played note's frequency.  
  `1.0` = fundamental, `2.0` = one octave up, `0.5` = one octave down.
- An **output level** (`fmOpNLevel`) — how much the operator contributes (0.0–1.0).
- A **feedback amount** (`fmOpNFeedback`) — how much the operator feeds its own output back into its phase input (0.0–1.0). In the DX7 architecture only one operator per algorithm physically carries the feedback path; see the note on feedback below.
- An independent **per-operator ADSR envelope** (`fmOpNAttack`, `fmOpNDecay`, `fmOpNSustain`, `fmOpNRelease`).

### Carriers and Modulators

Each algorithm divides operators into two roles:

- **Carrier** — its output is added to the final audio signal you hear.
- **Modulator** — its output is fed into the phase input of another operator, adding harmonics and timbral complexity. Modulators are not heard directly.

### Algorithms

The `fmAlgorithm` attribute (1–32) selects the routing topology. A higher algorithm number generally means more carriers (brighter, additive sound). A lower number generally means deeper modulation chains (more harmonically rich FM timbres).

Some useful algorithm categories:

| Algorithms | Topology | Character |
|-----------|----------|-----------|
| 1–8 | Deep chains (1 or 2 carriers) | Rich, complex FM timbres — basses, pads, strings |
| 9–19 | Mixed chains + parallel | Balanced, versatile — pianos, plucks, bells |
| 20–28 | 3+ carriers | Bright, additive-leaning — choir, organs, leads |
| 29–32 | 4–6 carriers | Additive/organ-like — drawbar organs, rich pads |

Algorithm 32 is a special case: all six operators are carriers, each producing a sine wave at its own ratio. This is pure additive synthesis.

### Feedback

Each algorithm designates exactly one operator as the feedback source. In the DX7 this is always Op 6 (the `fmOp6Feedback` attribute), so `fmOp6Feedback` is the feedback parameter that works in all 32 algorithms. Setting feedback on other operators has no audible effect unless the active algorithm routes them as the feedback source.

Small feedback values (0.0–0.15) add subtle harmonics and warmth. Larger values (0.3–0.6) create bright sawtooth-like waves. Very high values (0.7–1.0) produce noise and distortion.

### Per-Operator Envelopes

Each operator has its own ADSR envelope that shapes how that operator's contribution evolves over time:

- **Carrier envelopes** shape the timbre loudness over time (like a VCA).
- **Modulator envelopes** shape the modulation depth: high modulation at attack = bright attack transient; low modulation after decay = simpler sustained tone.

**Sentinel value (`-1.0` for release):** If you omit a per-operator release, or set it to `-1.0`, the operator has no independent release — it is fully controlled by the outer `<group>` ADSR's release stage. This is the default, so you only need to specify per-operator release when you want the operator to decay independently while the note is held.

## Per-Operator ADSR Example: DX7 E. Piano Transient

The famous DX7 Electric Piano transient comes from the modulator decaying quickly on attack while the carrier sustains:

```xml
<group fmAlgorithm="5"
       attack="0.001" decay="0.5" sustain="0.0" release="1.5"
       fmOp1Ratio="1.0"  fmOp1Level="0.90"
       fmOp2Ratio="14.0" fmOp2Level="0.55"
       fmOp2Attack="0.001" fmOp2Decay="0.40" fmOp2Sustain="0.0" fmOp2Release="0.15"
       fmOp3Ratio="1.0"  fmOp3Level="0.70"
       fmOp4Ratio="14.0" fmOp4Level="0.40"
       fmOp4Attack="0.001" fmOp4Decay="0.30" fmOp4Sustain="0.0" fmOp4Release="0.12"
       fmOp5Ratio="1.0"  fmOp5Level="0.55"
       fmOp6Ratio="14.0" fmOp6Level="0.30" fmOp6Feedback="0.0"
       fmOp6Attack="0.001" fmOp6Decay="0.25" fmOp6Sustain="0.0" fmOp6Release="0.10">
  <oscillator waveform="fm6op"
              loNote="0" hiNote="127" rootNote="60"
              loVel="0" hiVel="127" volume="1.0"/>
</group>
```

Here Op6/Op5/Op4/Op3/Op2 are modulators. The modulator envelopes are short (`decay=0.25`–`0.40`, `sustain=0`) so they decay to zero quickly — creating the bright "tine" transient at the note start. The outer `<group>` ADSR shapes the overall volume fade.

## Real-Time Modulation with Bindings

All FM operator parameters support real-time modulation. This lets you connect LFOs, envelopes, MIDI CCs, or UI knobs to FM parameters for expressive control.

### Example: Knob controls modulation depth (morph timbre)

```xml
<ui width="812" height="375">
  <tab>
    <labeled-knob x="350" y="30" width="100" height="100"
                  label="FM Depth" parameterName="FMDepth"
                  minValue="0" maxValue="1" value="0.5">
      <binding type="general" level="group" position="0"
               parameter="OSCILLATOR_FM_OP2_LEVEL"
               translation="linear"
               translationOutputMin="0.0" translationOutputMax="1.0"/>
    </labeled-knob>
  </tab>
</ui>
```

### Supported binding parameters

| Parameter | Range | What it controls |
|-----------|-------|-----------------|
| `OSCILLATOR_FM_ALGORITHM` | 1–32 | Routing topology (usually set once, not modulated) |
| `OSCILLATOR_FM_OP1_LEVEL` … `OSCILLATOR_FM_OP6_LEVEL` | 0.0–1.0 | Per-operator modulation depth / carrier level |
| `OSCILLATOR_FM_OP1_RATIO` … `OSCILLATOR_FM_OP6_RATIO` | Any positive value | Per-operator frequency ratio (detune, octave shifts) |
| `OSCILLATOR_FM_OP1_FEEDBACK` … `OSCILLATOR_FM_OP6_FEEDBACK` | 0.0–1.0 | Per-operator self-feedback |

See [Appendix B](appendix-b-the-binding-element.md) for a full listing.

## Classic Sound Recipes

### Drawbar Organ (Algorithm 32 — all carriers)

Algorithm 32 turns the FM engine into a pure additive synthesizer. Map each operator to a harmonic of the fundamental:

```xml
<group fmAlgorithm="32"
       attack="0.001" decay="0" sustain="1.0" release="0.05"
       fmOp1Ratio="1.0" fmOp1Level="0.85"  fmOp1Release="0.05"
       fmOp2Ratio="2.0" fmOp2Level="0.65"  fmOp2Release="0.05"
       fmOp3Ratio="3.0" fmOp3Level="0.45"  fmOp3Release="0.05"
       fmOp4Ratio="4.0" fmOp4Level="0.30"  fmOp4Release="0.05"
       fmOp5Ratio="6.0" fmOp5Level="0.18"  fmOp5Release="0.05"
       fmOp6Ratio="8.0" fmOp6Level="0.10"  fmOp6Feedback="0.08"  fmOp6Release="0.05">
  <oscillator waveform="fm6op" loNote="0" hiNote="127" rootNote="60"
              loVel="0" hiVel="127" volume="1.0"/>
</group>
```

Short per-operator releases (0.05 s) produce the characteristic fast key-off click of a tonewheel organ.

### Bell / FM Marimba (Algorithm 5 — three 2-op pairs)

Inharmonic ratios + all-decay envelopes = percussive bell tones:

```xml
<group fmAlgorithm="5"
       attack="0.001" decay="0" sustain="1.0" release="20"
       fmOp1Ratio="1.0"  fmOp1Level="1.0"
       fmOp1Attack="0.001" fmOp1Decay="4.0" fmOp1Sustain="0.0" fmOp1Release="2.0"
       fmOp2Ratio="3.5"  fmOp2Level="0.35"
       fmOp2Attack="0.001" fmOp2Decay="0.15" fmOp2Sustain="0.0" fmOp2Release="0.1"
       fmOp3Ratio="1.0"  fmOp3Level="0.70"
       fmOp3Attack="0.001" fmOp3Decay="3.0"  fmOp3Sustain="0.0" fmOp3Release="1.5"
       fmOp4Ratio="3.5"  fmOp4Level="0.28"
       fmOp4Attack="0.001" fmOp4Decay="0.12" fmOp4Sustain="0.0" fmOp4Release="0.09"
       fmOp5Ratio="1.0"  fmOp5Level="0.50"
       fmOp5Attack="0.001" fmOp5Decay="2.0"  fmOp5Sustain="0.0" fmOp5Release="1.0"
       fmOp6Ratio="3.5"  fmOp6Level="0.22"  fmOp6Feedback="0.0"
       fmOp6Attack="0.001" fmOp6Decay="0.10" fmOp6Sustain="0.0" fmOp6Release="0.08">
  <oscillator waveform="fm6op" loNote="0" hiNote="127" rootNote="60"
              loVel="0" hiVel="127" volume="1.0"/>
</group>
```

Note the long outer release (`release="20"`) — this keeps the voice alive while the per-operator envelopes decay naturally to silence on their own.

### Choir Pad (Algorithm 19 — shared modulator with detuned carriers)

Algorithm 19 routes a single Op6 modulator into both Op5 and Op4. Setting their ratios slightly apart creates a beating, chorus-like quality:

```xml
<group fmAlgorithm="19"
       attack="1.0" decay="0" sustain="1.0" release="20"
       fmOp4Ratio="1.003" fmOp4Level="0.75"
       fmOp4Attack="0.90" fmOp4Decay="0.0" fmOp4Sustain="1.0" fmOp4Release="1.5"
       fmOp5Ratio="1.0"   fmOp5Level="0.85"
       fmOp5Attack="0.90" fmOp5Decay="0.0" fmOp5Sustain="1.0" fmOp5Release="1.5"
       fmOp6Ratio="1.0"   fmOp6Level="0.30" fmOp6Feedback="0.08"
       fmOp6Attack="0.50" fmOp6Decay="0.30" fmOp6Sustain="0.20" fmOp6Release="0.5"
       fmOp1Ratio="1.0" fmOp1Level="0.60"
       fmOp1Attack="0.80" fmOp1Decay="0.0" fmOp1Sustain="1.0" fmOp1Release="1.5"
       fmOp2Ratio="1.0" fmOp2Level="0.50"
       fmOp2Attack="0.50" fmOp2Decay="1.0" fmOp2Sustain="0.50" fmOp2Release="1.0"
       fmOp3Ratio="3.0" fmOp3Level="0.35"
       fmOp3Attack="0.30" fmOp3Decay="0.50" fmOp3Sustain="0.30" fmOp3Release="0.5">
  <oscillator waveform="fm6op" loNote="0" hiNote="127" rootNote="60"
              loVel="0" hiVel="127" volume="1.0"/>
</group>
```

## Tips

- **Start simple.** Pick algorithm 5 and set only Op1 (carrier) + Op2 (modulator) to non-default values. Gradually raise `fmOp2Level` from 0 to 1 to hear FM modulation build from a sine wave to a complex timbre.
- **Keep loudness consistent.** When many operators have high levels the output can clip. Lower `volume` on the `<group>` or reduce carrier levels accordingly.
- **Per-operator release tuning.** Set per-operator releases independently to make different parts of the spectrum fade at different rates — carriers fade slowly (long `release`), modulators fade faster (short `release`) for a natural brightness decay.
- **Inharmonic ratios for metallic textures.** Try ratios like `3.5`, `7.07`, `1.41` (√2), or other non-integers on modulators for bells, gongs, and metallic sounds.
- **Operator 6 first.** `fmOp6Feedback` is nearly always the feedback operator. Start by adjusting this first when you want to add harmonic richness.
- **Combine with samples.** A group containing `<sample>` elements can share a track with an `fm6op` group. Use the groups' `output*` routing to send them to different buses if needed.
