The &lt;buses&gt; element
===========================
As of version 1.12.0, DecentSampler has support for audio buses, which can be used to create a more complex mix of the samples in the sample library as well as route audio to various audio outputs. Sample library designers can specify up to 16 buses in the &lt;buses&gt; element using the &lt;bus&gt; tag. Each bus can have its own volume and audio output settings.

## The &lt;bus&gt; element
Within the `<buses>` element, you can have up to 16 `<bus>` sub-elements. These specify parameters for each individual bus that you would like to have in your sample library. The `<bus>` element has the following attributes:

- `busVolume`: The volume of the bus. This is a floating-point number between 0.0 and 1.0, where 0.0 is silent and 1.0 is full volume.
- `output1Target`: The first audio output for this bus. This is a string that specifies the audio output that the bus should be routed to. The available options are `MAIN_OUTPUT` (the main audio output), `AUX_STEREO_OUTPUT_1` (auxiliary output 1), `AUX_STEREO_OUTPUT_2` (auxiliary output 2), `AUX_STEREO_OUTPUT_3` (auxiliary output 3), `AUX_STEREO_OUTPUT_4` (auxiliary output 4), `AUX_STEREO_OUTPUT_5` (auxiliary output 5), `AUX_STEREO_OUTPUT_6` (auxiliary output 6), `AUX_STEREO_OUTPUT_7` (auxiliary output 7), `AUX_STEREO_OUTPUT_8` (auxiliary output 8), `AUX_STEREO_OUTPUT_9` (auxiliary output 9), `AUX_STEREO_OUTPUT_10` (auxiliary output 10), `AUX_STEREO_OUTPUT_11` (auxiliary output 11), `AUX_STEREO_OUTPUT_12` (auxiliary output 12), `AUX_STEREO_OUTPUT_13` (auxiliary output 13), `AUX_STEREO_OUTPUT_14` (auxiliary output 14), `AUX_STEREO_OUTPUT_15` (auxiliary output 15), and `AUX_STEREO_OUTPUT_16` (auxiliary output 16).
- `output2Target`: The second audio output for this bus. This is a string that specifies the audio output that the bus should be routed to. The available options are the same as for `output1Target`.
- `output3Target`: The third audio output for this bus. This is a string that specifies the audio output that the bus should be routed to. The available options are the same as for `output1Target`.
- `output4Target`: The fourth audio output for this bus. This is a string that specifies the audio output that the bus should be routed to. The available options are the same as for `output1Target`.
- `output5Target`: The fifth audio output for this bus. This is a string that specifies the audio output that the bus should be routed to. The available options are the same as for `output1Target`.
- `output6Target`: The sixth audio output for this bus. This is a string that specifies the audio output that the bus should be routed to. The available options are the same as for `output1Target`.
- `output7Target`: The seventh audio output for this bus. This is a string that specifies the audio output that the bus should be routed to. The available options are the same as for `output1Target`.
- `output8Target`: The eighth audio output for this bus. This is a string that specifies the audio output that the bus should be routed to. The available options are the same as for `output1Target`.
- `output1Volume`: The volume of the audio being sent to the bus' first output. This is a floating-point number between 0.0 and 1.0, where 0.0 is silent and 1.0 is full volume.
- `output2Volume`: The volume of the audio being sent to the bus' second output. This is a floating-point number between 0.0 and 1.0, where 0.0 is silent and 1.0 is full volume.
- `output3Volume`: The volume of the audio being sent to the bus' third output. This is a floating-point number between 0.0 and 1.0, where 0.0 is silent and 1.0 is full volume.
- `output4Volume`: The volume of the audio being sent to the bus' fourth output. This is a floating-point number between 0.0 and 1.0, where 0.0 is silent and 1.0 is full volume.
- `output5Volume`: The volume of the audio being sent to the bus' fifth output. This is a floating-point number between 0.0 and 1.0, where 0.0 is silent and 1.0 is full volume.
- `output6Volume`: The volume of the audio being sent to the bus' sixth output. This is a floating-point number between 0.0 and 1.0, where 0.0 is silent and 1.0 is full volume.
- `output7Volume`: The volume of the audio being sent to the bus' seventh output. This is a floating-point number between 0.0 and 1.0, where 0.0 is silent and 1.0 is full volume.
- `output8Volume`: The volume of the audio being sent to the bus' eighth output. This is a floating-point number between 0.0 and 1.0, where 0.0 is silent and 1.0 is full volume.

Here is an example of a `<buses>` element with two buses:

```xml
<buses>
    <bus busVolume="0.5" output1Target="MAIN_OUTPUT" output2Target="AUX_STEREO_OUTPUT_1" output1Volume="0.8" output2Volume="0.5"/>
    <bus busVolume="0.8" output1Target="AUX_STEREO_OUTPUT_2" output2Target="AUX_STEREO_OUTPUT_3" output1Volume="0.6" output2Volume="0.7"/>
</buses>
```

In this example, there are two buses defined. The first bus has a volume of 0.5 and is routed to the main output with a volume of 0.8 and auxiliary output 1 with a volume of 0.5. The second bus has a volume of 0.8 and is routed to auxiliary outputs 2 and 3 with volumes of 0.6 and 0.7, respectively.

## Effects and buses

Buses can also have effects applied to them. To apply an effect to a bus, you can use the `<effects>` element within the `<bus>` element. The `<effects>` element can contain one or more `<effect>` sub-elements, each of which specifies an effect to apply to the bus. Details on the attributes of the `<effect>` element can be found in the [Effects](the-effects-element.md) section.

Here is an example of a `<bus>` element with an effect applied to it:

```xml
<bus busVolume="0.5" output1Target="MAIN_OUTPUT" output2Target="AUX_STEREO_OUTPUT_1" output1Volume="0.8" output2Volume="0.5">
    <effects>
        <effect type="Reverb" wetDryMix="0.5" roomSize="0.5" damping="0.5"/>
    </effects>
</bus>
```

In this example, a reverb effect is applied to the bus with a wet/dry mix of 0.5, room size of 0.5, and damping of 0.5.

## Using buses in the sample library

Once you have defined buses in your sample library, you can use them in the `<sample>` elements to route the audio of the samples to specific buses. To do this, you can use the `outputXTarget` attributes in the `<sample>`, `<group>`, or `<groups>` elements. The `outputXTarget` attributes specifies the audio output that the sample should be routed to. The available options are the same as for the `output1Target` attribute above, but, with additional options for the buses defined in the `<buses>` element using the format: `BUS_1`, `BUS_2`, ..., `BUS_16`.

Here is an example of a `<group>` element with samples routed to a bus:

```xml
<group output1Target="MAIN_OUTPUT" output1Volume="1.0" output2Target="BUS_1" output2Volume="0.2">
      <sample path="Samples/Volca Keys Poly-V127-29-F0.wav" loNote="17" hiNote="17" rootNote="17"/>
      <!-- More samples here -->
</group>
```

In this example, the audio of the samples is routed to both the main output and to the first bus defined in the `<buses>` element. The volume of the audio sent to the bus is 0.2 (20% of full volume).
