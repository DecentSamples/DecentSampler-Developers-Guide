The &lt;modulators&gt; element
==============================

Version 1.6.0 of Decent Sampler officially introduces the new `<modulators>` section into the `.dspreset` format. This section lives below the top-level `<DecentSampler>` element and it is where all modulators for the entire sample library live.

```xml
<DecentSampler>
    <modulators>
        <!-- Your modulators go here. -->
    </modulators>
</DecentSampler>
```

## The &lt;lfo&gt; element

Underneath the `<modulators>` section, you can have any number of different LFOs, which are defined using an `<lfo>` element, for example: 

```xml
<modulators>
  <lfo shape="sine" frequency="2" modAmount="1.0"></lfo>
</modulators>
```

For an LFO with a 500ms delay before starting:

```xml
<modulators>
  <lfo shape="sine" frequency="2" modAmount="1.0" delayTime="0.500"></lfo>
</modulators>
```

This element has the following attributes:

- **`shape`**: controls the oscillator shape. Possible values are `sine`, `square`, `saw`. 
- **`frequency`**: The speed of the LFO in cycles per second. For example, a value of 10 would mean that the waveform repeats ten times per second.
- **`modAmount`**: This value between 0 and 1 controls how much the  modulation affects the things it is targeting. In conventional terms, this is like the modulation depth. Default value: 1.0.
- **`delayTime`**: The time in seconds to wait before the LFO starts outputting signal. During this delay period, the LFO outputs zero. Default value: 0.0 (no delay).
- **`scope`**: Whether or not this LFO exists for all notes or whether each keypress gets its own LFO. Possible values are `global` (default for LFOs) and `voice`. If `voice` is chosen, a new LFO is started each time a new note is pressed.
- **`modBehavior`**: This attribute controls how the LFO affects the parameter it is targeting. Possible values are `add`, `multiply`, and `set`. If `add` is chosen, the LFO will add its value to the parameter it is targeting. If `multiply` is chosen, the LFO will multiply its value by the parameter it is targeting. If `set` is chosen, the LFO will set the parameter it is targeting to its value. Default value: `set`. 

## The &lt;envelope&gt; element

In addition to LFOs, you can also have additional ADSR envelopes. These can be useful for controlling group-level effects, such as low-pass filters. If this is what you wish to achieve, make sure you check out the section on group-level effects below.

To create an envelope, use an `<envelope>` element:

This element has the following attributes:
- **`attack`**: The length in seconds of the attack portion of the ADSR envelope
- **`decay`**: The length in seconds of the decay portion of the ADSR envelope
- **`sustain`**: The height of the sustain portion of the ADSR envelope. This is expressed as a value between 0 and 1. 
- **`release`**: The length in seconds of the release portion of the ADSR envelope
- **`modAmount`**: This value between 0 and 1 controls how much the  modulation affects the things it is targeting. In conventional terms, this is like the modulation depth. Default value: 1.0.
- **`scope`**: Whether or not this LFO exists for all notes or whether each keypress gets its own LFO. Possible values are `global` and `voice` (default for envelopes). If `voice` is chosen, a new LFO is started each time a new note is pressed.
- **`modBehavior`**: This attribute controls how the envelope affects the parameter it is targeting. Possible values are `add`, `multiply`, and `set`. If `add` is chosen, the envelope will add its value to the parameter it is targeting. If `multiply` is chosen, the envelope will multiply its value by the parameter it is targeting. If `set` is chosen, the envelope will set the parameter it is targeting to its value. Default value: `set`.
- **`attackCurve`**: A numeric value from -100 to 100 that determines the shape of the attack portion of the ADSR envelope. Common values are `-100` (logarithmic), `0` (linear), and `100` (exponential). Default value: `-100` (logarithmic).
- **`decayCurve`**: A numeric value from -100 to 100 that determines the shape of the decay portion of the ADSR envelope. Common values are `-100` (logarithmic), `0` (linear), and `100` (exponential). Default value: `100` (exponential).
- **`releaseCurve`**: A numeric value from -100 to 100 that determines the shape of the release portion of the ADSR envelope. Common values are `-100` (logarithmic), `0` (linear), and `100` (exponential). Default value: `100` (exponential).

## The &lt;mpeTimbre&gt; element

