How to add Keyswitches
========================================================

As of DecentSampler 1.4.0, it is now possible to implement keyswitches. For a long time, it’s been possible to trigger events when a MIDI continuous controller event is received: for example, using MIDI CCs we can change knob values or group volumes. Well, it is also possible to trigger events using MIDI notes as well. Here’s what the setup for a MIDI note-based event mapping would look like:

```xml
<midi>
  <note note="11">
    <binding type="general" level="group" position="0" parameter="ENABLED" 
             translation="fixed_value" translationValue="true" />
    <binding type="general" level="group" position="1" parameter="ENABLED" 
             translation="fixed_value" translationValue="false" />
  </note>
  <note note="12">
    <binding type="general" level="group" position="0" parameter="ENABLED" 
             translation="fixed_value" translationValue="false" />
    <binding type="general" level="group" position="1" parameter="ENABLED" 
             translation="fixed_value" translationValue="true" />
  </note>
</midi>
```

In this example, MIDI note 11 turns on group 0 and turns off group 1, whereas MIDI note 12 does the opposite. Note the use of the `fixed_value` translation type.

More documentation on this can be found [here](#the-midi-element).