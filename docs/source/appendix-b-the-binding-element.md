# Appendix B: The &lt;binding&gt; element

Adding a binding to a UI control, MIDI handler, or modulator tells the DecentSampler engine that it should take input from a source and use it to change values in another part of the engine. An example of this would be a knob which controls the volume of a group, a CC controller that changes an effect parameter, or an LFO that modulates an effect parameter.

In order to set up a binding for a specific source, create a `<binding>` element within the source element. 

In this example, a labeled knob is controlling the volume of the first group of samples (group 0): 

```xml
<DecentSampler>
  <ui>
    <tab>
      <labeled-knob x="420" y="100" label="RT" type="float" minValue="0" maxValue="1" value="0.3" textSize="20">
        <binding type="amp" level="group" position="0" parameter="AMP_VOLUME" translation="linear" translationOutputMin="0" translationOutputMax="1.0"  />
      </labeled-knob>
    </tab>
  </ui>
</DecentSampler>
```

Here's a full list of parameters for the `<binding>` element:

| Attribute                  | Description                                                                                                                                                                                                                                                                                                         |          |
|:---------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------|
| **`type`**                 | This tells the engine what type of parameter this is. Valid values are: "amp", "effect", "control".                                                                                                                                                                                                                 | Required |
| **`level`**                | Valid values are `ui`, `instrument`, `group`                                                                                                                                                                                                                                                                        | Required |
| **`position`**             | The specific 0-based index of the element to be modified by this binding. If you are targeting a group, for example, the first group would be 0, the second group would be 1, etc.                                                                                                                                  | Required |
| **`controlIndex`**         | When a binding is targeting a control, this is the same thing as the `position` attribute. It is a specific 0-based index of the control to be modified by this binding. If you are targeting an group-level effect, this would specified the group under which the effect lives.                                   | Optional |
| **`groupIndex`**           | When a binding is targeting a group, this is the same thing as the `position` attribute. It is a specific 0-based index of the group to be modified by this binding. If you are targeting an group-level effect, this would specified the group under which the effect lives.                                       | Optional |
| **`effectIndex`**          | When a binding is targeting an effect, this is the same thing as the `position` attribute. It is a specific 0-based index of the effect to be modified by this binding.                                                                                                                                             | Optional |
| **`modulatorIndex`**       | When a binding is targeting a modulator, this is the same thing as the `position` attribute. It is a specific 0-based index of the modulator to be modified by this binding.                                                                                                                                        | Optional |
| **`tags`**                 | A comma-separated list of tags to be modified by this binding. This allows you to set values for multiple groups at once by targeting a tag that is assigned to the groups.                                                                                                                                         | Optional |
| **`identifier`**           | A string identifying the specific parameter that you wish to change. If you are modulating based on tags, you would put the tag you are targeting here. See Appendix D for example.                                                                                                                                 | Required |
| **`parameter`**            | A token describing the specific kind of parameter that you wish to change. A list of controller parameters are below.                                                                                                                                                                                               | Required |
| **`translation`**          | Valid values are `fixed_value`, `linear` and `table`. Explanation of both translation modes is in a separate section below. Default: `linear`                                                                                                                                                                       | Optional |
| **`translationOutputMin`** | This is the min value this binding should send to the target parameter. This is only looked at if translation is set to `linear`.                                                                                                                                                                                   | Optional |
| **`translationOutputMax`** | This is the max value this binding should send to the target parameter. This is only looked at if translation is set to `linear`.                                                                                                                                                                                   | Optional |
| **`translationReversed`**  | Valid values are `true` and `false`. Default: `false`. This is only looked at if translation is set to `linear`.                                                                                                                                                                                                    | Optional |
| **`translationTable`**     | A list of input-output pairs that make up the translation table. The input and output are separated by commas. The groups of coordinates themselves are separated by semi-colons. Default: `0,0;1,1`. You must have at least two coordinates in your list. This is only looked at if translation is set to `table`. | Optional |
| **`translationValue`**     | The value that should be passed along when `translation` is set to `fixed_value`.                                                                                                                                                                                                                                   | Optional |


