The &lt;effects&gt; element
===========================

Adding global, instrument-wide effects is easy: just add an `<effects>` element right below your top-level `<DecentSampler>` element. 

It is also possible to have effects that only get added to a specific group. To adding effects that only apply to a specific group, all you need to do is create an `<effects>` group that lives underneath the `<group>` element for the group you want to affect. 

Group level effects are initialized every time a note is started and destroyed every time a note is stopped. If you play two notes simultaneously, two instances of this effect will be created and these will be independent of eachother. As a result, they use more CPU than global effects. 

NOTE: Only certain effects will work as group-level effects: lowpass filter, hipass filter, bandpass filter, gain, and chorus. Delay and reverb cannot work properly as they will be deleted before their tail peters out.

## The &lt;effect&gt; element
Within the `<effects>` element, you can have any number of `<effect>` sub-elements. These specify parameters for each individual effect that you would like to have in your global effects chain. There are currently only a handful effects available although more could definitely be added on request:

### Low-pass, Band pass, and Hi-pass filter

A 2-pole resonant filter that can be either a lowpass, bandpass, or highpass filter

Example:
```xml
<DecentSampler>
  <effects>
    <effect type="lowpass" resonance="0.7" frequency="22000" />
  </effects>
</DecentSampler>
```

There is also a single pole version of the lowpass filter that can be accessed using the `lowpass_1pl` effect type. This version does not have a resonance parameter.

```xml
<DecentSampler>
  <effects>
    <effect type="lowpass_1pl" frequency="22000" />
  </effects>
</DecentSampler>
```

Attributes:

|  Attribute  |          |           Type           |                                 Valid Range                                 | Default |
| ----------- | -------- | ------------------------ | --------------------------------------------------------------------------- | ------- |
| `type`      | Required | The type of filter       | Must be either `lowpass` (legacy: `lowpass_4pl`), `lowpass_1pl`, `bandpass`, or `highpass` |         |
| `resonance` | Optional | The filter resonance (Q) | 0.001 - 5.0, where 5 is big, 0 is small.                                      |     0.7 |
| `frequency` | Optional | The filter frequency     | 0 - 1.0, where 0 is not damped, 1.0 is fully damped.                        |     0.3 |



### Notch EQ Filter


A simple notch filter.

Example:
```xml
<DecentSampler>
  <effects>
    <effect type="notch"  q="0.7" frequency="22000" />
  </effects>
</DecentSampler>
```

Attributes:

|  Attribute  |          |           Type           |                                 Valid Range                                 | Default |
| ----------- | -------- | ------------------------ | --------------------------------------------------------------------------- | ------- |
| `type`      | Required | The type of filter       | Must be `notch`                                                             |         |
| `frequency` | Required | The filter frequency     | 60 - 22000.0                                                                |   10000 |
| `Q`         | Optional | Q is the ratio of center frequency to bandwidth | 0.01 - 18.0                                          |     0.7 |


### Peak EQ Filter

A peak filter centred around a given frequency, with a variable Q and gain.

Example:
```xml
<DecentSampler>
  <effects>
    <effect type="peak"  q="0.7" frequency="22000" gain="2.0" />
  </effects>
</DecentSampler>
```

Attributes:

|  Attribute  |          |           Type           |                                 Valid Range                                 | Default |
| ----------- | -------- | ------------------------ | --------------------------------------------------------------------------- | ------- |
| `type`      | Required | The type of filter       | Must be `peak`                                                              |         |
| `frequency` | Required | The filter frequency     | 60 - 22000.0                                                                |     10000 |
| `Q`         | Optional | Q is the ratio of center frequency to bandwidth | 0.01 - 18.0                                          |     0.7 |
| `gain`      | Required | Values greater than 1.0 will boost the high frequencies, values less than 1.0 will attenuate them.     | 0 - 1.0                        |     1.0 |

### Gain effect

Applies a volume boost or cut to the output signal.