The `<mpeTimbre>` element allows users to control the timbre of an instrument in response to MPE messages. NOTE: In order for this to work, the plugin must be in MPE mode. This can be turned on by going into the **File > MIDI Input Settings..** dialog box. The `<mpeTimbre>` element has the following attributes:
- **`scope`**: Whether or not this MPE timbre exists for all notes or whether each keypress gets its own MPE timbre. Possible values are `global` and `voice` (default for MPE timbre). If `voice` is chosen, a new MPE timbre is started each time a new note is pressed.
- **`risingSmoothingTime`**: The time in milliseconds it takes for the MPE timbre to rise to its target value. Default value: 0 milliseconds.
- **`fallingSmoothingTime`**: The time in milliseconds it takes for the MPE timbre to fall to its target value. Default value: 0 milliseconds.

Here's a practical example of how to use the `<mpeTimbre>` element:

```xml
<modulators>
    <mpeTimbre scope="voice" fallingSmoothingTime="20" risingSmoothingTime="20">
        <!-- This binding modifies the frequency of a low-pass filter -->
        <binding type="effect" level="group" groupIndex="0" effectIndex="0" parameter="FX_FILTER_FREQUENCY"
            modBehavior="add"
            translation="table"
            translationTable="0,33;0.3,150;0.4,450;0.5,1100;0.7,4100;0.9,11000;1.0001,22000" />
    </mpeTimbre>
</modulators>
```

This example shows an MPE timbre modulator that modifies the frequency of a low-pass filter based on the MPE timbre value. This example uses a translation table to create a non-linear response curve, making the filter frequency changes more musical and intuitive.

## The &lt;mpePressure&gt; element

The `<mpePressure>` element allows users to control an instrument in response to MPE Pressure messages. NOTE: In order for this to work, the plugin must be in MPE mode. This can be turned on by going into the **File > MIDI Input Settings..** dialog box. The `<mpePressure>` element has the following attributes:

- **`scope`**: Whether or not this MPE pressure exists for all notes or whether each keypress gets its own MPE pressure. Possible values are `global` and `voice` (default for MPE pressure). If `voice` is chosen, a new MPE pressure is started each time a new note is pressed.
- **`risingSmoothingTime`**: The time in milliseconds it takes for the MPE pressure to rise to its target value. Default value: 0 milliseconds.
- **`fallingSmoothingTime`**: The time in milliseconds it takes for the MPE pressure to fall to its target value. Default value: 0 milliseconds.

Here's a practical example of how to use the `<mpePressure>` element:

```xml
<modulators>
    <mpePressure scope="voice" fallingSmoothingTime="20" risingSmoothingTime="20">
        <binding type="amp" level="group" groupIndex="0" effectIndex="0" parameter="AMP_VOLUME" 
            modBehavior="set"
            translation="linear"
            translationOutputMin="0.5"
        translationOutputMax="1"  />
    </mpePressure>
</modulators>
```

This example shows an MPE pressure modulator that modifies the volume of a group based on the MPE pressure value. The translation is linear, meaning that the pressure value directly scales the volume between 0.5 and 1.

## The &lt;random&gt; element

The `<random>` element allows you to generate random values for modulation. This can be useful for adding unpredictability and variation to your sounds. The `<random>` element always produces a value between -1 and 1. It has the following attributes:

- **`mode`**: The event that triggers the random value generation. This can be either `note_on` or `periodic`.
- **`frequency`**: How often the random value is generated. This is only relevant if the `mode` is set to `periodic`.
- **`trigger`**: Two possible values: `attack` means that the random value generator is reset on note-on events, `none` means that it is not. This is really only help for periodic random value generators.
- **`seed`**: An integer seed value for the random number generator. This can be useful for creating reproducible results.
- **`scope`**: Whether or not this modulator exists for all notes or whether each keypress gets its own modulator. Possible values are `global` and `voice`. If `voice` is chosen, a new random modulator is started each time a new note is pressed.

Here's an example of how to use the `<random>` element:

```xml
<modulators>
    <random mode="note_on" seed="12345" scope="voice">
        <binding type="amp" level="group" groupIndex="0" effectIndex="0" parameter="AMP_VOLUME" 
            modBehavior="set"
            translation="linear"
            translationOutputMin="0.5"
            translationOutputMax="1"  />
    </random>
</modulators>

```

## How to use &lt;binding&gt;s in conjunction with modulators

