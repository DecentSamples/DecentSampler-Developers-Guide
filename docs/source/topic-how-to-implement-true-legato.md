# How to implement true legato

Let's walk through how to build a basic true legato instrument. Legato instruments generally consist of either two or three groups: 

1. First, there's the initial looping sustain sample that will get played when the note is first pressed down. 
2. Then there is the legato transition sample that will get played when a second note gets pressed
3. (optional) Depending on the implementation, there may be a third looping sustain group that gets played after the legato transition sample plays. In such cases, this third group usually contains the same samples as were used in group 1. 

**Step 1: Voice muting**

Before we get into legato, let’s talk about voice muting. This is the behavior wherein one set of samples causes another set of samples to stop playing. This can be desirable in situations such as legato instruments where two samples should not be sounding at the same time.

In Decent Sampler, voice-muting makes use of tags. These are text labels that you can use to identify samples or groups of samples. You can add a `silencedByTags` attribute to groups or sample elements. This consists of a comma-separated list of tags that specify which samples should silence the current samples. When a sample with a tag matching one of the tags in the `silencedByTags` is played, it will silence the current sample (or group). Here’s an example:

```xml
<DecentSampler>
    <groups>
        <group tags="sustain" silencedByTags="legato" silencingMode="normal" release="1.0">
            <!-- Your sustain samples go here. -->
        </group>
        <group tags="legato" silencedByTags="legato" silencingMode="normal" release="1.0">
            <!-- Your legato samples go here. -->
        </group>
    </groups>
</DecentSampler>
```

In the above scenario, if a legato sample is matched, any sample that might be playing from the "sustain" group will be stopped.

It's also worth mention the `silencingMode` attribute as well (a value of `fast` means we immediately silence that sample, whereas `normal` means we trigger the ADSR release phase).

**Step 2: Legato**

In order to specify which samples should be triggered first, we use the `trigger` attribute. This can that can be added to the `<group>` or individual `<sample>` tags. The default value is `attack`, but there are two useful new values:

- **`first`**: This value means that this sample will only trigger if no other notes are playing
- **`legato`**: This value means that this sample will only trigger if other notes are already playing

Here is an example of how this might look in cunjunction with our example from above:

```xml
<DecentSampler>
    <groups>
        <group trigger="first" tags="sustain" silencedByTags="legato" silencingMode="normal" release="1.0">
            <!-- Your sustain samples go here. -->
        </group>
        <group trigger="legato" tags="legato" silencedByTags="legato" silencingMode="normal" release="1.0">
            <!-- Your legato samples go here. -->
        </group>
    </groups>
</DecentSampler>
```

**Step 3: Specifying previous notes**

When creating a legato instrument, it is often essential to limit which legato transition gets played based on which note we are transition from. This can be achieved using either the `previousNote` or the `legatoInterval` attributes. The `previousNote` attribute causes the engine to only play this sample if the previously triggered note matches the specified note. Example usage:

```xml
<sample path="Samples/LV_Legato_F2_G2.wav" rootNote="G2" loNote="G2" hiNote="G2" previousNote="F2" start="43000" />
<sample path="Samples/LV_Legato_G2_A2.wav" rootNote="A2" loNote="A2" hiNote="A2" previousNote="G2" start="43000" />
```

The `legatoInterval` attribute causes the engine to only play the sample if distance between the current note and the previously triggered note is exactly this semitone distance. For example, if the note for which this sample is being triggered is a C3 and the `legatoInterval` is set to `-2`, then the sample will only play if the previous note was a D3 because D3 minus two semitones equals C3. 

**Step 4: Polyphony**

In legato instruments, it is sometimes useful to limit polyphony for a specific sample or set of samples. This is achieve using tags. At the top-level of your file, you can specify a <tags> element as follows:

```xml
<DecentSampler>
    <groups>
        <group tags="some-tag" >
            <!-- ... -->
        </group>
    </groups>
    <tags>
        <tag name="some-tag" polyphony="1" />
    </tags>
</DecentSampler>
```

**Putting it all together**

Here is a full example of a legato instrument:

```xml
<?xml version="1.0" encoding="UTF-8"?>

<DecentSampler pluginVersion="1">
  <groups>
    <group tags="sustain" silencedByTags="legato" trigger="first"
           silencingMode="normal" release="0.3">
      <sample path="Samples/LV_Sustain_D3.wav" rootNote="D3" loNote="C3" hiNote="D3"/>
      <sample path="Samples/LV_Sustain_E3.wav" rootNote="E3" loNote="D#3" hiNote="E3"/>
      <!-- More samples go here -->
    </group>
    <group tags="legato" silencedByTags="legato" trigger="legato"
           silencingMode="normal" attack="0.1" decay="0.2" sustain="0" release="1">
              previousNote="C#3"/>
      <sample path="Samples/LV_Legato_D3_E3.wav" rootNote="E3" loNote="E3"
              hiNote="E3" start="43000" previousNote="D3"/>
      <sample path="Samples/LV_Legato_E3_F3.wav" rootNote="F3" loNote="F3"
              hiNote="F3" start="43000" previousNote="E3"/>
      <!-- More samples go here -->
    </group>
    <group tags="legato-tails" silencedByTags="legato-tails" trigger="legato"
           attack="0.3" silencingMode="normal" release="0.3" start="5000">
      <sample path="Samples/LV_Sustain_D3.wav" rootNote="D3" loNote="C3" hiNote="D3"/>
      <sample path="Samples/LV_Sustain_E3.wav" rootNote="E3" loNote="D#3" hiNote="E3"/>
      <!-- More samples go here -->
    </group>
  </groups>
</DecentSampler>
```
