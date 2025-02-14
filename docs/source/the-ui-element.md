The &lt;ui&gt; element
======================

The `<ui>` element is how you specify a user interface for your instrument. Each **dspreset** should have at most one `<ui>` element. There are several important attributes:

- **`coverArt`** (optional): A relative or absolutely path to a cover art image to use. After the first time this library is opened, this will get displayed on the "My Libraries" tab.
- **`bgImage`** (optional): A relative or absolutely path to a background image to use. 
- **`bgColor`** (required): An eight digit hex value indicating the background color to be used for the background of the UI. This color will be drawn underneath any background image specified by `bgimage`.
- **`width`** (required): The width of your user interface. Recommended value: 812.
- **`height`** (required): The height of your user interface. Recommended value: 375.

Example:

```xml
<DecentSampler>
  <ui width="812" height="375">
    <tab name="main">
      <labeled-knob x="560" y="0" label="Tone" type="float" minValue="60" maxValue="22000"
                    textColor="FF000000" value="22000.0" uid="y8AA4uuURh3">
        <binding type="effect" level="instrument" position="0" parameter="FX_FILTER_FREQUENCY"/>
      </labeled-knob>
      <label x="360" y="0" width="50" height="30" text="Reverb"/>
      <control x="360" y="30" parameterName="Reverb" type="float" minValue="0" maxValue="1" textColor="FF000000" value="0.5">
      <!-- Your <binding /> elements should go here -->
      </control>
    </tab>
  </ui>
</DecentSampler>
```

## The &lt;tab&gt; element

The `<tab>` element lives underneath the `<ui>` element. This architecture was chosen because we may, at some point, add support for multiple tabs. At present it is only possible to have a single tab within DecentSampler instruments. As such, every UI must have at most one `<tab>` element. 

Attributes:

- **`name`** (optional): An optional name to be associated with this tab. This is currently not displayed anywhere.

## The &lt;button&gt; element
The `<button>` element allows you to create a button within your UI. It lives underneath the `<tab>` element. There are two styles of buttons: text buttons and image buttons.

Attributes:

| Attribute        | Required                       | Description                                                                                                                                                                                             | Default |
|:-----------------|:-------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------|
| **`x`**          | (required)                     | The x position of the menu                                                                                                                                                                              | None    |
| **`y`**          | (required)                     | The y position of the menu                                                                                                                                                                              | None    |
| **`width`**      | (required)                     | The width of the menu                                                                                                                                                                                   | None    |
| **`height`**     | (required)                     | The height of the menu                                                                                                                                                                                  | None    |
| **`value`**      | (optional)                     | The is the 0-based index of the button state that is currently selected. A value of 0 means that the first state is active.                                                                             | 0       |
| **`style`**      | (optional)                     | The type of button we want. There are two valid values: `text`, `image`.                                                                                                                                | `text`  |
| **`mainImage`**  | (required for `image` buttons) | For `image` buttons only. The path of the main image to display for this button. This can also be set at the state level so that it only applies to a specific state.                                   | None    |
| **`hoverImage`** | (optional)                     | For `image` buttons only. The path of the main image to display when the user hovers their mouse over this button. This can also be set at the state level so that it only applies to a specific state. | None    |
| **`clickImage`** | (optional)                     | For `image` buttons only. The path of the main image to display when the user clicks down on this button. This can also be set at the state level so that it only applies to a specific state.          | None    |
| **`visible`**    | (optional)                     | This controls whether or not this button is visible. There are two valid values: `true`, `false`.                                                                                                       | `true`  |
| **`tooltip`**    | (optional)                     | A tool tip to display when the user hovers over this control.  |   |

Example:
```xml
<button x="10" y="40"  width="120" height="30" style="image" value="0" mainImage="samples/ButtonMainImage.png" hoverImage="samples/ButtonHoverImage.png" clickImage="samples/ButtonSelectedImage.png">
    <!-- Your button states go here -->
</button>
```
### The &lt;state&gt; element

In order for your button to work, it must contain at least one `<state>` elements. 

Attributes:

| Attribute        | Required                       | Description                                                                                                                                           |
|:-----------------|:-------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------|
| **`name`**       | (required for `text` buttons)  | The text to display on a text button when this state is active                                                                                        |
| **`mainImage`**  | (required for `image` buttons) | For `image` buttons only. The path of the main image to display for this button when the button is in the current state.                              |
| **`hoverImage`** | (optional)                     | For `image` buttons only. The path of the image to display when the user hovers their mouse over this button when the button is in the current state. |
| **`clickImage`** | (optional)                     | For `image` buttons only. The path of the image to display when the user clicks down on this button when the button is in the current state.          |

