The &lt;tags&gt; element
========================

The `<tags>` element lives right below your top-level `<DecentSampler>` element. It allows you to specify details about the tags you use throughout your instrument. It is however not actually necessary to include a `<tags>` element for every tag you use. You only need to create this if you want to specify additional details about your tags.

#### The &lt;tag&gt; element
Underneath the `<tags>` element, you can have any number of `<tag>` elements. These specify details for each individual tag that you use throughout your sample mapping. Attributes:

| Attribute       |            | Description                                                                       |
|:----------------|:-----------|:----------------------------------------------------------------------------------|
| **`enabled`**   | (optional) | A number for 0.0 to 1.0 that specifies the initial volume for a tag. Default: 1.0 |
| **`volume`**    | (optional) | A number for 0.0 to 1.0 that specifies the initial volume for a tag. Default: 1.0 |
| **`polyphony`** | (optional) | A whole number that specifies the number of voices allowed for this tag.          |