In order to actually have your LFOs and envelopes do anything, you need to have bindings under them. If you are not familiar with the concept of bindings, you may want to read [this section](https://www.decentsamples.com/wp-content/uploads/2020/06/format-documentation.html#appendix-b-the-binding-element) then return here. Bindings tell the engine which parameters the LFO should be affecting and how. Here is an example:

```xml
<modulators>
    <lfo shape="sine" frequency="2" modAmount="1.0">
        <!-- This binding modifies the frequency of a low-pass filter  -->
        <binding type="effect" level="instrument" effectIndex="0" parameter="FX_FILTER_FREQUENCY" modBehavior="add" translation="linear" translationOutputMin="0" translationOutputMax="2000.0"  />
    </lfo>
</modulators>
```

### Controlling LFO Parameters with Bindings

You can also bind to LFO parameters themselves to control them in real-time. For example, to control the delay time of an LFO with a UI knob:

```xml
<DecentSampler>
  <ui>
    <tab>
      <labeled_knob x="10" y="30" width="90" textSize="16" textColor="AA000000" 
                    minValue="0" maxValue="2000" value="500">
        <binding type="modulator" level="instrument" parameter="MOD_DELAY_TIME" 
                 modulatorIndex="0" translation="linear" />
      </labeled_knob>
    </tab>
  </ui>
  <modulators>
    <lfo shape="sine" frequency="2" modAmount="1.0" delayTime="0.500">
      <binding type="effect" level="instrument" effectIndex="0" parameter="FX_FILTER_FREQUENCY" 
               modBehavior="add" translation="linear" translationOutputMin="0" translationOutputMax="2000.0" />
    </lfo>
  </modulators>
</DecentSampler>
```

You can also control the delay time via MIDI CC:

```xml
<DecentSampler>
  <midi>
    <cc number="74">
      <binding type="modulator" level="instrument" parameter="MOD_DELAY_TIME" 
               modulatorIndex="0" translation="linear" translationOutputMin="0" translationOutputMax="2000" />
    </cc>
  </midi>
  <modulators>
    <lfo shape="sine" frequency="2" modAmount="1.0" delayTime="0.500">
      <binding type="effect" level="instrument" effectIndex="0" parameter="FX_FILTER_FREQUENCY" 
               modBehavior="add" translation="linear" translationOutputMin="0" translationOutputMax="2000.0" />
    </lfo>
  </modulators>
</DecentSampler>
```

There are a few differences between bindings as they are used by knobs and the ones used by modulators. Specifically, when you move a UI control that has a binding attached, the engine actually goes out and changes the value of the parameter that is targeted by that binding. For example, if you have a knob that controls a lowpass filter's cutoff frequency, moving that knob will cause that actual frequency of that filter to change. In other words, the changes that the knob is making on the underlying sample library are _permanent_. The same is also true for bindings associated with MIDI continuous controllers. 

Modulators, on the other hand, are temporary. If a modulator (such as an LFO) changes its value, the engine looks at the bindings associated with that LFO and then makes a list of _temporary_ changes to the underlying data. When it comes time to render out the effect, it consults both the _permanent_ value and the _temporary_ modulation values. As a result of this difference in the way bindings are handled, only some parameters are "modulatable." At time of press, the following parameters are modulatable:

- Almost all effect parameters
- Group Volume
- Global Volume
- Group Pan
- Global Pan
- Group Tuning
- Global Tuning

## Modulator scope: global or voice-level

By default, all modulators will be created at the global level. This means that there will be exactly one modulator that is shared by all voices. In many situations, such as an LFO modulating a single low-pass filter which is shared by all of voices, this is often what we want. 

But there are other situations where we don’t want our modulator to be global. For example, what if we want to have a unique envelope for each key-press? Well, for that we use the `scope` attribute of the `<lfo>` or `<envelope>` element. This attribute can be set to `voice`, which means that each time a new note is pressed, a new modulator will be created for that note. This is particularly useful for envelopes, which are often used to control parameters that are unique to each note, such as the volume or filter cutoff of a note:

```xml
<envelope attack="2" decay="0" sustain="1" release="0.5" modAmount="1.0" scope="voice">
```

For certain parameters, such as **Group Tuning** or **Group Pan**, it may always make sense to have `scope="voice"` set. For others, such as **Global Volume** or **Global Pan**, it may make more sense to have `scope="global"` set.

For a full discussion of how to use modulators, check out [this article on the Decent Samples blog](https://www.decentsamples.com/2022/08/19/how-to-add-lfos-and-extra-envelopes-to-your-decent-sampler-instruments).