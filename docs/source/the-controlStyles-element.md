Control Styles
==============

Control styles let you design your own knobs, sliders and buttons without making images. A style is a short list of layers, such as circles, arcs, pointers, tick marks and text, with effects like shadows and bevels, written in the preset's `<ui>` element. The player draws controls from it as they're shown, so they stay sharp at every window size, and the same style can be used by as many controls as you like.

You can design styles visually on the **Style** page of the [UI Designer](the-ui-designer.md), or write the XML by hand, as described below. Both make the same XML, so you can switch between them whenever you like.

Control styles were added in Decent Sampler 1.43.0. Older versions skip them and show their usual knobs, sliders and buttons; see [Supporting older versions](#supporting-older-versions) for how to give them an image strip instead.

```{image} _static/images/control-styles-style-page.png
:alt: "The UI Designer with the Style page open for the vintage knob style: a preview of the knob at five values and three sizes, the style's layers (a pointer with an inner shadow, two circles, the lower one with a shadow and a bevel, and ticks), and the selected inner shadow's settings"
```

## Designing a style in the UI Designer

Open the [UI Designer](the-ui-designer.md) and click **Style** at the top of the inspector.

1. Select the knobs you want to style, then choose **STYLE > New knob style from** and pick a starter: **Flat**, **Vintage**, **Chicken Head**, **LED Ring**, **Synth Cap**, **Brushed Metal**, **Gray Bar** or **Blank**. **New slider style from** and **New button style from** do the same for sliders and buttons. The new style is applied to the selected controls straight away.
2. The preview shows the style at 0%, 25%, 50%, 75% and 100%, and at three sizes, on the interface's own background. Slider and button previews show their states, hover and pressed looks.
3. The list under the preview shows the style's layers, topmost first, with each layer's effects under it. Click one to change its settings below. **+ LAYER** adds a layer above the selected one, **+ EFFECT** adds an effect to it, and **UP**, **DOWN** and **DELETE** rearrange or remove the selected layer or effect.
4. Number fields can be changed by dragging their label left or right, or with the Up and Down arrow keys. Hold Shift for small steps or Cmd for big ones.

To use a style on more controls, select them, choose the style in the menu at the top of the page, and click **APPLY TO SELECTION**. The line under the preview says how many controls use it. You can also set a control's **Control style** on its **Control** page, or remove it there.

**STYLE > Duplicate** makes a copy to try changes on, and **STYLE > Delete** deletes the style and removes it from the controls that use it. Every change is one Undo step.

## Writing styles by hand

Styles live in a `<controlStyles>` element inside `<ui>`, next to the `<tab>`. Each style has an `id`, and a control uses it by naming that `id` in its `controlStyle` attribute:

```xml
<ui width="812" height="375" bgColor="FF1E2228">
  <controlStyles>
    <knobStyle id="flat">
      <arc r="42" width="7" color="FF3A3F48"/>
      <arc r="42" width="7" color="FF4FA3FF" to="value"/>
      <circle r="30" fill="FF2C3139"/>
      <pointer shape="rounded" r1="10" r2="26" width="5" color="FFF0F0F0"/>
    </knobStyle>
  </controlStyles>
  <tab name="main">
    <labeled-knob x="30" y="50" width="110" height="130" label="Tone" controlStyle="flat"
                  minValue="0" maxValue="1" value="0.5" textColor="FFF0F0F0">
      <binding type="effect" level="instrument" position="0" parameter="FX_FILTER_FREQUENCY"/>
    </labeled-knob>
  </tab>
</ui>
```

Layers are drawn in the order they're written, so the first is at the bottom. Colors use the usual eight-digit format (see [The color format](appendix-a-the-color-format.md)).

Each `id` must be different from every other style's in the preset, whatever kind it is.

| Element | Used by |
|:--|:--|
| `<knobStyle>` | A `<labeled-knob>` or `<control>` whose `style` is a round one (`rotary...` or `custom_skin...`) |
| `<sliderStyle>` | A `<labeled-knob>` or `<control>` whose `style` is a linear one (`linear...`) |
| `<buttonStyle>` | A `<button>` |

A style that doesn't fit the control it's applied to is ignored, and the control keeps its usual look.

## Knob styles

A knob style is drawn in a square, measured in percent: the center is at 50, 50, and a radius of 50 reaches the edge. So a style looks the same at any size. Angles are in degrees, clockwise from 12 o'clock.

`<knobStyle>` attributes:

| Attribute | Description | Default |
|:--|:--|:--|
| **`id`** | The name controls use for this style in their `controlStyle` attribute. Required. | |
| **`startAngle`** | Where the knob's sweep starts, at its minimum value. | `-135` |
| **`endAngle`** | Where the sweep ends, at its maximum value. | `135` |

`bakedImage`, `bakedFrames` and `bakedHash` are written when you bake the style (see [Supporting older versions](#supporting-older-versions)). Don't edit them.

### Attributes every layer has

| Attribute | Description | Default |
|:--|:--|:--|
| **`opacity`** | From 0 (invisible) to 1. | `1` |
| **`rotate`** | `value` turns the whole layer with the knob's value, around `cx`, `cy`. Use it for a knob cap with shading or an image that should turn. | |
| **`show`** | `hover` shows the layer only while the mouse is over the knob, and `notHover` only while it isn't. | `always` |
| **`cx`**, **`cy`** | The layer's center. | `50`, `50` |

### `<circle>`

A filled disc, with an optional gradient and outline.

| Attribute | Description | Default |
|:--|:--|:--|
| **`r`** | Radius. | `40` |
| **`fill`** | The color. | |
| **`fill2`** | The gradient's second color. | |
| **`gradient`** | `none`, `linear`, `radial`, or `conic` for brushed metal. | `none` |
| **`gradientAngle`** | The direction a linear gradient runs from `fill` to `fill2`: 180 is top to bottom. For `conic`, where the first bright streak is. | `180` |
| **`sheens`** | For `conic`: how many bright streaks run round the circle. | `4` |
| **`grain`** | For `conic`: fine brushed rings, from 0 (none) to 1. | `0` |
| **`outlineColor`**, **`outlineWidth`** | An outline round the edge. | none, `0` |

### `<arc>`

Part of a ring: the whole sweep for a track, or the part up to the value.

| Attribute | Description | Default |
|:--|:--|:--|
| **`r`** | Radius, to the middle of the line. | `45` |
| **`width`** | Line width. | `4` |
| **`color`** | The color. | |
| **`from`** | `start` begins at the start of the sweep. `center` begins at its middle, for bipolar controls such as pan. | `start` |
| **`to`** | `end` draws to the end of the sweep, for a track. `value` draws to the current value. | `end` |
| **`cap`** | `round` or `butt` (square) line ends. | `round` |

### `<pointer>`

The indicator that turns with the value, from radius `r1` out to `r2`. Negative radii reach past the center, which is how the Chicken Head starter makes its tail.

| Attribute | Description | Default |
|:--|:--|:--|
| **`shape`** | `rounded` or `square`: a bar with rounded or square ends. `dot`: a dot halfway between `r1` and `r2`. `triangle`: pointing outward, with its base at `r1`. | `rounded` |
| **`r1`**, **`r2`** | Where it starts and ends. | `10`, `40` |
| **`width`** | How wide it is. | `4` |
| **`color`** | The color. | |

### `<ticks>`

Scale markings spread evenly over the sweep.

| Attribute | Description | Default |
|:--|:--|:--|
| **`count`** | How many. | `11` |
| **`r1`**, **`r2`** | Where each tick starts and ends. | `44`, `48` |
| **`width`** | How thick each tick is. | `1` |
| **`color`** | The color. | |
| **`lit`** | `true` draws the ticks up to the current value in `litColor`, like an LED ring. | `false` |
| **`litColor`** | The color of lit ticks. | |

### `<image>`

An image from the library, such as a photographed knob cap, usually with `rotate="value"`. JPEG and PNG images work.

| Attribute | Description | Default |
|:--|:--|:--|
| **`path`** | The image, relative to the preset (or to the shared styles file, for a style in one). Required. | |
| **`x`**, **`y`**, **`w`**, **`h`** | Where to draw it in the square. | `0`, `0`, `100`, `100` |

### `<text>`

Fixed text, or the knob's current value, centered on `x`, `y`.

| Attribute | Description | Default |
|:--|:--|:--|
| **`text`** | The text to show. | |
| **`showValue`** | `true` shows the knob's current value instead. | `false` |
| **`x`**, **`y`** | The text's center. | `50`, `50` |
| **`width`** | How wide the text can be. | `100` |
| **`size`** | Text height. | `12` |
| **`color`** | The color. | |

### Effects

Any layer can contain effects, which are drawn with it:

| Element | Description | Attributes (defaults) |
|:--|:--|:--|
| `<shadow>` | A drop shadow under the layer. | `dx` (0), `dy` (2), `blur` (3), `color` (`99000000`) |
| `<glow>` | A soft glow round the layer, for an LED arc for example. | `radius` (4), `color` (`CCFFFFFF`) |
| `<bevel>` | Lightens the edge facing the light and darkens the opposite one, for a raised look. | `depth` (2), `lightAngle` (-45) |
| `<specular>` | A soft highlight on the side the light comes from. | `lightAngle` (-45), `size` (40), `strength` (0.6), `color` (`FFFFFFFF`) |
| `<innerShadow>` | A shadow inside the layer, cast from its edge, as if it were sunk in. | `dx` (0), `dy` (2), `blur` (3), `color` (`99000000`) |

The light stays in the same place as the knob turns: the shadow of a layer with `rotate="value"` falls the same way at every angle.

### Example

This is the Vintage starter: tick marks, a shaded cap with a shadow and a bevel, an inner disc, and a cream pointer.

```xml
<knobStyle id="vintage">
  <ticks count="11" r1="44" r2="49" width="1.5" color="FF8A8A8A"/>
  <circle r="36" fill="FF4A4A4A" fill2="FF141414" gradient="linear" gradientAngle="180"
          outlineColor="FF000000" outlineWidth="1">
    <shadow dy="3" blur="5" color="AA000000"/>
    <bevel depth="2.5" lightAngle="-45"/>
  </circle>
  <circle r="27" fill="FF2A2A2A" fill2="FF3C3C3C" gradient="linear" gradientAngle="180" opacity="0.8"/>
  <pointer shape="rounded" r1="5" r2="32" width="3.5" color="FFF2E6C8">
    <innerShadow dx="1" dy="1" blur="2" color="57000000"/>
  </pointer>
</knobStyle>
```

## Slider styles

A slider style is designed lying down, with the minimum on the left. A vertical slider shows it turned, with the minimum at the bottom. All sizes are in percent of the slider's thickness (its height when horizontal), including a thumb's length.

| Element | Description | Attributes (defaults) |
|:--|:--|:--|
| `<track>` | The groove, the full length of the travel. | `thickness` (30), plus the fill attributes below |
| `<fill>` | The part from where the fill starts (the control's `fillFrom`) to the value. | `thickness` (30), plus the fill attributes below |
| `<ticks>` | Marks along the travel. With `lit="true"`, those between the start of the fill and the value use `litColor`. | `count` (11), `thickness` (60), `width` (4), `color`, `lit` (false), `litColor` |
| `<thumb>` | The handle, at the value. | `length` (20), `thickness` (80), plus the fill attributes below |

The fill attributes are `fill` (or `color` on a track or fill), `fill2`, `gradient`, `gradientAngle`, `sheens`, `grain`, `outlineColor` and `outlineWidth`, as for a knob's `<circle>`, and `radius`, the corner radius in percent of the shape's shorter side (50 is fully round).

Every layer also has `opacity`, and `show` (`always`, `hover`, `notHover` or `pressed`), and can contain the same effects as a knob layer.

This is the Fader starter:

```xml
<sliderStyle id="fader">
  <track thickness="22" radius="50" fill="FF2A2F36">
    <innerShadow dy="1" blur="3" color="CC000000"/>
  </track>
  <fill thickness="22" radius="50" fill="FF3D8BFF"/>
  <thumb length="34" thickness="80" radius="25" fill="FFE8EAED" fill2="FFA8ADB5" gradient="linear">
    <shadow dy="2" blur="4" color="AA000000"/>
  </thumb>
</sliderStyle>
```

## Button styles

A button style is drawn in the button's own rectangle: `x`, `y`, `w` and `h` are in percent of the button's width and height, a circle's radius and the corner radii are in percent of its shorter side, and text `size` is in percent of its height.

| Element | Description | Attributes (defaults) |
|:--|:--|:--|
| `<rect>` | A rectangle, with rounded corners if you give it a `radius`. | `x` (0), `y` (0), `w` (100), `h` (100), plus the fill attributes |
| `<circle>` | A disc, for an LED for example. | `cx` (50), `cy` (50), `r` (40), plus the fill attributes |
| `<text>` | Fixed text, or with `showState="true"`, the name of the button's current state. | `text`, `showState` (false), `x` (50), `y` (50), `w` (100), `size` (40), `color` |
| `<image>` | An image from the library. | `path` (required), `x` (0), `y` (0), `w` (100), `h` (100) |

Each layer can have a `state` attribute listing the button states it's shown in, numbered from 0 and separated by commas, so that an LED lights only when the button is on. This is the LED Switch starter:

```xml
<buttonStyle id="led-switch">
  <rect radius="20" fill="FF2C3139"/>
  <circle cx="15" cy="50" r="18" fill="FF3A1010"/>
  <circle cx="15" cy="50" r="18" fill="FFFF4040" state="1">
    <glow radius="12" color="CCFF4040"/>
  </circle>
  <text showState="true" x="58" size="40" color="FFD0D0D0"/>
</buttonStyle>
```

Button layers also have `opacity`, `show` (`always`, `hover`, `notHover` or `pressed`) and effects, like slider layers.

## Sharing styles between presets

A library's presets can share one file of styles, so a change to a style shows up in all of them.

```xml
<controlStyles path="Styles/styles.xml">
  <!-- Styles here are used instead of shared ones with the same id -->
</controlStyles>
```

`path` is relative to the preset. The shared file's top element is a `<controlStyles>` of its own, holding the styles, and paths inside it (for image layers) are relative to the shared file. A style written in the preset is used instead of a shared one with the same `id`.

In the UI Designer, the **STYLE** menu does this for you:

- **Move to the shared styles file** moves the style into the preset's shared file, or into a new `Styles/styles.xml` beside the preset if it doesn't have one yet. The preset has to have been saved first.
- **Use a shared styles file...** points another preset at that file.
- **Copy into this preset** gives the preset its own copy of a shared style, to change it for that preset only.
- **Delete from the shared file** deletes a shared style. Other presets that use it will show their usual look.

Changes to a shared style are saved into the shared file when you save the preset.

Two more commands bring styles in from elsewhere:

- **Import styles from a preset...** copies the styles from another preset or styles file into this one, along with any images they use.
- **Save to My Styles** keeps a copy of a style on your computer, so you can start new styles from it in any preset. Your saved styles appear at the end of the **New ... style from** menus, and **Remove from My Styles** removes them. My Styles is yours only; it isn't part of any library.

## Supporting older versions

Versions of Decent Sampler before 1.43.0 don't know about control styles, so they show a styled knob as an ordinary knob. To have them show your design instead, bake the style into an image strip: click **BAKE STRIP** on the Style page, or choose **STYLE > Bake image strip**.

Baking draws the style at 101 positions into a PNG strip in the preset's `Images` folder, at twice the size of the largest knob that uses it, with a second strip if the style changes on hover. It then sets each of those knobs' `customSkinImage` and related attributes to the strip, and changes their `style` to the matching `custom_skin...` one. Older versions show the strip; 1.43.0 and later ignore it and keep drawing the style.

If you change the style after baking it, the Style page says **Strip for older versions is out of date** and the button becomes **BAKE AGAIN**. **STYLE > Remove baked strip** puts the knobs back as they were. One small difference: knob labels sit about 5 pixels higher in older versions.

Only knob styles can be baked. Older versions show sliders and buttons with styles as ordinary sliders and buttons.

## Checking styles

**VALIDATE** in the Preset Editor warns when:

- a control names a style that isn't defined, or two styles have the same `id`,
- a style contains a layer or effect the player doesn't know,
- a style is used by a control it doesn't fit, such as a knob style on a linear slider,
- a shared styles file can't be found or read,
- an image a style uses is missing,
- a knob style's baked strip is missing, or the style has changed since it was baked, so older versions show the old look.
