The &lt;midi&gt; element
========================

MIDI mappings can be added to your instrument by adding a `<midi>` element right below your top-level `<DecentSampler>` element. 


## The &lt;cc&gt; element
Within the `<midi>` element, you can have any number of `<cc>` elements. These allow you to map changes in incoming continuous controller messages to specific parameters of your instrument. To use this functionality, you'll want to add a separate `<cc>` element for each CC number you would like to respond to. The `<cc>` element has a single required attribute `number=""` which specifies the number (from 0 to 127) of the continuous controller you would like to listen on. Beneath the `<cc>` element, you can have any number of bindings. 

```xml
<midi>
  <cc number="11">
    <binding level="ui" type="control" position="0" parameter="VALUE" translation="linear" 
             translationOutputMin="0" translationOutputMax="1"/>
  </cc>
  <cc number="1">
    <binding level="ui" type="control" position="1" parameter="VALUE" translation="linear" 
             translationOutputMin="0" translationOutputMax="1"/>
  </cc>
</midi>
```

## The &lt;note&gt; element
Within the `<midi>` element, you can have any number of `<note>` elements. These allow you to map specific notes to specific parameters of your instrument. To use this functionality, you'll want to add a separate `<note>` element for each MIDI note or range of notes you would like to respond to.


Here are the attributes of the `<note />` element:

- **note** (required): This attribute specifies the MIDI note number (from 0 to 127) you would like to listen on. You can also specify ranges of notes by using a dash. For example `note="24-35"` would be used to specify bindings for the range of notes 24 thorugh 35. 
- **eventType** (optional): This attribute specifies the type of event to listen for: `note_on`, `note_off`, `any` (the default, both), or, from Decent Sampler 1.34.0, `first_note_on` and `last_note_off` (see below).
- **enabled** (optional): A true/false value that specifies whether this note listener is turned on.
- **swallowNotes** (optional): The bindings that live below this note listener are called before any notes are played. By default, swallowNotes is false, which means that the keypress will then be received by the sampler. If `swallowNotes` is true, the sampler will not receive the note. This is useful if you wish to prevent certain keys from triggers notes.

It is possible to enable and disable a note listener by targeting the `enabled` attribute.

Beneath the `<note>` element, you can have any number of bindings. Here is an example of how keyswitches might be set up:

```xml
<midi>
  <note note="11" enabled="true" eventType="note_on">
    <binding enabled="true" type="general" level="group" groupIndex="0" parameter="ENABLED" translation="fixed_value" translationValue="true" />
    <binding enabled="true" type="general" level="group" groupIndex="1" parameter="ENABLED" translation="fixed_value" translationValue="false" />
  </note>
  <note note="12" enabled="true" eventType="note_on">
    <binding enabled="true" type="general" level="group" groupIndex="0" parameter="ENABLED" translation="fixed_value" translationValue="false" />
    <binding enabled="true" type="general" level="group" groupIndex="1" parameter="ENABLED" translation="fixed_value" translationValue="true" />
  </note>
</midi>
```

In the above keyswitch example, MIDI note 11 turns on group 0 and turns off group 1, whereas MIDI note 12 does the opposite. Note the use of the `fixed_value` translation type.

### Responding to the first and last held note

`note_on` and `note_off` fire for every key. Sometimes you want something to happen once for a whole run of overlapping notes instead: when the first key goes down after a silence, or when the last held key is let go. That's what `first_note_on` and `last_note_off` do. They fire only when the note range goes from no keys held to one, and from one key held to none. Requires Decent Sampler 1.34.0.

For example, this starts an animation when playing begins and stops it when the last key is released, however many notes overlap in between:

```xml
<midi>
  <note note="0-127" eventType="first_note_on">
    <binding type="control" level="ui" position="0" parameter="VALUE" translation="fixed_value" translationValue="1" />
  </note>
  <note note="0-127" eventType="last_note_off">
    <binding type="control" level="ui" position="0" parameter="VALUE" translation="fixed_value" translationValue="0" />
  </note>
</midi>
```

These events count keys, not sounding notes, so holding the sustain pedal doesn't keep a run going: `last_note_off` fires when the last key comes up, even if notes are still ringing. An All Notes Off message (for instance when the host stops playback) also ends the run. `swallowNotes` works with `first_note_on` the same way it does with `note_on`.

## The &lt;velocity&gt; element
Within the `<midi>` element, you can also have a `<velocity>` element. This element allows you to control an instrument in response to MIDI velocity messages. This is useful for creating dynamic responses based on how hard a note is played.

Example usage:

```xml
<midi>
    <velocity>
      <binding modAmount="0.3" level="group" parameter="FX_FILTER_FREQUENCY"
               groupIndex="0" effectIndex="0" type="effect"/>
    </velocity>
  </midi>
```

In this example, the `<velocity>` element contains a single `<binding>` that modifies the `FX_FILTER_FREQUENCY` parameter of the first effect (effectIndex: 0) in the first group (groupIndex: 0) based on the velocity of incoming MIDI notes. The `modAmount` attribute specifies how much the velocity will affect the parameter.

### Bindings within the `<midi>` section

The bindings that the `<cc>`, `<note>`, and `<velocity>` element listens on are the same as those used by the UI controls. See [Appendix B](#appendix-b-the-binding-element) for a complete description of these.

If you have a UI control mapped to the same internal parameter as a MIDI mapping, you'll want to have your MIDI mapping control the UI control instead of the parameter directly. The benefit of doing this is that, as the MIDI CC input is received, the UI control will be updated as well as the desired internal parameter. 

The way to accomplish this is to make use of the `labeled_knob` or `control` binding types (`control` was introduced in version 1.1.7) as follows: 

```xml
<binding level="ui" type="control" position="0" parameter="VALUE" translation="linear" translationOutputMin="0" translationOutputMax="1"/>
```

You'll notice that the `control` type has a `level` value of `ui` and a `parameter` value of `VALUE`. Another thing to notice is the `position=""` parameter. This contains the 0-based index of the control to be modified. **NOTE: The indexes of the parameter list includes all UI controls, including `<label>` and `menu` controls, so you'll want to account for that when calculating your positions.** 

An example of changing a menu option based on a MIDI note (keyswitch) would look like this:
```xml
<midi>
  <note note="11" eventType="note_on">
    <binding type="control" level="ui" position="1" parameter="VALUE" translation="fixed_value" translationValue="1" />
  </note>
  <note note="12" eventType="note_on">
    <binding type="control" level="ui" position="1" parameter="VALUE" translation="fixed_value" translationValue="2" />
  </note>
</midi>
```