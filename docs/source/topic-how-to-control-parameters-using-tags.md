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