The &lt;groups&gt; element (required)
===============================

In this section, we'll find elements that pertain to samples and sample-mapping.

Every **dspreset** file should have one and only one `<groups>` element. This is where you specify the samples that make up your sample library. This element lives right underneath the top-level `<DecentSampler>` element. The basic structure is this:

```xml    
<DecentSampler>
    <groups>
        <group>
            <sample /> <!-- This is where -->
            <sample /> <!-- the samples   -->
            <sample /> <!-- get defined   -->
        </group>
    </groups>
</DecentSampler>
```

| Attribute          |            | Description       |
|--------------------|------------| ------------------|
| **`volume`**       | (optional) | The volume of the instrument as a whole. This will be reflected in the UI in the top-right corner. Value can be in linear 0.0-1.0 or in decibels. If it's in decibels you must append dB after the value (example: "3dB"). Default: 1.0 (no volume change)  |
| **`globalTuning`** | (optional) | Global pitch adjustment for changing note pitch. In semitones. For example 1.0 would be a half-step up. Default: 0 |
| **`glideTime`** | (optional) | The glide/portamento time in seconds. A value of 0.0 would mean no portamento. This value can also be set at the `<group>` and `<sample>` levels, although most people will want to set it globally at the `<groups>` level. Default: 0.0 |
| **`glideMode`** | (optional) | Controls the glide/portamento behavior. Possible values are: `always` (glide is always performed), `legato` (glide is performed only when transitioning from one note to another),  and `off`. This value can also be set at the `<group>` and `<sample>` levels, although most people will want to set it globally at the `<groups>` level. Default: `legato` |

## The &lt;group&gt; element
Samples live in groups. There can be many group elements under the `<groups>` element. It can be useful to sort your samples into groups in order to apply similar settings to them or to control them with a knob. The order of groups in a file matters insofar as bindings will often reference groups by using an index. The first group in a file is group 0, the second is group 1, etc.

| Attribute         | Description                                                                                                                                                                                                                                                                                                                                        |            |
|:------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------|
| **`enabled`**     | Whether or not this group is enabled. Possible values: true, false. Default: true                                                                                                                                                                                                                                                                  | (optional) |
| **`volume`**      | The volume of the group. Value can be in linear 0.0-1.0 or in decibels. If it's in decibels you must append dB after the value (example: "3dB"). Default: 1.0                                                                                                                                                                                      | (optional) |
| **`ampVelTrack`** | The degree to which the velocity of the incoming notes affects the volume of the samples in this group. 0 = not at all. 1 = volume is completely determined by incoming velocity. When the value is 1, a velocity of 127 (max velocity) yields a gain 1.0 (full volume), a velocity of 63 (half velocity) yields a gain of 0.5 (half volume), etc. | (optional) |
| **`groupTuning`** | Group-level pitch adjustment for changing note pitch. In semitones. For example 1.0 would be a half-step up and -1 would a half-step down. Default: 0                                                                                                                                                                                              | (optional) |

### The &lt;sample&gt; element

Underneath the `<group>` elements are `<sample>` elements. Each sample corresponds to a playable "zone" of your instrument. Attributes:

