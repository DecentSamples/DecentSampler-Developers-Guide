# How to do voice-muting for drums

Voice muting makes use of the tags functionality, these are text labels that you can use to identify samples or groups of samples. You start by adding tags to all of your groups (you can also add them to individual samples if you'd like). Next, you add a `silencedByTags` attribute to groups or sample elements that you want to be silenced by other samples. When a sample with a tag matching one of the tags in the `silencedByTags` is played, it will silence the current sample (or group). 

Here’s an example:

```xml
<DecentSampler>
    <groups>
        <group tags="hihat" silencedByTags="hihat" silencingMode="fast">
            <!-- Your samples go here. -->
        </group>
    </groups>
</DecentSampler>
```

Note the use of the `silencingMode` attribute (a value of "fast" means we immediately silence, whereas "normal" means we trigger the ADSR release phase). Alternatively, you can use the `silencingDecay` attribute to specify an exact fade-out time in seconds. For example, `silencingDecay="0.02"` creates a 20ms fade-out when hi-hats are muted, which can sound more natural than an immediate cutoff.