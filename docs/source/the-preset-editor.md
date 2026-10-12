The Preset Editor
=================

Decent Sampler has a built-in Preset Editor for building the sample mapping of an instrument without writing XML by hand. You can drag in audio files, map them across the keyboard, set up velocity layers and round robins, set loop points, slice drum loops, and save the result as a **dspreset** file. Everything it does is ordinary XML underneath, and the rest of this guide describes what each setting means.

The Preset Editor was redesigned in Decent Sampler 1.37.0. This chapter describes that version.

<!-- SCREENSHOT: the whole Preset Editor window, dark theme, with a multi-sampled instrument loaded and one zone selected -->

## Opening the editor

Load the preset you want to work on (or nothing, to start a new one), then choose **File > Developer Tools > Preset Editor (beta)...** in Decent Sampler, or press **F12**. Press F12 again, or click the back arrow at the top left, to go back to the player.

The Preset Editor isn't available on iOS, or for copy-protected libraries.

## The window

- **The toolbar** across the top has the back arrow, the **FILE...** and **TOOLS...** menus, and Undo and Redo. The name of the preset is in the middle, with a `*` when there are unsaved changes. **VALIDATE** and **SAVE** are on the right.
- **The groups list** on the left shows every `<group>` in the preset. The **+** button adds a group, and the **…** button has the group actions: Add Group, Rename, Duplicate, Merge Groups, Delete and Select All. You can select several groups at once. A disabled group is shown crossed out.
- **The samples pane** in the middle shows the zones (samples and oscillators) in the selected groups. Its header has the **Edit**, **Add**, **Map**, **Loop** and **Process** menus, and a switch between the **Map** and **List** views. If the window is narrow, these menus are combined into a single Edit menu.
- **The waveform** of the selected sample is shown under the map, behind a divider you can drag.
- **The inspector** on the right shows the settings of whatever is selected. It has three tabs: **Zone**, **Group** and **All groups**. The button at its top collapses it to a thin strip when you need more room for the map; click a tab name on the strip to open it again.

Keyboard shortcuts in this chapter are for the Mac. On Windows and Linux, use Ctrl where it says Cmd.

## Adding samples

Drag WAV or AIFF files from your computer onto the map or list. They are added to the selected group, or to a new group if there isn't one. Several files at once are spread side by side across the keyboard, ready to be mapped properly.

When you drop files onto a completely empty preset, the editor offers to walk you through setting them up. It asks whether the file names already contain note or velocity information, whether the samples should loop or play once, and whether they are velocity layers of the same sound. You can turn this off with "Don't ask me again", or in the preferences.