Example:
```xml
<DecentSampler>
  <effects>
    <effect type="gain" level="-6" />
  </effects>
</DecentSampler>
```

Attributes:

| Attribute |          | Type                                                                                                      | Default | Valid Range |
| --------- | -------- | --------------------------------------------------------------------------------------------------------- | ------- | ----------- |
| `type`    | Required | Must be `gain`                                                                                            |         | `gain`      |
| `level`   | Required | The amount of gain to be applied expressed in decibels. In other words, gain of -6dB reduces sound by 50% | 0       | -99 - 24    |

### Reverb effect

Example:
```xml
<DecentSampler>
  <effects>
    <effect type="reverb" roomSize="" damping="" wetLevel="" />
  </effects>
</DecentSampler>
```

Attributes:

| Attribute  |          |             Type            |                     Valid Range                      | Default |
| ---------- | -------- | --------------------------- | ---------------------------------------------------- | ------- |
| `type`     | Required | Must be `reverb`            | `reverb`                                             |         |
| `roomSize` | Optional | The reverb "room size"      | 0 - 1.0, where 1.0 is big, 0 is small.               |     0.7 |
| `damping`  | Optional | The reverb damping level    | 0 - 1.0, where 0 is not damped, 1.0 is fully damped. |     0.3 |
| `wetLevel` | Optional | The volume of reverb signal | 0 - 1.0                                              |       0 |

### Delay effect

A simple delay effect that can be controlled either in seconds or using musical time increments based on the host tempo. For a complete explanation of how to use tempo-syncing, see [here](https://www.decentsamples.com/2024/02/08/for-sample-creators-how-to-add-tempo-synced-delay-to-your-instruments/).

**Attributes:**

| Attribute         |          | Type                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Valid Range                                           | Default   |
|:------------------|:---------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------|:----------|
| `type`            | Required | Must be `delay`                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | `delay`                                               |           |
| `delayTimeFormat` | Optional | Determines whether the delay will be synced to DAW tempo or not, as well as what format you will be using for the `delayTime` parameter. There are two possible values: 1) `seconds`, which is the default, means that `delayTime` will be specified in seconds and will not change when the DAW tempo changes; 2) `musical_time` means that delay time will be specified using an integer value generated by a control which is setup to use the `musical_time` `valueType` parameter. In order for this to work, you will need to be using the plug-in within a DAW that provides a tempo to the plug-in.                                                                                                               | `seconds`, `musical_time`                             | `seconds` |
| `delayTime`       | Optional | The delay time in seconds                                                                                                                                                                                                                                                                                                                                                                                                                                                              | 0 - 20.0                                              | 0.7       |
| `feedback`        | Optional | The feedback level.                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | 0 - 1.0, where 0 is no feedback, 1.0 is max feedback. | 0.2       |
| `stereoOffset`    | Optional | The parameter allows you to introduce delay variations between the left and right channels. Half of this amount is subtracted from the left channel's delay time and half of this amount is added to the right channel's delay time. For example, if the `delayTime` is 0.5 seconds and the `stereoOffset` is 0.02 s, then the actual left channel delay time will be 0.49s and the actual right channel delay time will be 0.51s so that the two channels are offset by 0.02 seconds. | -10 - 10                                              | 0         |
| `wetLevel`        | Optional | The volume of the delay signal                                                                                                                                                                                                                                                                                                                                                                                                                                                         | 0 - 1.0                                               | 0.5       |


Example of using the delay effect when specifying time in seconds:

```xml
<DecentSampler>
  <effects>
    <effect type="delay" delayTime="0.5" stereoOffset="0.01" feedback="0.2" wetLevel="0.5" />
  </effects>
</DecentSampler>
```

Example of using the delay effect when specifying time in musical time:

