How to make loops that follow the tempo
=======================================

Decent Sampler can play a drum loop (or any rhythmic loop) in time with the song, whatever tempo the host is set to. It works like the Beats warp mode in Ableton Live: the loop is cut into slices, and each slice is started on its own beat at the current tempo. The groove follows the tempo, and the pitch stays where it is. This needs Decent Sampler 1.37.0 or later.

The attributes are described in full under [Slice Stretch](the-groups-element) in the &lt;groups&gt; element reference. This page walks through building an instrument.

## Step 1: Map the loop

Put the loop in a group with `sampleMode="slice_stretch"` and tell Decent Sampler how many beats long it is:

```xml
<groups attack="0" decay="0" sustain="1" release="0.1">
  <group sampleMode="slice_stretch" beats="8">
    <sample path="Samples/Drum Loop.wav" rootNote="60" loNote="48" hiNote="72"/>
  </group>
</groups>
```

Hold note 60 and the loop plays from the beginning, at the host's tempo, for as long as the key is held. Notes above and below it play the same rhythm with every slice pitched up or down: an octave up plays each slice twice as fast, but the hits still land on the same beats.

If you know the tempo the loop was recorded at rather than its length, use `originalTempo="100"` instead of `beats`.

The amplitude envelope still applies. An attack of 0 and a sustain of 1 let the loop play exactly as recorded, and the release sets how quickly it stops when the key is let go.

## Step 2: Decide where the slices go

Without any slice information, the loop is cut every 16th note. That works well for many drum loops, but slices that start exactly on each hit sound better, especially at slow tempos. There are three ways to get them, and Decent Sampler uses the first one it finds:

1. A `slices` attribute listing where each slice starts, in frames from the start of the file: `slices="0 6670 13340 20010"`.
2. Cue points (markers) saved in the WAV file. Many audio editors and hardware samplers can write them, so a loop that is already marked up needs nothing else.
3. A grid, set with `sliceGrid` (`1/8`, `1/16` and so on).

The easiest way to make a `slices` attribute is the Preset Editor (**File > Developer Tools > Preset Editor...**). Select the sample and set its **Sample Mode** to **Slice Stretch**: the waveform then shows the slice markers. From there you can:

- choose **Slices > Slice at Transients...** to place a slice on every hit, with a sensitivity control and a live preview;
- choose **Slices > Divide Into** to cut the loop into equal slices;
- drag a marker to move it, double-click to add one, or drag a marker up off the waveform to remove it;
- click a slice to hear it on its own.

## Step 3: Fill the gaps

When the song is slower than the loop, or you play above the root note, each slice finishes before the next one is due, and there is a gap. `sliceLoopMode` decides what happens:

- `off` leaves the gap silent. This can sound good on tight, dry drums.
- `forward` (the default) loops the second half of the slice until the next one starts.
- `back_and_forth` plays that part forwards and backwards in turn, which can be smoother on sustained sounds.

`sliceEnvelope` fades each slice. At 100 (the default) there is no fade. Lower values make every slice die away sooner, which gives a tighter, gated sound and hides the gaps at slow tempos.

## Step 4: Add controls

All of this can be changed from the interface. `SLICE_ENVELOPE` works with knobs and can also be modulated by an LFO or envelope; `SLICE_LOOP_MODE` and `SLICE_GRID` take fixed values from a menu:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<DecentSampler minVersion="1.37.0">
  <ui width="812" height="375">
    <tab name="main">
      <labeled-knob x="20" y="40" width="110" label="Slice Envelope" type="float"
                    minValue="0" maxValue="100" value="100">
        <binding type="general" level="group" position="0" parameter="SLICE_ENVELOPE"/>
      </labeled-knob>
      <menu x="160" y="60" width="160" height="30" value="2">
        <option name="Gaps: Off">
          <binding type="general" level="group" position="0" parameter="SLICE_LOOP_MODE"
                   translation="fixed_value" translationValue="off"/>
        </option>
        <option name="Gaps: Forward">
          <binding type="general" level="group" position="0" parameter="SLICE_LOOP_MODE"
                   translation="fixed_value" translationValue="forward"/>
        </option>
        <option name="Gaps: Back and Forth">
          <binding type="general" level="group" position="0" parameter="SLICE_LOOP_MODE"
                   translation="fixed_value" translationValue="back_and_forth"/>
        </option>
      </menu>
    </tab>
  </ui>
  <groups attack="0" decay="0" sustain="1" release="0.1">
    <group sampleMode="slice_stretch" beats="8" sliceLoopMode="forward">
      <sample path="Samples/Drum Loop.wav" rootNote="60" loNote="48" hiNote="72"/>
    </group>
  </groups>
</DecentSampler>
```

## Things to know

- The loop always starts from its first slice when a key is pressed. It isn't lined up with the host's bar position, so start notes on the beat.
- Slice-stretch samples always play from memory, even if the user has chosen disk streaming.
- `start` and `end` set the part of the file used as the loop, so a file with silence before or after it can be trimmed without editing the audio.
- In the standalone app, Decent Sampler uses its own tempo setting instead of a host's.
