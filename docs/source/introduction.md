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

Example:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<DecentSampler minVersion="1.0.0">
    <!-- More tags go here. :) -->
</DecentSampler>
```

Underneath the top-level `<DecentSampler>` element you can put any number of other elements, which are described in the sections that follow.