## Controllable Parameters

This is a list of parameters that can be used in conjunction with the `<binding>` element above.

| Description                             | `type`      | `level`                 | `parameter`            | Valid Range                  | Modulatable | Additional required parameters                                                                        |
|:----------------------------------------|:------------|:------------------------|:-----------------------|:-----------------------------|:------------|:------------------------------------------------------------------------------------------------------|
| Global Volume                           | `amp`       | `instrument`            | `AMP_VOLUME`           | 0.0 - 16.0                   | No          | N/A                                                                                                   |
| Global Tuning                           | `amp`       | `instrument`            | `GLOBAL_TUNING`        | -36.0 - 36.0                 | No          | N/A                                                                                                   |
| Global Pan                              | `amp`       | `instrument`            | `PAN`                  | -100 - 100                   | No          | N/A                                                                                                   |
| Sample Start (see note 2 below)         | `general`   | `instrument` or `group` | `SAMPLE_START`         | 0 - last sample              | No          | This value will be in number of raw samples where 0 is the beginning. See note 2 below.               |
| Sample End (see note 2 below)           | `general`   | `instrument` or `group` | `SAMPLE_END`           | 0 - last sample              | No          | This value will be in number of raw samples where 0 is the beginning. See note 2 below.               |
| Loop Start (see note 2 below)           | `general`   | `instrument` or `group` | `LOOP_START`           | 0 - last sample              | No          | This value will be in number of raw samples where 0 is the beginning. See note 2 below.               |
| Loop End (see note 2 below)             | `general`   | `instrument` or `group` | `LOOP_END`             | 0 - last sample              | No          | This value will be in number of raw samples where 0 is the beginning. See note 2 below.               |
| Amplitude Velocity Tracking             | `amp`       | `instrument`            | `AMP_VEL_TRACK`        | 0.0 - 1.0                    | No          | N/A                                                                                                   |
| Global Amp Envelope Attack              | `amp`       | `instrument`            | `ENV_ATTACK`           | 0.0 - 10.0                   | No          | N/A                                                                                                   |
| Global Amp Envelope Attack Curve Shape  | `amp`       | `instrument`            | `ENV_ATTACK_CURVE`     | -100 - 100                   | No          | N/A                                                                                                   |
| Global Amp Envelope Decay               | `amp`       | `instrument`            | `ENV_DECAY`            | 0.0 - 25.0                   | No          | N/A                                                                                                   |
| Global Amp Envelope Decay Curve Shape   | `amp`       | `instrument`            | `ENV_DECAY_CURVE`      | -100 - 100                   | No          | N/A                                                                                                   |
| Global Amp Envelope Sustain             | `amp`       | `instrument`            | `ENV_SUSTAIN`          | 0.0 - 1.0                    | No          | N/A                                                                                                   |
| Global Amp Envelope Release             | `amp`       | `instrument`            | `ENV_RELEASE`          | 0.0 - 25.0                   | No          | N/A                                                                                                   |
| Global Amp Envelope Release Curve Shape | `amp`       | `instrument`            | `ENV_RELEASE_CURVE`    | -100 - 100                   | No          | N/A                                                                                                   |
| Glide/Portamento Time                   | `amp`       | `instrument`            | `GLIDE_TIME`           | 0.0 - 10.0                   | No          | N/A                                                                                                   |
| Effect Enabled (all effects)            | `effect`    | `instrument`            | `ENABLED`              | false, true                  | Yes         | `effectIndex` or `position` contains the 0-based index of the effect                                  |
| Convolution Mix Level                   | `effect`    | `instrument`            | `FX_MIX`               | 0.0 - 1.0                    | Yes         | `effectIndex` or `position` contains the 0-based index of the effect                                  |
| Convolution IR File                     | `effect`    | `instrument`            | `FX_IR_FILE`           | Text                         | No          | `effectIndex` or `position` contains the 0-based index of the effect                                  |
| Filter Frequency (for several filters)  | `effect`    | `instrument`            | `FX_FILTER_FREQUENCY`  | 0.0 - 22000.0                | Yes         | `effectIndex` or `position` contains the 0-based index of the effect                                  |
| Peak or Notch Filter Q                  | `effect`    | `instrument`            | `FX_FILTER_Q`          | 0.01 - 18.0                  | Yes         | `effectIndex` or `position` contains the 0-based index of the effect                                  |
| Peak or Notch Filter Gain               | `effect`    | `instrument`            | `FX_FILTER_GAIN`       | 0.0 - 10.0                   | Yes         | `effectIndex` or `position` contains the 0-based index of the effect                                  |
| Low-pass or High-pass Filter Resonance  | `effect`    | `instrument`            | `FX_FILTER_RESONANCE`  | 0.0 - 5.0                    | Yes         | `effectIndex` or `position` contains the 0-based index of the effect                                  |
| Reverb Wet Level                        | `effect`    | `instrument`            | `FX_REVERB_WET_LEVEL`  | 0.0 - 1.0                    | Yes         | `effectIndex` or `position` contains the 0-based index of the effect                                  |
| Reverb Room Size                        | `effect`    | `instrument`            | `FX_REVERB_ROOM_SIZE`  | 0.0 - 1.0                    | Yes         | `effectIndex` or `position` contains the 0-based index of the effect                                  |
| Reverb Damping                          | `effect`    | `instrument`            | `FX_REVERB_DAMPING`    | 0.0 - 1.0                    | Yes         | `effectIndex` or `position` contains the 0-based index of the effect                                  |
| Chorus/Phaser/Convolution Mix Level     | `effect`    | `instrument`            | `FX_MIX`               | 0.0 - 1.0                    | Yes         | `effectIndex` or `position` contains the 0-based index of the effect                                  |
| Chorus/Phaser Mod Depth                 | `effect`    | `instrument`            | `FX_MOD_DEPTH`         | 0.0 - 1.0                    | Yes         | `effectIndex` or `position` contains the 0-based index of the effect                                  |
| Chorus/Phaser Mod Rate                  | `effect`    | `instrument`            | `FX_MOD_RATE`          | 0.0 - 10.0                   | Yes         | `effectIndex` or `position` contains the 0-based index of the effect                                  |
| Phaser Center Frequency                 | `effect`    | `instrument`            | `FX_CENTER_FREQUENCY`  | 0.0 - 22000.0                | Yes         | `effectIndex` or `position` contains the 0-based index of the effect                                  |
| Phaser/Delay Feedback                   | `effect`    | `instrument`            | `FX_FEEDBACK`          | 0.0 - 1.0                    | Yes         | `effectIndex` or `position` contains the 0-based index of the effect                                  |
| Delay Time                              | `effect`    | `instrument`            | `FX_DELAY_TIME`        | 0.0 - 1.0                    | Yes         | `effectIndex` or `position` contains the 0-based index of the effect                                  |
| Delay Time Format                       | `effect`    | `instrument`            | `FX_DELAY_TIME_FORMAT` | seconds, musical_time        | Yes         | `effectIndex` or `position` contains the 0-based index of the effect                                  |
| Delay Stereo Offset                     | `effect`    | `instrument`            | `FX_STEREO_OFFSET`     | 0.0 - 1.0                    | Yes         | `effectIndex` or `position` contains the 0-based index of the effect                                  |
| Delay Wet Level                         | `effect`    | `instrument`            | `FX_WET_LEVEL`         | 0.0 - 1.0                    | Yes         | `effectIndex` or `position` contains the 0-based index of the effect                                  |
| Gain Level                              | `effect`    | `instrument`            | `LEVEL`                | 0.0 - 8.0                    | Yes         | `groupIndex` or `position` contains the 0-based index of the group                                    |
| Wave Folder Drive Level                 | `effect`    | `instrument`            | `FX_DRIVE`             | 1 - 100                      | Yes         | `groupIndex` or `position` contains the 0-based index of the group                                    |
| Wave Folder Threshold                   | `effect`    | `instrument`            | `FX_THRESHOLD`         | 1 - 100                      | Yes         | `groupIndex` or `position` contains the 0-based index of the group                                    |
| Wave Shaper Drive Level                 | `effect`    | `instrument`            | `FX_DRIVE`             | 0.0 - 1000.0                 | Yes         | `groupIndex` or `position` contains the 0-based index of the group                                    |
| Wave Shaper Output Level                | `effect`    | `instrument`            | `FX_OUTPUT_LEVEL`      | 0.0 - 8.0                    | Yes         | `groupIndex` or `position` contains the 0-based index of the group                                    |
| Wave Shaper Drive Boost                 | `effect`    | `instrument`            | `FX_DRIVE_BOOST`       | 0.0 - 1.0                    | Yes         | `groupIndex` or `position` contains the 0-based index of the group                                    |
| Group Enabled / Disabled                | `amp`       | `group`                 | `ENABLED`              | true, false                  |             | `groupIndex` or `position` contains the 0-based index of the group                                    |
| Group Volume                            | `amp`       | `group`                 | `AMP_VOLUME`           | 0.0 - 16.0                   | Yes         | `groupIndex` or `position` contains the 0-based index of the group                                    |
| Group Tuning                            | `amp`       | `group`                 | `GROUP_TUNING`         | -36.0 - 36.0                 | Yes         | `groupIndex` or `position` contains the 0-based index of the group                                    |
| Pan                                     | `amp`       | `group`                 | `PAN`                  | -100 - 100                   | Yes         | `groupIndex` or `position` contains the 0-based index of the group                                    |
| Amplitude Velocity Tracking             | `amp`       | `group`                 | `AMP_VEL_TRACK`        | 0.0 - 1.0                    |             | `groupIndex` or `position` contains the 0-based index of the group                                    |
| Group Amp Envelope Attack               | `amp`       | `group`                 | `ENV_ATTACK`           | 0.0 - 10.0                   |             | `groupIndex` or `position` contains the 0-based index of the group                                    |
| Group Amp Envelope Decay                | `amp`       | `group`                 | `ENV_DECAY`            | 0.0 - 25.0                   |             | `groupIndex` or `position` contains the 0-based index of the group                                    |
| Group Amp Envelope Sustain              | `amp`       | `group`                 | `ENV_SUSTAIN`          | 0.0 - 1.0                    |             | `groupIndex` or `position` contains the 0-based index of the group                                    |
| Group Amp Envelope Release              | `amp`       | `group`                 | `ENV_RELEASE`          | 0.0 - 25.0                   |             | `groupIndex` or `position` contains the 0-based index of the group                                    |
| Group Glide/Portamento Time             | `amp`       | `group`                 | `GLIDE_TIME`           | 0.0 - 10.0                   |             | `groupIndex` or `position` contains the 0-based index of the group                                    |
| Tag Enabled                             | `amp`       | `tag`                   | `TAG_ENABLED`          | true, false                  |             | `identifier` contains the tag name                                                                    |
| Tag Volume                              | `amp`       | `tag`                   | `TAG_VOLUME`           | 0.0 - 16.0                   |             | `identifier` contains the tag name                                                                    |
| UI Control Visibile                     | `control`   | `ui`                    | `VISIBLE`              | true, false                  |             | `controlIndex` or `position` contains the 0-based index of the control in question (see note 1 below) |
| UI Control Value                        | `control`   | `ui`                    | `VALUE`                | Any number                   |             | `controlIndex` or `position` contains the 0-based index of the control in question (see note 1 below) |
| UI Control Text                         | `control`   | `ui`                    | `TEXT`                 | Text                         |             | `controlIndex` or `position` contains the 0-based index of the control in question (see note 1 below) |
| UI Control Minimum Value                | `control`   | `ui`                    | `MIN_VALUE`            | Any number                   |             | `controlIndex` or `position` contains the 0-based index of the control in question (see note 1 below) |
| UI Control Maximum Value                | `control`   | `ui`                    | `MAX_VALUE`            | Any number                   |             | `controlIndex` or `position` contains the 0-based index of the control in question (see note 1 below) |
| UI Control Value Type                   | `control`   | `ui`                    | `VALUE_TYPE`           | float, integer, musical_time |             | `controlIndex` or `position` contains the 0-based index of the control in question (see note 1 below) |
| UI Image Control Path                   | `control`   | `ui`                    | `PATH`                 | Text                         |             | `controlIndex` or `position` contains the 0-based index of the control in question (see note 1 below) |
| Modulator Amount (Depth)                | `modulator` | `instrument`            | `MOD_AMOUNT`           | 0.0 - 1.0                    |             | `modulatorIndex` contains the 0-based index of the modulator in question                              |
| LFO Modulator Rate (or Frequency)       | `modulator` | `instrument`            | `FREQUENCY`            | 0.0 - 22000.0                |             | `modulatorIndex` contains the 0-based index of the modulator in question                              |
| Envelope Modulator Attack               | `modulator` | `instrument`            | `ENV_ATTACK`           | 0.0 - 10.0                   |             | `modulatorIndex` contains the 0-based index of the modulator in question                              |
| Envelope Modulator Attack Curve Shape   | `modulator` | `instrument`            | `ENV_ATTACK_CURVE`     | -100 - 100                   |             | `modulatorIndex` contains the 0-based index of the modulator in question                              |
| Envelope Modulator Decay                | `modulator` | `instrument`            | `ENV_DECAY`            | 0.0 - 25.0                   |             | `modulatorIndex` contains the 0-based index of the modulator in question                              |
| Envelope Modulator Decay Curve Shape    | `modulator` | `instrument`            | `ENV_DECAY_CURVE`      | -100 - 100                   |             | `modulatorIndex` contains the 0-based index of the modulator in question                              |
| Envelope Modulator Sustain              | `modulator` | `instrument`            | `ENV_SUSTAIN`          | 0.0 - 1.0                    |             | `modulatorIndex` contains the 0-based index of the modulator in question                              |
| Envelope Modulator Release              | `modulator` | `instrument`            | `ENV_RELEASE`          | 0.0 - 25.0                   |             | `modulatorIndex` contains the 0-based index of the modulator in question                              |
| Envelope Modulator Release Curve Shape  | `modulator` | `instrument`            | `ENV_RELEASE_CURVE`    | -100 - 100                   |             | `modulatorIndex` contains the 0-based index of the modulator in question                              |
  