In order to have your `<button>` elements actually do something useful, you need to attach bindings to them. Here's an example:

```xml
<button x="10" y="30"  width="70" height="50" style="image" value="0" >
    <state name="English" mainImage="samples/EFlag_MainImage.png" hoverImage="samples/EFlag_HoverImage.png" clickImage="samples/EFlag_SelectedImage.png">
        <!-- Turn on this group -->
        <binding type="general" level="group" position="0" parameter="ENABLED" translation="fixed_value" translationValue="true" />
        <!-- Turn off this group -->
        <binding type="general" level="group" position="1" parameter="ENABLED" translation="fixed_value" translationValue="false" />
    </state>
    <state name="French" mainImage="Samples/FFlag_MainImage.png" hoverImage="Samples/FFlag_HoverImage.png" clickImage="Samples/FFlag_SelectedImage.png">
        <!-- Turn off this group -->
        <binding type="general" level="group" position="0" parameter="ENABLED" translation="fixed_value" translationValue="false" />
        <!-- Turn on this group -->
        <binding type="general" level="group" position="1" parameter="ENABLED" translation="fixed_value" translationValue="true" />
    </state>
</button>
```

As you can see, this example uses a button to switch between two groups. You'll note the liberal use of the `fixed_value` translation mode above. This means that when any of these options are selected, a fixed predetermined value is used for the value of that binding. 

## The &lt;image&gt; element
The `<image>` element allows you to place a static image into your user interface. It lives underneath the `<tab>` element. Attributes:

- **`x`** (required): The `x` position of your image where (0,0) is the top-left corner
- **`y`** (required): The `y` position of your image where (0,0) is the top-left corner
- **`width`** (required): The width in pixels of the image component
- **`height`** (required): The height in pixels of the image component
- **`path`** (required): The relative path of the image file to show in this component
- **`aspectRatioMode`** (required): Whether or not the engine should preserve the aspect ratio of the image. Note: regardless of these settings, you still need to specify a width and height for your image element. Valid values: `preserve`, `stretch`. Default value is `preserve`. 
- **`visible`** (optional): This controls whether or not this image is visible. There are two valid values: `true` (default), `false`.
- **`tooltip`** (optional): A tool tip to display when the user hovers over this image.

## The %lt;multiFrameImage&gt; element

The `<multiFrameImage>` element allows you to play a sequence of images as an animation. The expectation is that all the frames of the animation will be loaded in a single image, arranged in a strip – either horizontal or vertical. This is the same format as is used by the custom knobs above. It lives underneath the `<tab>` element. Attributes:

- **`x`** (required): The `x` position of your image where (0,0) is the top-left corner
- **`y`** (required): The `y` position of your image where (0,0) is the top-left corner
- **`width`** (required): The width in pixels of the image component
- **`height`** (required): The height in pixels of the image component
- **`path`** (required): The relative path of the image file to show in this component
- **`numFrames`** (required): The number of frames in the animation
- **`frameRate`** (required): The frame rate of the animation in frames per second. The maximum supported frame rate is 24 frames per second.
- **`sourceFormat`** (required): The orientation of the frames within the image strip. Valid values: `horizontal_image_strip`, `vertical_image_strip`.
- **`playbackMode`** (optional): The direction in which the animation should play. Valid values: `forward_loop`, `forward_once`, `reverse_loop`, `reverse_once`, `ping_pong_loop` (forth and back), and `stopped`. Default value is `forward_loop`.
- **`visible`** (optional): This controls whether or not this image is visible. There are two valid values: `true` (default), `false`.
- **`tooltip`** (optional): A tool tip to display when the user hovers over this image.

Example:

```xml
<multiFrameImage x="10" y="10" width="64" height="64" path="Images/Animation.png" numFrames="31" imageStripOrientaton="vertical" frameRate="24" playbackMode="forward"/>
```

### The &lt;label&gt; element
The `<label>` element allows you to place a static block of text into yoru user interface. It lives underneath the `<tab>` element. Attributes:

