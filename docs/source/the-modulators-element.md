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
<modulators>1
  <lfo shape="sine" frequency="2" modAmount="1.0"></lfo>
</modulators>
```

This element has the following attributes:

- **`shape`**: controls the oscillator shape. Possible values are `sine`, `square`, `saw`. 
- **`frequency`**: The speed of the LFO in cycles per second. For example, a value of 10 would mean that the waveform repeats ten times per second.
- **`modAmount`**: This value between 0 and 1 controls how much the  modulation affects the things it is targeting. In conventional terms, this is like the modulation depth. Default value: 1.0.
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

There are a few differences between bindings as they are used by knobs and the ones used by modulators. Specifically, when you move a UI control that has a binding attached, the engine actually goes out and changes the value of the parameter that is targeted by that binding. For example, if you have a knob that controls a lowpass filter's cutoff frequency, moving that knob will cause that actual frequency of that filter to change. In other words, the changes that the knob is making on the underlying sample library are _permanent_. The same is also true for bindings associated with MIDI continuous controllers. 

Modulators, on the other hand, do not work this way. If a modulator (such as an LFO) changes its value, the engine looks at the bindings associated with that LFO and then makes a list of _temporary_ changes to the underlying data. When it comes time to render out the effect, it consults both the _permanent_ value and the _temporary_ modulation values. As a result of this difference in the way bindings are handled, only some parameters are "modulatable." At time of press, the following parameters are modulatable:

- All gain effect parameters
- All delay effect parameters
- All phaser effect parameters
- All filter effect parameters
- All reverb effect parameters
- All chorus effect parameters
- Group Volume
- Global Volume
- Group Pan
- Global Pan
- Group Tuning
- Global Tuning

## Modulator scope: global or voice-level

By default, all modulators will be created at the global level. This means that there will be exactly one modulator that is shared by all voices. In many situations, such as an LFO modulating a single low-pass filter which is shared by all of voices, this is often what we want. 

But there are other situations where we don't want our modulator to be global. In such cases we 

```xml
<modulators>
    <envelope attack="2" decay="0" sustain="1" release="0.5" modAmount="1.0">
        <binding type="effect" level="group" groupIndex="0" effectIndex="0" parameter="FX_FILTER_FREQUENCY" translation="linear" translationOutputMin="0" translationOutputMax="4000.0" modBehavior="add" />
    </envelope>
</modulators>
```

Note that this voice-level modulator is now targeting a group level effect.