1. NOTE: The indexes of the parameter list also include UI controls that are not editable, such as `<label>` UI controls, so you'll want to account for that when calculating your positions. 

    Here's a quick example:

    If your UI's `<tab>` section has the following elements under it: `<label>`, `<control>`,`<label>`,`<control>`. The `position` indexes of the four elements will be 0, 1, 2, 3. Therefore, the indexes of the two `<control>` elements will be 1 and 3, respectively.

2. NOTE: If your sample library manipulates `start`, `end`, `loopStart`, or `loopEnd` after a sample library's initial load, the sample playback engine must be in RAM/Memory mode (not disk streaming) or you will get very unpredictable results. In order to enforce this, sample creators should use the `playbackEngine` attribute.

## Translation Modes

There are currently three binding translation modes: `linear`, `table`, `fixed_value`

### Mode #1: `linear`

**linear** mode allows values that come in to be scaled up or down before they get passed along to the binding's target. If you set your translation mode to `linear` you should also `translationOutputMin` and `translationOutputMax`.

Example usage:

```xml
<binding level="ui" type="control" position="0" parameter="value" translation="linear" 
               translationOutputMin="0" translationOutputMax="1"/>
```

### Mode #2: `table`

**table** mode allows you to transform the binding's input in a more complex fashion before it gets passed along to the binding's target. If you set your translation mode to `table` you must define the `translationTable` parameter as well. This consists of a series of input-output pairs, separated by semi-colons. 

### Mode #3: `fixed_value`

**fixed_value** mode allows you to completely disregard the input of a binding and instead always use a supplied value. In order to use this translation mode, you must also specify a `translationValue`. This can be very useful when trying to have menu options enable and disable groups. An example usage:

```xml
<binding type="general" level="group" position="1" parameter="ENABLED" translation="fixed_value" translationValue="true" />
```