# How to Use Buses and Auxiliary Outputs

As of version 1.12.0, Decent Sampler has support for both audio buses and auxiliary outputs. Buses are a powerful feature that allow you to route audio to different auxiliary outputs and apply effects to the audio. This can be useful for creating complex audio routing setups and adding effects to groups of samples. This tutorial will show you how to use buses and auxiliary outputs in Decent Sampler.

## Auxilary Outputs

By default, all audio in Decent Sampler is routed to the main audio output. However, you can also route audio to secondary,auxiliary outputs. Auxiliary outputs are additional audio outputs that can be used to route audio to external effects processors or other audio devices within your DAW. Decent Sampler supports up to 16 auxiliary outputs, which can be used to create complex audio routing setups.

There are two ways that you can route audio to auxiliary outputs in Decent Sampler:

1. You can use the `outputXTarget` attributes in the `<sample>`, `<group>`, or `<groups>` elements to directly specify the audio output that the sample should be routed to. The available options are `MAIN_OUTPUT` (the main audio output, which is the default) and `AUX_STEREO_OUTPUT_1` through `AUX_STEREO_OUTPUT_16` (the auxiliary outputs).
2. You can route audio from the samples to user-defined buses, and then route audio from the buses to the auxiliary outputs. This allows you to apply effects to the audio before sending it to the auxiliary outputs.

## Buses

As mentioned above, buses can be used to create a more complex mix of the samples in the sample library as well as route audio to various audio outputs. Sample library designers can specify up to 16 buses in the `<buses>` element using the `<bus>` tag. Each bus can have its own volume and audio output settings. For a full list of attributes that can be used in the `<bus>` element, see the [Buses Element](the-buses-element.md) documentation.

## Examples

Here are a few examples of how you can use buses and auxiliary outputs in Decent Sampler:

### Example 1: Routing audio to an auxiliary output

In this example, we will route audio from a sample to an auxiliary output. To do this, we will use the `outputXTarget` attributes in the `<sample>` element to specify the audio output that the sample should be routed to. Here is an example of how you can route audio to `AUX_STEREO_OUTPUT_1`:

```xml
<group output1Target="MAIN_OUTPUT" output1Volume="1.0" output2Target="AUX_STEREO_OUTPUT_1" output2Volume="0.5" />
    <!-- Samples go here -->
</group>
```

In this example, the audio from the samples in the `<group>` element will be routed to the main audio output with a volume of 1.0 and to auxiliary output 1 with a volume of 0.5.

### Example 2: Applying effects to audio using buses

In this example, we will apply an effect to audio using a bus. To do this, we will define a bus with an effect applied to it, and then route audio from the samples to the bus. Here is an example of how you can apply a reverb effect to audio using a bus:

```xml
<buses>
    <bus busVolume="0.5" output1Target="MAIN_OUTPUT" output2Target="AUX_STEREO_OUTPUT_1" output1Volume="0.8" output2Volume="0.5">
        <effects>
            <effect type="Reverb" wetDryMix="0.5" roomSize="0.5" damping="0.5" />
        </effects>
    </bus>
</buses>
<group output1Target="MAIN_OUTPUT" output2Target="BUS_1" output2Volume="1.0">
    <!-- Samples go here -->
</group>
```

In this example, a bus is defined with a reverb effect applied to it. The audio from the samples in the `<group>` element will be routed to the main audio output with a volume of 1.0, as well as to the bus with the reverb effect applied to it.

## Conclusion

Buses and auxiliary outputs are powerful features in Decent Sampler that allow you to create complex audio routing setups and apply effects to groups of samples. By using buses and auxiliary outputs, you can create more dynamic and interesting sample libraries that take full advantage of Decent Sampler's capabilities.