- **`x`** (required): The `x` position of your control where (0,0) is the top-left corner
- **`y`** (required): The `y` position of your control where (0,0) is the top-left corner
- **`text`** (required): The actual text that should be displayed as part of the label.
- **`textColor`** (optional): An 8 digit hex value indicating the text color to be used for the label. See [Appendix A](#appendix-a-the-color-format) for an explanation on these hex values.
- **`textSize`** (optional): A font size for the text label. Default: 12
- **`width`** (required): The width in pixels of the label.
- **`height`** (required): The height in pixels of the label.
- **`vAlign`** (optional): The vertical alignment of the text within the box described by the width and height attributes. Valid values: `top`,`bottom`, `center`. Default is `center`.
- **`hAlign`** (optional): The horizontal alignment of the text within the box described by the width and height attributes. Valid values: `left`,`right`, `center`. Default is `center`.
- **`visible`** (optional): This controls whether or not this text label is visible. There are two valid values: `true` (default), `false`.
- **`tooltip`** (optional): A tool tip to display when the user hovers over this label.

A label's text can also be set dynamically using bindings using the `TEXT` binding parameter name.

### The &lt;labeled-knob&gt; and &lt;control&gt; elements

The `<labeled-knob>` and `<control>` elements live underneath the `<tab>` element. These tags correspond to user controls (usually round radial dials) that can be used as part of a UI. These two element types are the same except that `<labeled-knob>` elements contain built-in labels, where as `<control>` elements do not.  Every tab can have many `<control>` or `<labeled-knob>` elements underneath it. 

For precise UI creation, it may be advisable to use a combination of `<control>` & `<label>` elements rather than `<labeled-knob>`.

Attributes:

- **`x`** (required): The `x` position of your control where (0,0) is the top-left corner
- **`y`** (required): The `y` position of your control where (0,0) is the top-left corner
- **`width`** (required): The width in pixels of the knob + label.
- **`height`** (required): The height in pixels of the knob + label.
- **`parameterName`** (required): In a situation where the sampler does not have enough room to display the full UI, a shrunken down version of the UI will be used. In such situations, this control will be labeled using the `parameterName`. It is good practice to always include a `parameterName`. If not `parameterName` is specified and a value `label` is specified, then that will be used instead. 
- **`style`** (optional): The specific kind of control that is created. The following values are supported: `linear_bar`, `linear_bar_vertical`, `linear_horizontal`, `linear_vertical`, `rotary`, `rotary_horizontal_drag`, `rotary_horizontal_vertical_drag`, `rotary_vertical_drag`, `custom_skin_vertical_drag`, `custom_skin_horizontal_drag`. Default: `rotary_vertical_drag`.
- **`showLabel`** (optional): A true/false value dictating whether or not a built-in label should be displayed. Default: true for `<labeled-knob>` and false for `<control>` elements
- **`label`** (optional): If `showLabel` is true, the actual text that should be displayed above the control.
- **`parameterName`** (required): In a situation where the sampler does not have enough room to display the full UI, a shrunken down version of the UI will be used. In such situations, this control will be labeled using the `parameterName`. It is good practice to always include a `parameterName`.
- **`minValue`** (optional): The minimum value of your control. Default: 0
- **`maxValue`** (optional): The maximum value of your control. Default: 1
- **`value`** (optional): The initial value of your control. Default: 0
- **`defaultValue`** (optional): If a user double-clicks on the control, the control's value will be set to this default value. If no default value is specified, then nothing will happen on double-click.
- **`valueType`** (optional): There are three possible values for this `float` which yields numbers with two decimal points, `integer` which yields whole numbers, and `musical_time` which yields musical time increments in beats and measures. This last option is for use with the built-in delay effect only. Default: float
- **`textColor`** (optional): An 8 digit hex value indicating the text color to be used for the label. See [Appendix A](#appendix-a-the-color-format) for an explanation on these hex values.
- **`textSize`** (optional): A font size for the text label. Default: 12
- **`trackForegroundColor`** (optional): An 8 digit hex value indicating the foreground color to use for the knob track. See [Appendix A](#appendix-a-the-color-format) for an explanation on these hex values.
- **`trackBackgroundColor`** (optional): An 8 digit hex value indicating the background color to use for the knob track. See [Appendix A](#appendix-a-the-color-format) for an explanation on these hex values.
- **`visible`** (optional): This controls whether or not this control is visible. There are two valid values: `true` (default), `false`.
- **`tooltip`** (optional): A tool tip to display when the user hovers over this control.
- **`snapMode`** (optional): This attribute controls how the control should snap to values. Valid values: `none`, `whole_numbers`, `tenths`, `hundredths`, `thousandths`, and `stop_points`. Default: none.
- **`snapStopPoints`** (optional): A comma-separated list of values that the control should snap to when `snapMode` is set to `stop_points`. Default: none.
- **`defeatSnapWithShift`** (optional): A true/false value indicating whether or not the user can defeat the snap-to-value behavior by holding down the shift key. Default: false.

It is also possible to use custom control graphics using the following attributes:

- **`customSkinImage`** (optional): This is path to an image to use for the control. This is expected to be a JPEG or PNG in KnobMan format. A huge gallery of compatible knobs can be found [here](https://www.g200kg.com/en/webknobman/gallery.php).
- **`customSkinNumFrames`** (optional): The number of animation frames contained in the KnobMan image pointed to by `customSkinImage`.
- **`customSkinImageOrientation`** (optional): The orientation of the frames within the KnobMan image pointed to by `customSkinImage`. Valid values: `horizontal`, `vertical`. Default: vertical.
- **`mouseDragSensitivity`** (optional): An integer number describing how sensitive the control should be to mouse drags. The higher the number, the less sensitive the control will be to mouse movements.

If you are using custom knobs, it's important that you specify a `style=` value of either `custom_skin_vertical_drag` or `custom_skin_horizontal_drag`. 

Example: 

```xml
<DecentSampler>
  <ui>
    <tab>
      <labeled-knob x="560" y="0" label="Tone" type="float" minValue="60" maxValue="22000" textColor="FF000000" value="22000.0">
      <!-- Your <binding /> elements should go here -->
      </labeled-knob>
      <label x="360" y="0" width="50" height="30" text="Reverb"/>
      <control x="360" y="30" parameterName="Reverb" type="float" minValue="0" maxValue="1" textColor="FF000000" value="0.5" style="custom_skin_vertical_drag" customSkinImage="Samples/ENIGMA-nolight.png" customSkinNumFrames="31" customSkinImageOrientation="horizontal" mouseDragSensitivity="100">
      <!-- Your <binding /> elements should go here -->
      </control>
    </tab>
  </ui>
</DecentSampler>
```

To learn how to make knobs actually control parameters of your instrument, see "Appendix B: Bindings" section below.

## The &lt;menu&gt; element
The `<menu>` element allows you to create a drop-down menu within your UI.

Attributes:

| Attribute      | Required   | Description                                                                                                                                                             |        |
|:---------------|:-----------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------|
| **x**          | (required) | The x position of the menu                                                                                                                                              |        |
| **y**          | (required) | The y position of the menu                                                                                                                                              |        |
| **width**      | (required) | The width of the menu                                                                                                                                                   |        |
| **height**     | (required) | The height of the menu                                                                                                                                                  |        |
| **value**      | (optional) | The is the 1-based index of the menu option that is currently selected. **NOTE: Index numbers for menu items start at 1.** A value of 0 means that no item is selected. |        |
| **visible**    | (optional) | This controls whether or not this button is visible. There are two valid values: `true`, `false`.                                                                       | `true` |
| **tooltip**    | (optional) | A tool tip to display when the user hovers over this control.                                                                                                           |        |

Example:
```xml
<menu x="10" y="40"  width="120" height="30" value="2">
    <!-- Your menu options go here -->
</menu>
```
### The &lt;option&gt; element

In order for your drop-down menu to have options, it must contain `<option>` elements.

Attributes:

| Attribute | Required   | Description              |
|:----------|:-----------|:-------------------------|
| name      | (required) | The name of this element |

That's right. The `<option>` element has only one attribute. In order to have your `<option>` elements actually do something useful, you need to attach bindings to them. Here's an example:

```xml
<menu x="10" y="40"  width="120" height="30" requireSelection="true" placeholderText="Choose..." value="2">
    <option name="Menu Option 1">
        <!-- Set the text of a label element -->
        <binding type="control" level="ui" position="2" parameter="TEXT" translation="fixed_value" translationValue="You chose the first option." />
        <!-- Turn on this group -->
        <binding type="general" level="group" position="0" parameter="ENABLED" translation="fixed_value" translationValue="true" />
        <!-- Turn off this group -->
        <binding type="general" level="group" position="1" parameter="ENABLED" translation="fixed_value" translationValue="false" />
    </option>
    <option name="Menu Option 2">
        <!-- Set the text of a label element -->
        <binding type="control" level="ui" position="2" parameter="TEXT" translation="fixed_value" translationValue="You chose the second option." />
        <!-- Turn off this group -->
        <binding type="general" level="group" position="0" parameter="ENABLED" translation="fixed_value" translationValue="false" />
        <!-- Turn on this group -->
        <binding type="general" level="group" position="1" parameter="ENABLED" translation="fixed_value" translationValue="true" />
    </option>
  </menu>
```

As you can see, this example uses a menu to switch between two groups. It also sets the text of a text label. You'll note the liberal use of the new `fixed_value` translation mode above. This means that when any of these options are selected, a fixed predetermined value is used for the value of that binding. 

## The &lt;xyPad&gt; element
The `<xyPad>` element allows you to create a two-dimensional pad control within your UI. This control functions almost like two sliders, one for the x-axis and one for the y-axis.

Attributes:
- **`x`** (required): The `x` position of your control where (0,0) is the top-left corner
- **`y`** (required): The `y` position of your control where (0,0) is the top-left corner
- **`width`** (required): The width in pixels of the control.
- **`height`** (required): The height in pixels of the control.
- **`markerDiameter`** (optional): The diameter of the marker that moves around the control. Default: 10
- **`markerOutlineColor`** (optional): An 8 digit hex value indicating the color of the marker's outline. See [Appendix A](#appendix-a-the-color-format) for an explanation on these hex values.
- **`markerFillColor`** (optional): An 8 digit hex value indicating the color of the marker's fill. See [Appendix A](#appendix-a-the-color-format) for an explanation on these hex values.
- **`outlineColor`** (optional): An 8 digit hex value indicating the color of the control's outline. See [Appendix A](#appendix-a-the-color-format) for an explanation on these hex values.
- **`bgColor`** (optional): An 8 digit hex value indicating the color of the control's background. See [Appendix A](#appendix-a-the-color-format) for an explanation on these hex values.
- **`tooltip`** (optional): A tool tip to display when the user hovers over this control.

### The &lt;x&gt; and &lt;y&gt; elements

Below the `<xyPad>` element, you can specify `<x>` and `<y>` elements. These elements are used to specify the bindings for the x and y axes of the control.

Example:
```xml
<xyPad x="10" y="10" width="300" height="100" parameterName="Pad" xValue="0.5" yValue="0.5" bgColor="77FFCC00" markerFillColor="FFFFFFFF" outlineColor="77FFFFFFF">
  <x>
    <binding type="amp" level="group" groupIndex="0" parameter="AMP_VOLUME" translation="linear" translationOutputMin="0" translationOutputMax="1" />
    <binding type="amp" level="group" groupIndex="1" parameter="AMP_VOLUME" translation="linear" translationOutputMin="1" translationOutputMax="0" />
  </x>
  <y>
    <binding type="effect" level="instrument" effectIndex="0" parameter="FX_FILTER_FREQUENCY"
        translation="table" 
        translationTable="0,33;0.3,150;0.4,450;0.5,1100;0.7,4100;0.9,11000;1.0001,22000"/>
    </y>
  </xyPad>
```

## The &lt;keyboard&gt; element

The `<keyboard>` element lives underneath the `<ui>` element. This is where you specify settings relating to the on-screen keyboard. There should be only one `<keyboard>` element in your preset file. At this point, the only settings are color ranges which are specified using `<color>` sub-elements.

### The &lt;color&gt; element

You can use `<color>` elements to change the color of portions of the on-screen keyboard. You can have as many `<color>` elements as you like.  Only white keys are affected. It's worth noting that colors specified in the `<color>` elements are overlayed on top of the white keys using a 75% transparency, so choose your colors accordingly. This is done to preserve the readability of the key labels.

```xml    
<DecentSampler>
    <ui>
        <!-- Other stuff here -->
        <keyboard>
            <color loNote="36" hiNote="50" color="FF2C365E" />
            <color loNote="51" hiNote="57" color="FF6D9DC5" />
            <color loNote="58" hiNote="67" color="FFCCF3F5" />
            <color loNote="68" hiNote="73" color="FFE8DA9B" />
            <color loNote="74" hiNote="84" color="FFD19D61" />
        </keyboard>
    </ui>
    <!-- Other stuff here -->
</DecentSampler>

```

| Attribute    |            | Description                                                                                                                                              |
|:-------------|:-----------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **`loNote`** | (required) | The bottom of the range for which this color should be displayed. Format: MIDI Note number.                                                              |
| **`hiNote`** | (required) | The top of the range for which this color should be displayed. Format: MIDI Note number.                                                                 |
| **`color`**  | (required) | A text representation of the color to be used for this key range. See [Appendix A](#appendix-a-the-color-format) for an explanation on these hex values. |
