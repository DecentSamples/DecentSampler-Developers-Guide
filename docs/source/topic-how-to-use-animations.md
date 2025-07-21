# How to Use Animations

As of version 1.12.9, DecentSampler has support for simple animations. You can use animations to create more compelling user interfaces. You can also use animations to create visual feedback for your controls. For example, a knob might control an animated visual element that rotates as the knob is turned.

Animations are defined using the `<multiFrameImage>` tag. This tag allows you to specify a series of images that will be displayed in sequence. It uses the same sprite sheet format that is used elsewhere in DecentSampler for defining knob skins. The `<multiFrameImage>` tag has the following attributes:

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

Here is an example of a simple animation that loops continuously:

```xml
<DecentSampler pluginVersion="1">
  <ui bgColor="FFADD8E6">
    <tab>
      <multiFrameImage x="350" y="80" width="64" height="64"  path="Images/AnimationDemo128.png" numFrames="31" sourceFormat="vertical_image_strip"frameRate="24" playbackMode="forward_loop"/>
    </tab>
  </ui>
</DecentSampler>
```

In this example, the image `Images/AnimationDemo128.png` is a vertical image strip with 31 frames. The animation will play at 24 frames per second in a forward loop. The image will be displayed at position (10,10) with a width of 64 pixels and a height of 64 pixels.

## Controlling Animations

### Current Frame

You can control the current frame of an animation using the `currentFrame` attribute. This attribute is a zero-based index that specifies the current frame of the animation. You can use this attribute to create animations that are controlled by other components. For example, you could use a knob to control the current frame of an animation.

Here is an example of an animation that is controlled by a knob:

```xml
<DecentSampler>
  <ui bgColor="FFADD8E6">
    <tab>
      <multiFrameImage x="450" y="80" width="64" height="64" path="Images/AnimationDemo128.png" numFrames="31" sourceFormat="vertical_image_strip"frameRate="24" playbackMode="stopped"/>
      <labeled-knob x="280" y="50" label="Frame" type="integer" minValue="0" maxValue="31" value="0" textColor="FF000000" value="0">
        <binding type="control" level="ui" position="0" parameter="CURRENT_FRAME" translation="linear" translationOutputMin="0" translationOutputMax="31"  />
      </labeled-knob>
    </tab>
  </ui>
</DecentSampler>
```
In this example, the knob controls the current frame of the animation. The `binding` tag is used to bind the knob to the `CURRENT_FRAME` parameter of the animation. The `translation` attribute specifies how the knob value should be translated to the animation frame. In this case, the knob value is linearly translated to the range 0-31, which corresponds to the 31 frames of the animation.

You can download a working example of this code [here](https://github.com/DecentSamples/DecentSampler-Sample-Library-Examples/releases). This is Example 2 in the `example-011-how-to-use-animations` folder.

## Frame Rate

You can control the frame rate of an animation using the `frameRate` attribute. This attribute specifies the number of frames per second that the animation should play at. You can use this attribute to create animations that play at different speeds. For example, you could use a slider to control the frame rate of an animation.

Here is an example of an animation that is controlled by a slider:

```xml
<DecentSampler>
  <ui bgColor="FFADD8E6">
    <tab>
      <multiFrameImage x="450" y="80" width="64" height="64" path="Images/AnimationDemo128.png" numFrames="31" sourceFormat="vertical_image_strip"frameRate="24" playbackMode="forward_loop"/>
      <labeled-knob x="280" y="50" label="Frame Rate" type="integer" minValue="1" maxValue="60" value="24" textColor="FF000000" value="24">
        <binding type="control" level="ui" position="0" parameter="FRAME_RATE" translation="linear" translationOutputMin="1" translationOutputMax="24"  />
      </labeled-knob>
    </tab>
  </ui>
</DecentSampler>
```

In this example, the slider controls the frame rate of the animation. The `binding` tag is used to bind the slider to the `FRAME_RATE` parameter of the animation. The `translation` attribute specifies how the slider value should be translated to the animation frame rate. In this case, the slider value is linearly translated to the range 1-24, which corresponds to the frame rates supported by DecentSampler.

You can download a working example of this code [here](https://github.com/DecentSamples/DecentSampler-Sample-Library-Examples/releases). This is Example 3 in the `example-011-how-to-use-animations` folder.

## Playback Mode

You can control the playback mode of an animation using the `playbackMode` attribute. This attribute specifies how the animation should play. You can use this attribute to create animations that play in different ways. For example, you could use a drop-down menu to select the playback mode of an animation.

Here is an example of an animation that is controlled by a drop-down menu:

```xml
<DecentSampler>
  <ui bgColor="FFADD8E6">
    <tab>
      <multiFrameImage x="450" y="80" width="64" height="64" path="Images/AnimationDemo128.png" numFrames="31" sourceFormat="vertical_image_strip"frameRate="24" playbackMode="forward_loop"/>
      <label x="275" y="50" height="30" text="Playback Mode" textColor="FF000000" textSize="16" />
      <menu x="280" y="80" width="100" height="30">
        <option name="Forward Loop">
            <binding type="control" level="ui" position="0" parameter="PLAYBACK_MODE" translation="fixed_value" translationValue="forward_loop"/>
        </option>
        <option name="Forward Once">
            <binding type="control" level="ui" position="0" parameter="PLAYBACK_MODE" translation="fixed_value" translationValue="forward_once"/>
        </option>
        <option name="Reverse Loop">
            <binding type="control" level="ui" position="0" parameter="PLAYBACK_MODE" translation="fixed_value" translationValue="reverse_loop"/>
        </option>
        <option name="Reverse Once">
            <binding type="control" level="ui" position="0" parameter="PLAYBACK_MODE" translation="fixed_value" translationValue="reverse_once"/>
        </option>
        <option name="Ping Pong Loop">
            <binding type="control" level="ui" position="0" parameter="PLAYBACK_MODE" translation="fixed_value" translationValue="ping_pong_loop"/>
        </option>
        <option name="Stopped">
            <binding type="control" level="ui" position="0" parameter="PLAYBACK_MODE" translation="fixed_value" translationValue="stopped"/>
        </option>
        </menu>
    </tab>
  </ui>
</DecentSampler>
```

In this example, the drop-down menu controls the playback mode of the animation. The `binding` tag is used to bind the menu to the `PLAYBACK_MODE` parameter of the animation. The `translation` attribute specifies how the menu value should be translated to the animation playback mode. In this case, the menu value is translated to one of the valid playback modes: `forward_loop`, `forward_once`, `reverse_loop`, `reverse_once`, `ping_pong_loop`, and `stopped`.

You can download a working example of this code [here](https://github.com/DecentSamples/DecentSampler-Sample-Library-Examples/releases). This is Example 4 in the `example-011-how-to-use-animations` folder.

## Conclusion

In this guide, we have covered how to use animations in DecentSampler. You can use animations to create more compelling user interfaces and provide visual feedback for your controls. You can control animations using the `currentFrame`, `frameRate`, and `playbackMode` attributes. You can also bind animations to other components to create interactive animations.