```xml
<DecentSampler>
  <ui>
    <tab>
      <labeled-knob x="180" y="40" label="Delay Time" valueType="musical_time" 
                    minValue="0" maxValue="20" value="10" defaultValue="10">
        <binding type="effect" level="instrument" position="0" parameter="FX_DELAY_TIME" />
      </labeled-knob>
    </tab>
  </ui>
  <effects>
    <effect type="delay" delayTimeFormat="musical_time" delayTime="10" stereoOffset="0.01" 
            feedback="0.3" wetLevel="0.5" />
  </effects>
</DecentSampler>
```

### Chorus effect

Example:
```xml
<DecentSampler>
  <effects>
    <effect type="chorus" mix="0.5" modDepth="0.2" modRate="0.2"/>
  </effects>
</DecentSampler>
```

Attributes:

| Attribute  |          | Type                        | Valid Range   | Default  |
| ---------- | -------- | --------------------------- | ------------- | -------- |
| `type`     | Required | Must be `chorus`            | `chorus`      |          |
| `mix`      | Optional | The wet/dry mix which controls how much of the chorus signal we hear | 0 - 1.0, where 1.0 is just chorus, 0 is just dry signal.     | 0.5      |
| `modDepth` | Optional | The modulation depth of the effect    | 0 - 1.0, where 0 is no modulation, 1.0 is max modulation. | 0.2      |
| `modRate`  | Optional | The modulation speed in Hz. | 0 - 10.0       | 0.2        |

### Phaser effect

Example:
```xml
<DecentSampler>
  <effects>
    <effect type="phaser" mix="0.5" modDepth="0.2" modRate="0.2" centerFrequency="400" feedback="0.7" />
  </effects>
</DecentSampler>
```

Attributes:

| Attribute         |          | Type                        | Valid Range   | Default  |
| ----------------- | -------- | --------------------------- | ------------- | -------- |
| `type`            | Required | Must be `phaser`            | `phaser`      |          |
| `mix`             | Optional | The wet/dry mix which controls how much of the chorus signal we hear | 0 - 1.0, where 1.0 is just chorus, 0 is just dry signal.     | 0.5      |
| `modDepth`        | Optional | The modulation depth of the effect    | 0 - 1.0, where 0 is no modulation, 1.0 is max modulation. | 0.2      |
| `modRate`         | Optional | The modulation speed in Hz. | 0 - 10.0       | 0.2        |
| `centerFrequency` | Optional | The center frequency (in Hz) of the phaser all-pass filters modulation | 0 - 1.0       | 400        |
| `feedback`        | Optional | Sets the feedback volume of the phaser. | -1 - 1.0       | 0.7        |

### Convolution effect

This effect allows you to use a convolution reverb or amp simulation to your sample library. Depending on the length of the impulse response, the convolution effect can use substantial CPU, so you'll definitely want to do some testing both with and without the convolution effect turned on.

Example:
```xml
<DecentSampler>
  <effects>
    <effect type="convolution" mix="0.5" irFile="Samples/Hall 3.wav" />
  </effects>
</DecentSampler>
```

Attributes:

| Attribute |          | Type                                                                | Valid Range                                                   | Default    |
|:----------|:---------|:--------------------------------------------------------------------|:--------------------------------------------------------------|:-----------|
| `type`    | Required | Must be `convolution`                                               | `convolution`                                                 |            |
| `mix`     | Optional | The wet/dry mix controls how much of the convolution signal we hear | 0 - 1.0, where 1.0 is just convolution, 0 is just dry signal. | 0.5        |
| `irFile`  | Required | The path of the WAV or AIFF to use as an Impulse Response (IR) file | String                                                        | No default |

### Wave Folder effect

Introduced in version 1.7.2.  This effect allows you to fold a waveform back on itself. This is very useful for generating additional harmonic content.

Attributes:

