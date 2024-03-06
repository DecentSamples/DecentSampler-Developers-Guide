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

For a full discussion of how to use note sequences, see the tutorial on [how to use note sequences](topic-how-to-use-note-sequences).