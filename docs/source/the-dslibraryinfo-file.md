The DSLibraryInfo.xml file
===========================

The `DSLibraryInfo.xml` is an optional sidecar file that describes a whole sample library, as opposed to a single preset. If your library has one, it lives at the top level of your library's folder, a sibling to your `Presets/`, `Samples/`, etc. subfolders — not inside any individual `.dspreset` file.

Everything in this file is optional, including the file itself. A sample library with no `DSLibraryInfo.xml` at all behaves exactly as it always has.

## The &lt;DecentSamplerLibraryInfo&gt; element

This is the root element of `DSLibraryInfo.xml`.

Attributes:

| Attribute | Required/Optional | Description | Default |
|:----------|:-------------------|:-------------|:---------|
| **`name`** | optional | The library's display name, shown wherever DecentSampler lists your installed libraries. | *(the folder's own name)* |
| **`productId`** | optional | Ties this library to a specific product/purchase, if you distribute it through DecentSampler's built-in store integration. Do not add this yourself, as it will break everything. :) | *(none)* |
| **`version`** | optional | The library's own version string, in `major.minor.patch` form (e.g. `1.2.0`). | *(none)* |
| **`coverArt`** | optional | Path to a cover art image for the library, relative to this file's own folder. This gets displayed in the File Browser. | *(none)* |

Example:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<DecentSamplerLibraryInfo name="My Sample Library" productId="com.example.mylibrary"
                           version="1.2.0" coverArt="CoverArt.png">
</DecentSamplerLibraryInfo>
```

## The &lt;presetMenu&gt; element

Larger sample libraries often ship many presets (e.g. a dozen pads, a couple dozen leads, a handful of basses). Without a `<presetMenu>` element, DecentSampler's preset browser simply mirrors your presets' physical folder layout on disk. That's fine for small libraries, but it means the only way to reorganize how your presets are grouped in the browser is to actually move the `.dspreset` files around on disk, and since a preset's own sample paths are relative to its own file's location, it's cumbersome and can easily break presets.

The `<presetMenu>` element solves this by letting you define a browsing hierarchy that's completely independent of where your `.dspreset` files actually live on disk. Your presets stay right where they are; `<presetMenu>` just describes how you want them grouped when someone clicks on the menu at the top of the plug-in.

### The &lt;menu&gt; element

A `<menu>` element represents one folder/category in the browsing hierarchy.

Attributes:

| Attribute | Required/Optional | Description | Default |
|:----------|:-------------------|:-------------|:---------|
| **`name`** | required | The category's display label in the preset browser. | *(none)* |

A `<menu>` can contain any number of `<menu>` and `<preset>` children, nested as deeply as you like.

### The &lt;preset&gt; element

A `<preset>` element places one existing `.dspreset` file into the browsing hierarchy.

Attributes:

| Attribute | Required/Optional | Description | Default |
|:----------|:-------------------|:-------------|:---------|
| **`file`** | required | Path to a `.dspreset` file, relative to the library's top-level folder (the same folder `DSLibraryInfo.xml` itself lives in). | *(none)* |

### How unmapped presets are handled

You don't have to list every preset in your library inside `<presetMenu>`, only the ones you want organized into a specific category. Any `.dspreset` (or `.dsproduct`) file under your library's top-level folder that isn't referenced anywhere in `<presetMenu>` still shows up automatically, at the top level of the browsing hierarchy, right alongside your named categories.

### Fallback and error-tolerance rules

- If a `<preset file="...">` entry points at a file that no longer exists, it's silently skipped. No error is shown to the end user.
- A `<menu>` left with no children (after the rule above) is dropped entirely rather than showing up as an empty category.
- Top-level entries, both your named `<menu>` categories and any unmapped presets that fell back to the top level, are always shown in alphabetical order in the browser. Presets and sub-menus *inside* one of your `<menu>` elements keep the order you wrote them in.
- A library with no `<presetMenu>` element at all, or one that ends up with nothing left in it after the rules above, behaves exactly like a library that never used this feature: a flat, alphabetically-sorted, disk-mirrored preset list.

Example:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<DecentSamplerLibraryInfo name="My Sample Library">
  <presetMenu>
    <menu name="Pads">
      <preset file="Presets/Pads/WarmPad.dspreset"/>
      <preset file="Presets/Pads/GlassPad.dspreset"/>
      <menu name="Analog">
        <preset file="Presets/Pads/Analog/Drift.dspreset"/>
      </menu>
    </menu>
    <menu name="Leads">
      <preset file="Presets/Leads/Saw.dspreset"/>
    </menu>
  </presetMenu>
</DecentSamplerLibraryInfo>
```

In this example, the preset browser shows two top-level categories, **Pads** (with a nested **Analog** sub-category) and **Leads**. Any other `.dspreset` file elsewhere in this library's folder that isn't mentioned above (say, a `Bass/DeepBass.dspreset`) would still appear, automatically, alongside **Pads** and **Leads** at the top level.
