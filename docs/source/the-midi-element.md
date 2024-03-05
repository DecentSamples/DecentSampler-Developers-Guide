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
Within the `<midi>` element, you can have any number of `<note>` elements. These allow you to map specific notes to specific parameters of your instrument. To use this functionality, you'll want to add a separate `<note>` element for each MIDI note you would like to respond to. The `<note>` element has a single required attribute `note=""` which specifies the number (from 0 to 127) of the MIDI note you would like to listen on. Beneath the `<note>` element, you can have any number of bindings. Here is an example of how keyswitches could be set up:

```xml
<midi>
  <note note="11">
    <binding type="general" level="group" position="0" parameter="ENABLED" translation="fixed_value" translationValue="true" />
    <binding type="general" level="group" position="1" parameter="ENABLED" translation="fixed_value" translationValue="false" />
  </note>
  <note note="12">
    <binding type="general" level="group" position="0" parameter="ENABLED" translation="fixed_value" translationValue="false" />
    <binding type="general" level="group" position="1" parameter="ENABLED" translation="fixed_value" translationValue="true" />
  </note>
</midi>
```

In the above keyswitch example, MIDI note 11 turns on group 0 and turns off group 1, whereas MIDI note 12 does the opposite. Note the use of the `fixed_value` translation type.

### Bindings within the `<midi>` section

The bindings that the `<cc>` and `<note>` element listens on are the same as those used by the UI controls. See [Appendix B](#appendix-b-the-binding-element) for a complete description of these.

If you have a UI control mapped to the same internal parameter as a MIDI mapping, you'll want to have your MIDI mapping control the UI control instead of the parameter directly. The benefit of doing this is that, as the MIDI CC input is received, the UI control will be updated as well as the desired internal parameter. 

The way to accomplish this is to make use of the `labeled_knob` or `control` binding types (`control` was introduced in version 1.1.7) as follows: 

```xml
<binding level="ui" type="control" position="0" parameter="VALUE" translation="linear" translationOutputMin="0" translationOutputMax="1"/>
```

You'll notice that the `control` type has a `level` value of `ui` and a `parameter` value of `VALUE`. Another thing to notice is the `position=""` parameter. This contains the 0-based index of the control to be modified. **NOTE: The indexes of the parameter list includes all UI controls, including `<label>` and `menu` controls, so you'll want to account for that when calculating your positions.** 

An example of changing a menu option based on a MIDI note (keyswitch) would look like this:
```xml
<midi>
  <note note="11">
    <binding type="control" level="ui" position="1" parameter="VALUE" translation="fixed_value" translationValue="1" />
  </note>
  <note note="12">
    <binding type="control" level="ui" position="1" parameter="VALUE" translation="fixed_value" translationValue="2" />
  </note>
</midi>
```