| Attribute                   |            | Description       |
|-----------------------------|------------| ------------------|
| **`path`**                  | (required) | The relative path of the sample file to play for this zone.  |
| **`rootNote`**              | (required) | The MIDI note number (from 1 to 127) of the note. |
| **`loNote`**                | (optional) | The MIDI note number (from 1 to 127) of the lowest note for which the zone should be triggered. Default: 0. |
| **`hiNote`**                | (optional) | The MIDI note number (from 1 to 127) of the highest note for which the zone should be triggered. Default: 127. |
| **`loVel`**                 | (optional) | The lowest velocity for which this zone should be triggered. Default: 0 |
| **`hiVel`**                 | (optional) | The highest velocity for which this zone should be triggered. Default: 127 |
| **`loCCN`** **`hiCCN`** | (optional) | Using these parameter, you can use MIDI continuous controllers to filter whether or not a note should be played. This lets you, for example, have one set of samples that get played when the piano sustain pedal is down and another set that get played when it is up. Each time a MIDI CC value comes for a specific CC#, the engine stores that value. When a "note on" signal is received, the engine makes a decision (based on the last received value and the range defined by these attributes) about whether or not this sample should be played. If you use `loCCN`, you must also use a corresponding `hiCCN` for the same MIDI CC number so that you are defining a range of values. Example: `loCC64="90"` and `hiCC64="127"` would mean that a "note on" message will only trigger this sample if the last received value for CC64 (Sustain Pedal) is between 90 and 127. This can also be set at the `<group>` level. Default:-1 (off) |
| **`start`**                 | (optional) | The frame/sample position of the start of the sample audio. This is useful if the sample starts midway through the audio file. Default: 0 |
| **`end`**                   | (optional) | The frame/sample position of the end of the sample audio. The is useful is the zone ends before the end of the audio file. Default: the file's length in samples minus 1. |
| **`tuning`**                | (optional) | A fine-tuning number (in semitones) for changing the note pitch. e.g 1.0 would be a half-step up. Default: 0 |
| **`volume`**                | (optional) | The volume of the sample. Value can be in linear 0.0-1.0 or in decibels. If it's in decibels you must append dB after the value (example: "3dB"). Default: 1.0  |
| **`pan`**                   | (optional) | A number of -100 to 100. -100 in panned all the way to the left, 100 is panned all the way to the right. This can also be set at the `<group>` or `<groups>` levels. Default: 0 |
| **`pitchKeyTrack`**         | (optional) | A number from 0.0 to 1.0. 0 means that the pitch will stay the same regardless of what note is played. 1 means that the pitch will increase by one semitone when the note increases by one semitone (i.e. normal key pitch tracking). This can also be set at the `<group>` level. Default: 1 |
| **`trigger`**               | (optional) | Valid values: `attack` means a sample is played when the *note on* message is received. `release` means the sample is played when the *note off* message is received (aka a release trigger). `first` means that the sample will only be played if no other notes are playing. `legato` means that the sample will only be played if _some_ other notes are already playing. This can also be set at the `<group>` level. Default: `attack`. |  
| **`tags`**                  | (optional) | A command-separated list of tags. Example: tags="rt,mic1". These are useful when controlling volumes using tags. See Appendix D. |  
| **`onLoCCN`** **`onHiCCN`** | (optional) | If you want a sample to be triggered when a MIDI CC controller message comes in, for example for piano pedal down and pedal up samples, you use these attributes to specify the range of values that should trigger the sample. If you use `onLoCCN`, you must also use a corresponding `onHiCCN` for the same MIDI CC number. Example: `onLoCC64="90"` and `onHiCC64="127"` would mean that values of CC64 (Sustain Pedal) between 90 and 127 will trigger the given sample. This can also be set at the `<group>` level. Default:-1 (off) |
| **`playbackMode`**          | (optional) | By default, the choice of playback engine is left up to the discretion of the user–they can set this in the Preferences screen. However, a sample creator can override this setting by setting the playbackMode for a specific sample, a group, or the entire instrument. Possible values are `memory`, `disk_streaming`, and `auto` (default). |

**Looping**

| Attribute                |            | Description       |
|--------------------------|------------| ------------------|
| **`loopStart`**          | (optional) | The frame/sample position of the start of the sample's loop. If this is not specified, but the sample is a wave file with embedded loop markers, those will be used instead. Default: 0 |
| **`loopEnd`**            | (optional) | The frame/sample position of the end of the sample's loop. If this is not specified, but the sample is a wave file with embedded loop markers, those will be used instead. Default: the file's length in samples minus 1. |
| **`loopCrossfade`**      | (optional) | When loop crossfades are used, instead of simply looping at a specific end point, a portion of the audio from before the loop point is faded in just as the audio from the end of the loop is faded out. In this way, smooth audio loops can be achieved on samples that weren't specifically prepared as looping. This parameter is used for specifying the length of the crossade region in frames/samples. This can also be set at the `<group>` level. Default: 0 (crossfades off). |
| **`loopCrossfadeMode`**  | (optional) | This parameter is used to specify the curve used for crossfading when loop crossfades are turned on. This can also be set at the `<group>` level. Value values: `linear`,  `equal_power`. Default: `equal_power`. |
| **`loopEnabled`**        | (optional) | A boolean value indicating whether or not the loop should be used. Valid values: true, false |

**Amplitude Envelope**

Each sample has its own ADSR amplitude envelope. 

