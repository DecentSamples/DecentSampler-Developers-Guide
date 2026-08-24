# How to Use the Arpeggiator within your Sample Libraries

As of version 1.26.0, **DecentSampler** has a built-in arpeggiator. Unlike the [note sequencer](the-noteSequences-element), which replays a fixed pattern you authored ahead of time, the arpeggiator reacts live to whatever notes are currently held: hold a chord and it generates a stream of notes from that chord — in the order and octave range you choose — for as long as you hold it. This guide walks through arming it, exposing its parameters as knobs, and modulating it with an LFO.

## The `<arpeggiator>` element

Add a single `<arpeggiator>` element as a direct child of your `<DecentSampler>` root, alongside `<groups>`, `<midi>`, etc.:

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
               arpRateMultiplier="1.0"/>
</DecentSampler>
```

With just this, the arpeggiator is already fully functional — hold a chord and it will play. The full attribute reference, including a worked example for every `arpOrder` value, is in [the &lt;arpeggiator&gt; element](the-arpeggiator-element).

## Binding knobs to the arpeggiator

Every `<arpeggiator>` attribute can be controlled from a UI knob using the `arpeggiator` binding type at `level="instrument"`. Which `translation` you use depends on the kind of parameter:

- **Discrete parameters** (`ARP_ENABLED`, `ARP_ORDER`, `ARP_OCTAVE_MODE`) are a fixed list of named states, so they're best driven by a `valueType="multi_state"` knob — each state fires a `translation="fixed_value"` binding that sends the exact attribute value as a string.
- **Numeric parameters** (`ARP_OCTAVE_RANGE`, `ARP_STEP_COUNT`, `ARP_GATE_LENGTH`, `ARP_RATE_MULTIPLIER`, `ARP_OVERRIDE_BPM`) are continuous or integer ranges, driven the same way as any other numeric binding (`translation="linear"`, with `valueType="integer"` for the whole-number ones).

### An on/off toggle

```xml
<labeled-knob x="12" y="40" width="90" height="110" label="Arp" valueType="multi_state" value="1">
  <state name="Off">
    <binding type="arpeggiator" level="instrument" parameter="ARP_ENABLED" translation="fixed_value" translationValue="false"/>
  </state>
  <state name="On">
    <binding type="arpeggiator" level="instrument" parameter="ARP_ENABLED" translation="fixed_value" translationValue="true"/>
  </state>
</labeled-knob>
```

### A discrete order selector

Turning this knob cycles through named states, each firing a `fixed_value` binding with the matching `arpOrder` string:

```xml
<labeled-knob x="112" y="40" width="120" height="110" label="Order" valueType="multi_state" value="0">
  <state name="Up">
    <binding type="arpeggiator" level="instrument" parameter="ARP_ORDER" translation="fixed_value" translationValue="up"/>
  </state>
  <state name="Down">
    <binding type="arpeggiator" level="instrument" parameter="ARP_ORDER" translation="fixed_value" translationValue="down"/>
  </state>
  <state name="Up-Down">
    <binding type="arpeggiator" level="instrument" parameter="ARP_ORDER" translation="fixed_value" translationValue="up_down"/>
  </state>
  <state name="Up-Down Incl">
    <binding type="arpeggiator" level="instrument" parameter="ARP_ORDER" translation="fixed_value" translationValue="up_down_inclusive"/>
  </state>
  <state name="Down-Up">
    <binding type="arpeggiator" level="instrument" parameter="ARP_ORDER" translation="fixed_value" translationValue="down_up"/>
  </state>
  <state name="Down-Up Incl">
    <binding type="arpeggiator" level="instrument" parameter="ARP_ORDER" translation="fixed_value" translationValue="down_up_inclusive"/>
  </state>
  <state name="As Played">
    <binding type="arpeggiator" level="instrument" parameter="ARP_ORDER" translation="fixed_value" translationValue="as_played"/>
  </state>
  <state name="Random">
    <binding type="arpeggiator" level="instrument" parameter="ARP_ORDER" translation="fixed_value" translationValue="random"/>
  </state>
  <state name="Random No Repeat">
    <binding type="arpeggiator" level="instrument" parameter="ARP_ORDER" translation="fixed_value" translationValue="random_no_repeat"/>
  </state>
</labeled-knob>
```

### Numeric knobs

Octave range and step count are whole numbers, so use `valueType="integer"`. Gate length and rate multiplier are continuous:

```xml
<labeled-knob x="242" y="40" width="90" height="110" label="Octave Range"
              valueType="integer" minValue="1" maxValue="4" value="2" defaultValue="2">
  <binding type="arpeggiator" level="instrument" parameter="ARP_OCTAVE_RANGE" translation="linear"/>
</labeled-knob>

<labeled-knob x="472" y="40" width="90" height="110" label="Gate Length"
              valueType="float" minValue="0.05" maxValue="1.5" value="0.6" defaultValue="0.6">
  <binding type="arpeggiator" level="instrument" parameter="ARP_GATE_LENGTH" translation="linear"/>
</labeled-knob>
```

## Modulating the arpeggiator with an LFO

Because a knob and a modulator reach the arpeggiator through the exact same `<binding type="arpeggiator">` mechanism, you can also target it from an `<lfo>` (or any other modulator) — for example, to breathe the gate length in and out over time:

```xml
<modulators>
  <lfo shape="sine" rate="0.2" scope="global">
    <binding type="arpeggiator" level="instrument" parameter="ARP_GATE_LENGTH"
             modBehavior="add" translation="linear"
             translationOutputMin="-0.2" translationOutputMax="0.2"/>
  </lfo>
</modulators>
```

A quick note on rate modulation specifically: nudging `ARP_RATE_MULTIPLIER` from a knob or MIDI CC (a mod wheel ramping the arp faster during a performance, say) is common and works well. Continuously modulating it from a fast LFO is a much less common technique — most arpeggiators treat rate as a synced, discrete setting rather than a modulation target, because warping the clock mid-stream can make individual steps land unevenly. It's supported (nothing about the binding is special-cased), but treat it as an experimental, "clock wobble"-style effect rather than a typical use case.

## Conclusion

In this guide, we went over how to arm the arpeggiator, how to expose its parameters as knobs — both discrete (multi-state) and numeric — and how to modulate it with an LFO using the same binding mechanism. For the complete attribute reference and a worked example of every order mode, see [the &lt;arpeggiator&gt; element](the-arpeggiator-element).
