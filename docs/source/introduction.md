File Format Overview
====================

At its core each DecentSampler sample library consists of two things: a folder containing a bunch of assets like audio files and pictures, and a single text file (called a **dspreset** file) which describes how the engine should use all of those files. This reference document is a guide to creating **dspreset** files.

**dspreset** files are just XML files. As such, each one begins with an XML declaration: 

```xml
<?xml version="1.0" encoding="UTF-8"?>
```

The top-level &lt;DecentSampler&gt; element (required)
------------------------------------------------------

At the top level of every **dspreset** file is a `<DecentSampler>` element. Every file **must** have one. Here is a list of attributes:

- **`minVersion`** (optional): This is the minimum version on which this preset is known to run. If a user is running an old version of DS, and a developer has specified a minVersion for their instrument, a dialog box will show up telling users that their version is outdated and that they should upgrade in order to get the full effect. They _can_ than choose to ignore this warning or hit download. The dialog box does not show up for iOS users as most of them have auto-updates turned on.

- **`limiterStyle`** (optional): Chooses how the output stage behaves. Valid values are `default` and `brickwall`. Default: `default`.

  `default` is the behaviour every instrument built before this option was added was made against. As well as catching peaks, it compresses anything above -10 dBFS at 4:1 and adds makeup gain to compensate. That is what keeps loud instruments consistent, but it also means busy passages come out flatter than the raw samples: with a library normalised close to full scale, a three note chord can measure slightly *quieter* than a single note.

  `brickwall` keeps only the ceiling, with no compression and no makeup. Chords and dynamics come through at full size. In exchange everything is 3.75 dB quieter than it would be under `default`, so you should expect to set your instrument's levels a little higher, and material normalised right up to full scale will touch the ceiling more often. Choose it when the dynamics of your instrument matter more than matching the loudness of everything else.

### How loud your samples come out

It is worth knowing that a sample does not come out of DecentSampler at the level it went in at, so if your instrument sounds quieter than you expected, this is why rather than anything being wrong with your samples.

A voice that is not panned anywhere passes through a constant-power pan law, which costs it 3 dB in the centre position, and then through the output stage, which adds some level back under the default `limiterStyle` and none at all under `brickwall`. The result, measured on a centred sample with no other processing:

| `limiterStyle` | A centred sample comes out |
| -------------- | -------------------------- |
| `default`      | 5.3 dB below its own level |
| `brickwall`    | 9.0 dB below its own level |

You do not need to do anything about this. Most people simply set their instrument's `volume` until it sounds right next to whatever else they are using, which is exactly the right approach. But if you want your samples to come out at the level they were recorded at, `volume="1.837"` gets you there under the default style and `volume="2.828"` under `brickwall`.

Two things worth keeping in mind. Those numbers are the gain of the whole voice, so a group volume and a sample volume multiply on top of them. And if you are pushing levels up, remember that `default` also compresses anything above -10 dBFS, so a louder instrument gives that compressor more to do.

Example:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<DecentSampler minVersion="1.0.0">
    <!-- More tags go here. :) -->
</DecentSampler>
```

Underneath the top-level `<DecentSampler>` element you can put any number of other elements, all of which are described in the sections that follow.