| Attribute         |            | Description       |
|-------------------|------------| ------------------|
| **`ampEnvEnabled`**      | (optional) | This turns the amplitude envelope on and off. Valid values are: `false` and `true` (default).  |
| **`attack`**      | (optional) | The attack time in seconds of the amplitude envelope of this zone. This can also be set at the `<group>` or `<groups>` levels.  |
| **`decay`**       | (optional) | The decay time in seconds of the amplitude envelope of this zone.  This can also be set at the `<group>` or `<groups>` levels.  |
| **`sustain`**     | (optional) | The sustain level (0.0 - 1.0) of the amplitude envelope of this zone.  This can also be set at the `<group>` or `<groups>` levels. |
| **`release`**     | (optional) | The release time in seconds of the amplitude envelope of this zone. This can also be set at the `<group>` or `<groups>` levels. |

The curve shapes of the attack, decay, and release zones can be changed as well. All three of the of the following parameters use the same system: a value from -100 to 100 that determines the shape of the curve. **-100 is a logarithmic curve, 0 is a linear curve, and 100 is an exponential curve.**

<p align="center">
<img alt="-100 is a logarithmic curve, 0 is a linear curve, and 100 is an exponential curve" src="_static/images/adsr-curves.png" title="ADSR Curves" style="max-width: 535px;" />
</p>

| Attribute         |            | Description       | Default Value |
|-------------------|------------| ------------------|---------------|
| **`attackCurve`**      | (optional) | A value from -100 to 100 that determines the shape of the attack curve. This can also be set at the `<group>` or `<groups>` levels.  | -100 (logarithmic) |
| **`decayCurve`**       | (optional) | A value from -100 to 100 that determines the shape of the decay curve. The decay time in seconds of the amplitude envelope of this zone.  This can also be set at the `<group>` or `<groups>` levels.  | 100 (exponential) |
| **`releaseCurve`**     | (optional) | A value from -100 to 100 that determines the shape of the release curves. The release time in seconds of the amplitude envelope of this zone. This can also be set at the `<group>` or `<groups>` levels. | 100 (exponential) |

**Round Robins**

Round robins allow different samples to be played each time a zone is triggered. This is especially useful with sounds that have short attacks (such as drums), and is a great way to keep your sample libraries from sounding fake. In order for round robins to work, you must specify both a `seqMode` and a `seqPosition` for all samples. If you have several different sets of round robins with different lengths, you'll want to set the `seqLength` value as well. There are several round-robin modes:

- `round_robin`: This causes samples to be triggered sequentially according to their `seqPosition` values.
- `random`: This causes random samples to be chosen from within the group of samples. If there are more than two round robins, then the algorithm makes sure not to hit the same one twice in a row.
- `true_random`: This causes random samples to be chosen from within the group of samples.
- `always`: This just turns round robins off.

| Attribute         |            | Description                                                                                                                                                                                                                                    |
|:------------------|:-----------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **`seqMode`**     | (optional) | Valid values are `random`, `true_random`, `round_robin`, and `always`. A value indicating the desired round robin behavior for this sample or group of samples. This can also be set at the `<group>` and `<groups>` levels. Default: `always` |
| **`seqLength`**   | (optional) | The length of the round robin queue. This can also be set at the `<group>` or `<groups>` levels. If this is left out, then the engine will try to auto-detect the length of the roudn robin sequence. Default: 0                               |
| **`seqPosition`** | (optional) | A number indicating this zone's position in the round robin queue. This can also be set at the `<group>` level. Default: 1                                                                                                                     |

**Voice-Muting / Legato**

**Looping**

| Attribute            |            | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:---------------------|:-----------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **`silencedByTags`** | (optional) | A command-separated list of tags. Example: tags="rt,mic1". If a sample containing one of these tags gets triggered, then this sample will be stopped. This is useful when setting up drums as it will allow you mute one hi-hat when another hi-hat plays. See Appendix D.                                                                                                                                                                                                  |
| **`silencingMode`**  | (optional) | Controls how quickly voices get silenced. `fast` = immediately; `normal` = triggers the sample's release phase. This second option, when used in conjunction with the `release` attribute, allows you to specify a longer release time. Values: `fast`, `normal`. Default: `fast`                                                                                                                                                                                           |
| **`previousNotes`**  | (optional) | Only play this sample if the previously triggered note equals one of these notes. Format: a comma-separated list of MIDI note numbers (from 1 to 127) of the note.                                                                                                                                                                                                                                                                                                          |
| **`legatoInterval`** | (optional) | This is similar to the `previousNote` attribute. This causes the engine to only play the sample if the previously triggered note is exactly this semitone distance from the previous note. For example, if the note for which this sample is being triggered is a C3 and the `legatoInterval` is set to `-2`, then the sample will only play if the previous note was a D3 because D3 minus 2 semitones equals C3. Format: This can be a positive or negative whole number. |
