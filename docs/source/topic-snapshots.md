How to ship snapshots with your library
========================================================

As of DecentSampler 1.39.0, a library can include **snapshots**: small files that point at one of your presets and hold only the positions of its controls. They let you offer many variations of a sound without making a copy of the whole preset for each one. That matters when a preset is large: a snapshot is a few hundred bytes however big its preset is, and switching between snapshots of the preset that's already loaded is instant, with no samples to reload.

Users can make their own snapshots too, with **File > Save Snapshot...**. This page is about the ones you ship. Older versions of DecentSampler ignore `.dssnapshot` files.

## Making a snapshot

1. Load the preset in DecentSampler and set its controls the way you want them.
2. Choose **File > Save Snapshot...** and give it a name.
3. Open the preset menu at the top of the plug-in and choose **Manage Snapshots > Show in Finder** (or **Show in Explorer**). That opens the folder the snapshot was saved in.
4. Copy the `.dssnapshot` file into your library, usually right next to its preset, and rename the file if you like.

That's all. DecentSampler finds snapshots anywhere inside your library's folder.

You can also write the file by hand, or edit the one DecentSampler saved. It's plain XML:

```xml
<DecentSamplerSnapshot formatVersion="1" name="Dark">
  <preset path="Pads/Warm Pad.dspreset"/>
  <controls>
    <control index="0" type="knob" label="Attack" value="0.62"/>
    <control index="1" type="knob" label="Tone" value="0.2"/>
    <control index="2" type="menu" label="Character" value="3"/>
  </controls>
  <settings globalTuning="0.0"/>
</DecentSamplerSnapshot>
```

## The file format

### The `<DecentSamplerSnapshot>` element

- **`formatVersion`** (required): Always `1` for now.
- **`name`** (optional): The name shown for the snapshot. If it's missing, the file name is used.

### The `<preset>` element

Says which preset the snapshot belongs to.

- **`path`** (required): The preset's path, relative to your library's top-level folder (the one with your `DSLibraryInfo.xml` in it, or your `.dsbundle` folder). If the snapshot sits right next to its preset, just the preset's file name works too.

`.dspreset` and `.dsproduct` are interchangeable here, so a snapshot made against your `Warm Pad.dspreset` while you work still finds `Warm Pad.dsproduct` in the version you ship.

A snapshot saved by DecentSampler also has `libraryPath`, `productId`, `library` and `presetName` attributes. Those help a user's own snapshots find their preset from outside the library; in a snapshot you ship, you can leave them or delete them.

### The `<controls>` element

One `<control>` element for each control in the preset's first `<tab>` that has a value: knobs (`labeled-knob` and `control`), buttons, menus, XY pads and drawbars. Labels, images and other elements without a value aren't counted.

- **`index`**: The control's position among those controls, counting from 0.
- **`type`**: `knob`, `button`, `menu`, `xyPad` or `drawbars`.
- **`label`**: The control's `label` (or `parameterName`, or `name`, whichever it has).
- The value itself, using the control's own attribute names: `value` for knobs, buttons and menus; `xValue` and `yValue` for an XY pad; `level1`, `level2` and so on for drawbars.

A control is matched by its position first. If the control at that position has a different type or label, which is what happens when a later version of your preset adds a control in front of it, DecentSampler looks for the one control with the same type and label instead. So:

- Give your controls labels, and keep them unique, so snapshots survive changes to your UI.
- Controls a snapshot doesn't mention keep their values from the preset.
- A value outside the control's current range is brought back into range.

When a user loads a snapshot that has values for controls the preset no longer has, they're told how many settings couldn't be restored.

### The `<settings>` element

- **`globalTuning`** (optional): Main Tuning, in semitones.

### The `<midi>` element

Snapshots a user saves carry their MIDI Learn mappings here. Snapshots inside a library never change anyone's MIDI mappings, so DecentSampler ignores this element in the ones you ship. Put any MIDI mappings your library needs in the preset itself.

## Where your snapshots appear

- **The preset menu** lists each snapshot right after its preset, and the arrows beside the preset name step through them in that order.
- **The library browser** shows a preset's snapshots underneath it.
- **`<presetMenu>`** in your `DSLibraryInfo.xml` can place snapshots wherever you like, exactly as it places presets: `<preset file="Pads/Warm Pad - Dark.dssnapshot"/>`. Any snapshot it doesn't mention goes right after its preset. See [The DSLibraryInfo.xml file](the-dslibraryinfo-file.md).

Users' own snapshots are kept in DecentSampler's settings folder, never inside your library, so installing an update to your library doesn't touch them. They appear under **Your Snapshots** in the preset menu.

## Copy-protected libraries

Snapshots work with copy-protected (`.dsproduct`) libraries. A snapshot file isn't encrypted, and it doesn't need to be: it holds nothing but control positions, never any of the preset itself.