You can also use **Add > Add Sample** to add an empty sample and choose its file in the inspector, or **Add > Add Oscillator** to add one of the built-in oscillators (see [The &lt;oscillator&gt; element](the-groups-element.md#the-oscillator-element)).

To swap the audio file of a sample, click the **File** field in the inspector, or drop a file onto it.

## Mapping samples to notes

Each zone on the map is a rectangle: left to right is its note range (`loNote` to `hiNote`), and bottom to top is its velocity range (`loVel` to `hiVel`). The keyboard under the map highlights the root notes of the selected zones.

- Click a zone to select it, and Shift-click to add to the selection. Drag across empty space to select several.
- Drag a zone to move it, or drag one of its edges to change its note or velocity range.
- To type exact values, use the **Root note**, **Low note**, **High note**, **Low velocity** and **High velocity** fields in the inspector. Notes can be typed as names (`C3`) or numbers (`60`).

The **Map** menu does most of the work for a whole set of samples at once:

- **Set Root Note** finds each sample's `rootNote` by detecting its pitch (which also sets `tuning` to correct for a slightly sharp or flat recording), from the root note saved in the audio file, or from a note name in its file name. The tuning-fork button next to the Root note field in the inspector also detects the pitch.
- **Map Notes** spreads the selected zones across the keyboard by their root notes, with each root note at the top, middle or bottom of its range. **Close Gaps** stretches the existing ranges so there are no gaps or overlaps, and **Distribute Evenly Across Range...** spreads the selection evenly between two notes.
- **Map From Filename...** reads the note, velocity layer, round robin and group from the parts of each file name, such as `Piano_C3_v2_rr1.wav`. You say what each part of the name means, and it maps everything in one go.

## Inherited values

Most settings can be made on a sample, on its `<group>`, or on the top-level `<groups>` element. A sample uses its own value if it has one, then its group's, then the value set for all groups, and otherwise the default. (Volume is the exception: the sample, group and all-groups volumes multiply together.)

The inspector shows where each value comes from:

- A value set on the zone itself is shown in a solid box.
- A value it inherits is shown in a dashed outline, with a small tag saying where it comes from: **group**, **all groups** or **default**.

Changing a field sets the value on whatever is selected. To go back to the inherited value, right-click the field and choose **Use group value**, **Use all-groups value** or **Use default value**. In the List view, inherited values are shown dimmed.

When several zones are selected and they don't agree on a value, the field shows **Mixed**. Typing a value sets it on all of them.

## The Zone tab

The **Zone** tab edits the selected samples and oscillators:

- **Mapping**: root note, note range and velocity range.
- **Sample**: the audio file, and the **Mode**: Normal, or Slice Stretch for loops that follow the tempo (see [Slicing loops](#slicing-loops-that-follow-the-tempo) below).
- **Pitch**: pitch tracking (`pitchKeyTrack`) and tuning.
- **Playback**: the start and end points, volume, pan and round-robin position (`seqPosition`).
- **Loop**: looping on or off, loop start and end, and the crossfade.

Oscillators have their own settings here, including their envelope. A selection that mixes samples and oscillators shows only the mapping fields.

## Group settings

Select a group in the list on the left and use the **Group** tab to edit settings for the whole group. If you select several groups, the changes apply to all of them. The **All groups** tab sets defaults for every group in the preset, on the `<groups>` element.

The Group tab has:

- **Group**: the group's name, whether it's enabled, and its volume, pan and tuning offsets.
- **Zone defaults**: velocity sensitivity (`ampVelTrack`) and key tracking.
- **Amp envelope**: attack, decay, sustain and release.
- **Round robin**: the mode and length (see below).
- **Trigger & tags**: when the group plays (note on, note off, first note or legato), its tags, and which tags silence it.
- **Glide**: glide time and mode.

The All groups tab has the instrument's volume, pan and tuning, the zone defaults, the amp envelope and glide.

These settings are described in full under [The &lt;group&gt; element](the-groups-element.md#the-group-element).

## Velocity layers

A velocity layer is a set of zones that share notes but cover different velocity ranges, so that playing harder plays a louder recording. On the map, the upper zones play at high velocities and the lower ones at low velocities.

There are three ways to set them up:

- Select all the samples and choose **Map > Set Velocity Layer From Audio Loudness**. For each root note, it measures how loud each sample is and splits the velocity range 1 to 127 between them, quietest at the bottom.
- If the velocity layer is in the file names, use **Map > Map From Filename...** and mark that part of the name as Velocity Layer.
- Drag the top and bottom edges of the zones on the map, or type the **Low velocity** and **High velocity** values in the inspector.

## Round robins

Round robins play a different recording of the same note each time it is hit, which keeps repeated notes from sounding mechanical.

1. Put the alternate recordings in the same group, mapped to the same notes.
2. On the **Group** tab, set the **Round robin** **Mode** to Round robin, Random or True random, and set its **Length** to the number of alternate recordings.
3. Give each sample its **Seq position** on the Zone tab: 1 for the first recording, 2 for the second, and so on.

If the round robin number is in the file names, **Map > Map From Filename...** can set all of this for you. See **Round Robins** under [The &lt;sample&gt; element](the-groups-element.md#the-sample-element) for how the modes behave.

## Loops

Select a sample, set its **Loop** **Mode** to Looping, and set the loop start and end, either in the inspector or by dragging the loop handles on the waveform. The **Loop tuner** switch above the waveform shows the end of the loop next to its start, so you can see whether they join smoothly, with **-1** and **+1** buttons to nudge each point by one sample. The play button plays the loop.

The **Loop** menu has helpers that work on all the selected samples:

- **Auto Loop Detection** finds a loop and a crossfade for you.
- **Read Loop Audio File Metadata** uses the loop points saved in the audio files.
- **Snap Loop Points To Zero Crossing** moves the loop points slightly to avoid clicks.
- **Auto-Suggest Crossfade Length** picks a crossfade length.
- **Set Loop From Percentages...** sets the loop as a percentage of each file's length.

Two items change your audio files, and can't be undone with Undo: **Write Loop Points To Audio Files** saves the loop points into the WAV files themselves, and **Burn Crossfade Into Samples...** bakes the crossfade into the audio. Both ask before they do anything.

## Slicing loops that follow the tempo

A drum loop can be set to play in time with the host's tempo by cutting it into slices. [How to make loops that follow the tempo](topic-how-to-make-loops-that-follow-the-tempo) explains how this works. In the editor:

1. Select the loop and set its **Mode** to **Slice Stretch** in the inspector. The Loop section becomes a **Slices** section, and the waveform shows the slices.
2. Enter the loop's length in **Beats**, or its **Original tempo**.
3. Choose where the slices go. Out of the box, the loop is cut on the **Grid** (every 16th note), or at the cue points saved in the file if it has any. To slice it at each hit instead, open the **Slices** menu and choose **Slice at Transients...**: adjust the sensitivity and minimum gap while watching the markers on the waveform, then click **Apply**. **Divide Into** cuts it into equal slices.
4. Adjust the slices on the waveform. Drag a marker to move it, double-click to add one, and drag a marker up off the waveform to remove it. Click a slice to hear it.
5. Set **Slice loop** and **Slice envelope** to decide what fills the gap when a slice finishes before the next one is due.

## Processing samples

The **Process** menu works on the selected samples:

- **Balance Volumes To -12dB** sets each sample's volume so they all peak at the same level. It doesn't change the audio files.
- **Set Start To First Transient** and **Trim Silence** move each sample's start and end points past silence. They don't change the audio files either.
- **Create Truncated Copies...** and **Apply Fade In/Out...** write new WAV files next to the originals (ending in `_trimmed` or `_faded`) and switch the samples to use them. The originals are left alone.
- **Experimental > Generate In-Between Notes...** and **Experimental > Extend Range Upward...** create new pitch-shifted samples to fill gaps in the mapping or extend it upwards. They write new WAV files into a `Samples` folder.

## Saving, undo and checking your work

**SAVE** (Cmd+S) saves the preset over the file you loaded, and **FILE... > Save As...** saves it under a new name. Sample paths are saved relative to the preset's folder. The FILE... menu also has **New**, **Load...**, **Load Recent** and **Show Preset XML...** (F11), which shows the XML you are building.

Undo and Redo (Cmd+Z and Cmd+Shift+Z) work on everything in the editor except the menu items that change audio files.

**VALIDATE** checks the preset for problems such as missing samples; see [How to Validate a Preset File](topic-validate-preset).

## Other tools

The **TOOLS...** menu has:

- **Collect Samples...** copies any samples stored outside the preset's folder into its `Samples` folder and updates the paths, so the library can be shared. The originals stay where they are.
- **Clean Up Sample Library...** finds audio files in a library folder that no preset uses, and moves the ones you choose to the Trash.
- **Sequence Manager...** imports MIDI files as note sequences (see [How to use note sequences](topic-how-to-use-note-sequences)).
- **Manage Preset Menu...** edits the list of presets in the library's `DSLibraryInfo.xml` (see [The DSLibraryInfo file](the-dslibraryinfo-file)).
- **Templates** applies a saved set of controls and effects to the current preset, or saves the current preset as a template.
- **Preset Editor Preferences...** (see below).
- **Reset Window Size**.

## Preferences and themes

**TOOLS... > Preset Editor Preferences...** has the **Theme** setting: **Dark** (the default), **Light**, or **Match system**, which follows your computer's light or dark mode. It also has the option to show the setup walkthrough when you drop samples onto an empty preset.