| Attribute   |          | Type                                                     | Valid Range                                                                                                 | Default |
|:------------|:---------|:---------------------------------------------------------|:------------------------------------------------------------------------------------------------------------|:--------|
| `type`      | Required | Must be `wave_folder`                                    | `wave_folder`                                                                                               |         |
| `drive`     | Optional | The volume of the input signal                           | 1 - 100, where 100 means the signal is amplified by a factor of 100 and 1 means no amplification is applied | 1       |
| `threshold` | Optional | The amplitude above which wave folding should take place | 0 - 10.0                                                                                                    | 0.25    |

Because wave folding tends to sound better when applied on a per-voice basis, it usually makes sense to set up the wave folder at the group level (separate group effects get created for each keypress). Example:

```xml
<DecentSampler pluginVersion="1">
  <ui>
    <tab>
      <labeled-knob x="180" y="40" label="Drive" type="float" minValue="1" maxValue="100" textColor="FF000000" value="1">
        <binding type="effect" level="group" groupIndex="0" effectIndex="0" parameter="FX_DRIVE" translation="linear" />
      </labeled-knob>
      <labeled-knob x="280" y="40" label="Threshold" type="float" minValue="0" maxValue="1" value="1" textColor="FF000000">
        <binding type="effect" level="group" groupIndex="0" effectIndex="0" parameter="FX_THRESHOLD" translation="linear" />
      </labeled-knob>
    </tab>
  </ui>
  <groups>
    <group>
        <!-- Samples go here. -->
    <effects>
        <effect type="wave_folder" drive="1" threshold="1" />
      </effects>
    </group>
  </groups>
```
### Wave Shaper effect

Introduced in version 1.7.2. This effect allows you to distort an audio signal. This is very useful for generating additional harmonic content.

Attributes:

| Attribute     |          | Type                                                                                                                                      | Valid Range                                                                                                    | Default |
|:--------------|:---------|:------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------|:--------|
| `type`        | Required | Must be `wave_shaper`                                                                                                                     | `wave_folder`                                                                                                  |         |
| `drive`       | Optional | The amount of distortion. This really just controls the volume of the input signal. The volume of the input signal                        | 1 - 1000, where 1000 means the signal is amplified by a factor of 1000 and 1 means no amplification is applied | 1       |
| `driveBoost`  | Optional | Introduces an extra gain boost to the drive                                                                                               | 0 - 1.0                                                                                                        | 1       |
| `outputLevel` | Optional | The linear output level of the signal                                                                                                     | 0 - 1.0                                                                                                        | 0.1     |
| `highQuality` | Optional | Whether or not oversampling is performed. Oversampling sounds better, but it's CPU intensive. If you want to save CPU, set this to false. | true, false                                                                                                    | true    |

Because wave shaping tends to sound better when applied on a per-voice basis, it usually makes sense to set up the wave shaper at the group level (separate group effects get created for each keypress). Example:

```xml
<DecentSampler pluginVersion="1">
  <ui>
    <tab>
      <labeled-knob x="180" y="40" label="Drive" type="float" minValue="1" maxValue="40" textColor="FF000000" value="0.5473124980926514">
        <binding type="effect" level="group" groupIndex="0" effectIndex="0" parameter="FX_DRIVE" translation="linear"/>
      </labeled-knob>
      <labeled-knob x="280" y="40" label="Drive Boost" type="float" minValue="0" maxValue="1" value="0.328312486410141" textColor="FF000000">
        <binding type="effect" level="group" groupIndex="0" effectIndex="0" parameter="FX_DRIVE_BOOST" translation="linear"/>
      </labeled-knob>
      <labeled-knob x="380" y="40" label="Output Lvl" type="float" minValue="0" maxValue="1" value="0.328312486410141" textColor="FF000000">
        <binding type="effect" level="group" groupIndex="0" effectIndex="0" parameter="FX_OUTPUT_LEVEL" translation="linear"/>
      </labeled-knob>
    </tab>
  </ui>
  <groups>
    <group>
        <!-- Samples go here. -->
      <effects>
        <effect type="wave_shaper" drive="0.5473124980926514" shape="0.328312486410141" outputLevel="0.1"/>
      </effects>
    </group>
  </groups>
```