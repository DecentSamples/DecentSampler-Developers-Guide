The &lt;tags&gt; element
========================

The `<tags>` element lives right below your top-level `<DecentSampler>` element. It allows you to specify details about the tags you use throughout your instrument. It is however not actually necessary to include a `<tags>` element for every tag you use. You only need to create this if you want to specify additional details about your tags.

## The &lt;tag&gt; element
Underneath the `<tags>` element, you can have any number of `<tag>` elements. These specify details for each individual tag that you use throughout your sample mapping. Attributes:

| Attribute       |            | Description                                                                       |
|:----------------|:-----------|:----------------------------------------------------------------------------------|
| **`enabled`**   | (optional) | Whether or not this tag is enabled. Possible values: true, false. Default: true   |
| **`volume`**    | (optional) | A number for 0.0 to 1.0 that specifies the initial volume for a tag. Default: 1.0 |
| **`polyphony`** | (optional) | A whole number that specifies the number of voices allowed for this tag. Default: -1 (unlimited). Set to 1 for monophonic behavior. |

## Example

This example shows a tag configuration with polyphony control:

```xml
<tags>
  <tag name="voice1" volume="1" pan="0" polyphony="12"/>
</tags>
```

The `polyphony` attribute can be controlled dynamically using bindings with the `TAG_POLYPHONY` parameter. This is useful for creating mono/poly switches:

```xml
<button x="740" y="130" width="12" height="12" style="image" value="0">
  <state name="Poly" mainImage="Images/Checkbox On.png">
    <binding type="general" level="tag" identifier="voice1" 
             parameter="TAG_POLYPHONY" translation="fixed_value" translationValue="12"/>
  </state>
  <state name="Mono" mainImage="Images/Checkbox Off.png">
    <binding type="general" level="tag" identifier="voice1" 
             parameter="TAG_POLYPHONY" translation="fixed_value" translationValue="1"/>
  </state>
</button>
```
