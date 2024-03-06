How to add Dropdown Menus
=========================

## Dropdown Menus

In order to implemented dropdown menus, use the new `<menu>` and `<option>` elements. The `<menu>` element defines where the dropdown menu will show up in the ui, whereas the <option> XML elements determine what menu options it has and what if anything those options actually do:

```xml
<menu x="10" y="40" width="120" height="30" value="2">
  <option name="Menu Option 1">
    <!-- Turn on this group -->
    <binding type="general" level="group" position="0" parameter="ENABLED" 
    translation="fixed_value" translationValue="true" />
    <!-- Turn off this group -->
    <binding type="general" level="group" position="1" parameter="ENABLED" 
    translation="fixed_value" translationValue="false" />
  </option>
  <option name="Menu Option 2">
    <!-- Turn off this group -->
    <binding type="general" level="group" position="0" parameter="ENABLED" 
    translation="fixed_value" translationValue="false" />
    <!-- Turn on this group -->
    <binding type="general" level="group" position="1" parameter="ENABLED" 
    translation="fixed_value" translationValue="true" />
  </option>
</menu>
```

In this example, a menu is being used to switch between two groups (the first menu option turns group 0 on and group 1 off; the section option turns group 0 off and group 1 on). Full documentation for the new `<menu>` and `<option>` elements is [here](#the-ui-element).

### The new `fixed_value` translation type

You’ll note, in the example above, there’s something new in the bindings: the four bindings elements have a `translation` parameter of type **fixed\_value**. This is a new translation type. Up until now, binding translation has strictly been about taking an input parameter (such as a knob value or continuous controller amount) and translating it so that it is useful for some other purpose (it’s our way of being able to do a little bit of math without having a full-blown scripting language). This new **fixed\_value** binding is different. It ignores the input value completely and instead provides whatever is specified in the `translationValue` parameter. In this way, each menu option can have hardcoded values that it provides its bindings when it is selected.