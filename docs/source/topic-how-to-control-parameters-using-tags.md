# How to control parameters using tags (Example: Mic-level Knobs)

As of version 1.0.2, the best way to implement mic-level knobs is using the new sample tagging feature. It is possible to assign tags to specific samples. In this way, you can specify which type of sound they are:

```xml
<sample volume="0.0dB" tags="note,mic1" />
<sample volume="0.0dB" tags="rt,mic1" />
<sample volume="0.0dB" tags="note,mic2" />
<sample volume="0.0dB" tags="rt,mic2" />
```

You can also assign tags at the group level. You can also mix and match, and the tags specified at the group level will be added to the list of tags already specified at the sample level:

```xml
<group tags="note">
  <sample volume="0.0dB" tags="mic1" />
  <sample volume="0.0dB" tags="mic2" />
</group>
<group tags="rt">
  <sample volume="0.0dB" tags="mic1" />
  <sample volume="0.0dB" tags="mic2" />
</group>
```

Then you can make controls with bindings that reference those tags:

```xml
<control x="246" y="115" parameterName="MIC 1" style="linear_bar_vertical" type="float" minValue="0" maxValue="100" value="60" width="20" height="70" trackForegroundColor="FFFFFFFF" trackBackgroundColor="FF888888">
    <binding type="amp" level="tag" identifier="mic1" parameter="AMP_VOLUME" />
</control>

<control x="346" y="115" parameterName="MIC 2" style="linear_bar_vertical" type="float" minValue="0" maxValue="100" value="60" width="20" height="70" trackForegroundColor="FFFFFFFF" trackBackgroundColor="FF888888">
    <binding type="amp" level="tag" identifier="mic2" parameter="AMP_VOLUME" />
</control>
```
## Targeting individual samples and oscillators

The bindings above use `level="tag"`, which is a good fit for volume-style controls but only reaches
a fixed set of parameters. If you need to change some *other* parameter on one specific sample, use
`level="sample"` together with the `sampleTags` attribute. This targets every `<sample>` carrying one
of those tags, in any group, instead of the whole group the sample happens to live in:

```xml
<group>
  <sample loNote="0" hiNote="127" rootNote="60" path="Samples/close.wav" tags="mic1" />
  <sample loNote="0" hiNote="127" rootNote="60" path="Samples/far.wav" tags="mic2" />
</group>
```

```xml
<control x="246" y="115" parameterName="MIC 1 TUNE" type="float" minValue="-12" maxValue="12" value="0">
    <binding type="amp" level="sample" sampleTags="mic1" parameter="TUNING" />
</control>
```

Because both samples sit in the same group, a `level="group"` binding would have moved both of them.
`sampleTags` reaches just the one.

Oscillators work the same way, using `level="oscillator"` and `oscillatorTags`:

```xml
<group>
  <oscillator shape="saw" tags="osc1" />
  <oscillator shape="square" tags="osc2" />
</group>
```

```xml
<control x="346" y="115" parameterName="OSC 2 TUNE" type="float" minValue="-12" maxValue="12" value="0">
    <binding type="amp" level="oscillator" oscillatorTags="osc2" parameter="TUNING" />
</control>
```

If you leave the typed attribute off and just write `tags="mic1"`, it will still work: at
`level="sample"` a plain `tags` list is treated as `sampleTags`, and at `level="oscillator"` as
`oscillatorTags`. Prefer the typed attributes in new presets, since they say what they mean.
