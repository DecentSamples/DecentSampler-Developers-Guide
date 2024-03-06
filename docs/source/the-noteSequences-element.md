The &lt;noteSequences&gt; element
=================================

As of 1.11.1, DecentSampler has a built-in note sequencer. It can be mapped to keys so that clusters of notes are played every time certain keys are pressed. It can also be mapped to UI controls such as buttons.

The `<noteSequences>` element is how you specify note sequences that can be used by this playback engine. There should be exactly one `<noteSequences>` element in each `<DecentSampler>` file.

The `<noteSequences>` element can contain one or more `<sequence>` elements:

## The &lt;sequence&gt; element

The `<sequence>` element has the following attributes:

- `name` (optional): An optional descriptive name for the sequence. This is only used in the sample editor UI to help you identify the sequence.
- `length` (required): The length of the sequence in beats. This is a floating point number.
- `rate` (optional): The rate at which the sequence is played. This is a floating point number. The default is 1.0.

The `<sequence>` element can contain one or more `<note>` elements:

### The &lt;note&gt; element

The `<note>` element has the following attributes:
- `position` (required): The position of the note in the sequence, in beats. This is a whole number.
- `velocity` (required): The velocity of the note. This is a floating point number between 0 and 1.
- `note` (required): The MIDI note number of the note.
- `length` (required): The length of the note in beats. This is a whole number.

This is an an example showing the strum from an Omnichord:

```xml
<noteSequences>
    <sequence name="Maj1Slow" length="768.0" rate="96">
        <note position="0" velocity="1" note="48" length="768"/>
        <note position="11" velocity="1" note="52" length="757"/>
        <note position="29" velocity="1" note="55" length="739"/>
        <note position="46" velocity="1" note="60" length="722"/>
        <note position="63" velocity="1" note="64" length="705"/>
        <note position="81" velocity="1" note="67" length="687"/>
        <note position="99" velocity="1" note="72" length="669"/>
        <note position="117" velocity="1" note="76" length="651"/>
        <note position="134" velocity="1" note="79" length="634"/>
        <note position="153" velocity="1" note="84" length="615"/>
        <note position="171" velocity="1" note="88" length="597"/>
        <note position="192" velocity="1" note="91" length="576"/>
    </sequence>
</noteSequences>
```

## Example: How to trigger note sequences using MIDI

```xml
<DecentSampler pluginVersion="1" minVersion="1.11.1">
  <ui>
    <keyboard>
      <!-- We color in the keys from 24 to 35 to let a user know that these are special -->
      <color enabled="true" loNote="24" hiNote="35" color="FFFF9C9C"/>
    </keyboard>
  </ui>
  <midi>
    <!-- We define a listener for the notes 24 through 35... -->
    <note note="24-35" swallowNotes="true" enabled="1">
      <!-- ... and we tell it to play the note sequence Maj1Slow.
           Notice how we tell it to transpose the sequence relative to note 24.
           Also notice that we tell it not to loop, we tell it to play back at 
           twice its normal rate. Perhaps the most important thing is that we 
           specify its trigger behavior as being `midi_key`. This tells the engine
           that it should keep track of this keypress and stop playing the sequence 
           when a key is released. -->
      <binding level="instrument" type="note_sequence" seqIndex="0" seqLoopMode="no_loop" seqTriggerBehavior="midi_key" seqTransposeWithRootNote="24.0" seqTrackMidiInputVelocity="1.0" seqPlaybackRate="2.0"/>
    </note>
  </midi>
  <noteSequences>
    <sequence name="Maj1Slow" length="768.0" rate="96">
      <note position="0" velocity="1" note="48" length="768"/>
      <note position="11" velocity="1" note="52" length="757"/>
      <note position="29" velocity="1" note="55" length="739"/>
      <note position="46" velocity="1" note="60" length="722"/>
      <note position="63" velocity="1" note="64" length="705"/>
      <note position="81" velocity="1" note="67" length="687"/>
      <note position="99" velocity="1" note="72" length="669"/>
      <note position="117" velocity="1" note="76" length="651"/>
      <note position="134" velocity="1" note="79" length="634"/>
      <note position="153" velocity="1" note="84" length="615"/>
      <note position="171" velocity="1" note="88" length="597"/>
      <note position="192" velocity="1" note="91" length="576"/>
    </sequence>
  </noteSequences>
  <groups>
    <group>
      <!-- Samples are defined here... -->
    </group>
  </groups>
</DecentSampler>

```

## Example: How to trigger note sequences using UI controls
```xml
<DecentSampler pluginVersion="1" minVersion="1.11.1">
  <ui>
    <tab>
      <!-- This button has two states -->
      <button x="200" y="17" width="100" height="30" style="text" parameterName="Sequencer Enabled" value="0" defaultValue="0">
        <!-- In the Off state, we silence any sequence that might be playing that has the identifier "sequence_button_1"  -->
        <state name="Off">
          <binding level="instrument" type="note_sequence" seqIndex="0" seqTriggerBehavior="off" seqPlayerIdentifier="sequence_button_1"/>
        </state>
        <!-- In the On state, we tell the engine to play the sequence Maj1Slow. We will specify that it should be 
        tracked internally using the identifier "sequence_button_1". That fact that we are sepcifying this idenfitier 
        now will allow us to turn the sequence off later. -->
        <state name="On">
          <binding level="instrument" type="note_sequence" seqIndex="0" seqLoopMode="forward" seqTriggerBehavior="on" seqTrackMidiInputVelocity="1.0" seqPlaybackRate="1" seqTranspose="12" seqPlayerIdentifier="sequence_button_1"/>
        </state>
      </button>
    </tab>
  </ui>
  <noteSequences>
    <sequence name="Maj1Slow" length="768.0" rate="96">
      <note position="0" velocity="1" note="48" length="768"/>
      <note position="11" velocity="1" note="52" length="757"/>
      <note position="29" velocity="1" note="55" length="739"/>
      <note position="46" velocity="1" note="60" length="722"/>
      <note position="63" velocity="1" note="64" length="705"/>
      <note position="81" velocity="1" note="67" length="687"/>
      <note position="99" velocity="1" note="72" length="669"/>
      <note position="117" velocity="1" note="76" length="651"/>
      <note position="134" velocity="1" note="79" length="634"/>
      <note position="153" velocity="1" note="84" length="615"/>
      <note position="171" velocity="1" note="88" length="597"/>
      <note position="192" velocity="1" note="91" length="576"/>
    </sequence>
  </noteSequences>
  <groups>
    <group>
      <!-- Samples are defined here... -->
    </group>
  </groups>
</DecentSampler>
```