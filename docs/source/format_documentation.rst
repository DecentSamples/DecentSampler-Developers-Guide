File Format Reference Guide
==========================================

Document Version: 1.10.0

Table of Contents
-----------------

-  `Introduction <#introduction>`__
-  `The top-level <DecentSampler> element
   (required) <#the-top-level-decentsampler-element-required>`__

   -  `The <ui> element (optional) <#the-ui-element-optional>`__

      -  `The <tab> element <#the-tab-element>`__

         -  `The <button> element <#the-button-element>`__

            -  `The <state> element <#the-state-element>`__

         -  `The <image> element <#the-image-element>`__
         -  `The <label> element <#the-label-element>`__
         -  `The <labeled-knob> and <control>
            elements <#the-labeled-knob-and-control-elements>`__
         -  `The <menu> element <#the-menu-element>`__

            -  `The <option> element <#the-option-element>`__

      -  `The <keyboard> element <#the-keyboard-element>`__

         -  `The <color> element <#the-color-element>`__

   -  `The <groups> element (required) <#the-groups-element-required>`__

      -  `The <group> element <#the-group-element>`__

         -  `The <sample> element <#the-sample-element>`__

   -  `The <effects> element <#the-effects-element>`__

      -  `The <effect> element <#the-effect-element>`__

         -  `Low-pass, Band pass, and Hi-pass
            filter <#low-pass-band-pass-and-hi-pass-filter>`__
         -  `Notch EQ filter <#notch-eq-filter>`__
         -  `Peak EQ filter <#peak-eq-filter>`__
         -  `Gain effect <#gain-effect>`__
         -  `Reverb effect <#reverb-effect>`__
         -  `Delay effect <#delay-effect>`__
         -  `Chorus effect <#chorus-effect>`__
         -  `Phaser effect <#phaser-effect>`__
         -  `Convolution effect <#convolution-effect>`__
         -  `Wave Folder effect <#wave-folder-effect>`__
         -  `Wave Shaper effect <#wave-shaper-effect>`__

   -  `The <midi> element <#the-midi-element>`__

      -  `The <cc> element <#the-cc-element>`__
      -  `The <note> element <#the-note-element>`__

   -  `The <modulators> element <#the-modulators-element>`__

      -  `The <lfo> element <#the-lfo-element>`__
      -  `The <envelope> element <#the-envelope-element>`__

   -  `The <tags> element <#the-tags-element>`__

      -  `The <tag> element <#the-tag-element>`__

-  `Appendix A: The Color Format <#appendix-a-the-color-format>`__
-  `Appendix B: The <binding>
   element <#appendix-b-the-binding-element>`__

   -  `Controllable Parameters <#controllable-parameters>`__
   -  `Translation Modes <#translation-modes>`__

-  `Appendix C: Boilerplate .dspreset
   File <#appendix-c-boilerplate-dspreset-file>`__
-  `Appendix D: How to Control Parameters Using Tags (Example: Mic-level
   Knobs) <#appendix-d-how-to-control-parameters-using-tags-example-mic-level-knobs>`__
-  `Appendix E: How to do voice-muting for
   drums <#appendix-e-how-to-do-voice-muting-for-drums>`__
-  `Appendix F: How to implement true
   legato <#appendix-f-how-to-implement-true-legato>`__
-  `Appendix G: Advanced Topics <#appendix-g-advanced-topics>`__

--------------

Introduction
------------

At its core each DecentSampler sample library consists of two things: a
folder containing a bunch of assets like audio files and pictures, and a
single text file (called a **dspreset** file) which describes how the
engine should use all of those files. This reference document is a guide
to creating **dspreset** files.

**dspreset** files are just XML files. As such, each one begins with an
XML declaration:

.. code:: xml

   <?xml version="1.0" encoding="UTF-8"?>

The top-level <DecentSampler> element (required)
------------------------------------------------

At the top level of every **dspreset** file is a ``<DecentSampler>``
element. Every file **must** have one. Here is a list of attributes:

-  **``minVersion``** (optional): This is the minimum version on which
   this preset is known to run. If a user is running an old version of
   DS, and a developer has specified a minVersion for their instrument,
   a dialog box will show up telling users that their version is
   outdated and that they should upgrade in order to get the full
   effect. They *can* than choose to ignore this warning or hit
   download. The dialog box does not show up for iOS users as most of
   them have auto-updates turned on.

Example:

.. code:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <DecentSampler minVersion="1.0.0">
       <!-- More tags go here. :) -->
   </DecentSampler>

Underneath the top-level ``<DecentSampler>`` element you can put any
number of other elements, which are described below:

The <ui> element (optional)
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The ``<ui>`` element is how you specify a user interface for your
instrument. Each **dspreset** should have at most one ``<ui>`` element.
There are several important attributes:

-  **``coverArt``** (optional): A relative or absolutely path to a cover
   art image to use. After the first time this library is opened, this
   will get displayed on the “My Libraries” tab.
-  **``bgImage``** (optional): A relative or absolutely path to a
   background image to use.
-  **``bgColor``** (required): An eight digit hex value indicating the
   background color to be used for the background of the UI. This color
   will be drawn underneath any background image specified by
   ``bgimage``.
-  **``width``** (required): The width of your user interface.
   Recommended value: 812.
-  **``height``** (required): The height of your user interface.
   Recommended value: 375.

Example:

.. code:: xml

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

The <tab> element
^^^^^^^^^^^^^^^^^

The ``<tab>`` element lives underneath the ``<ui>`` element. This
architecture was chosen because we may, at some point, add support for
multiple tabs. At present it is only possible to have a single tab
within DecentSampler instruments. As such, every UI must have at most
one ``<tab>`` element.

Attributes:

-  **``name``** (optional): An optional name to be associated with this
   tab. This is currently not displayed anywhere.

The <button> element
^^^^^^^^^^^^^^^^^^^^

The ``<button>`` element allows you to create a button within your UI.
It lives underneath the ``<tab>`` element. There are two styles of
buttons: text buttons and image buttons.

Attributes:

+---+-------+------------------------------------------------------+---+
| A | Req   | Description                                          | D |
| t | uired |                                                      | e |
| t |       |                                                      | f |
| r |       |                                                      | a |
| i |       |                                                      | u |
| b |       |                                                      | l |
| u |       |                                                      | t |
| t |       |                                                      |   |
| e |       |                                                      |   |
+===+=======+======================================================+===+
| * | (requ | The x position of the menu                           | N |
| * | ired) |                                                      | o |
| ` |       |                                                      | n |
| ` |       |                                                      | e |
| x |       |                                                      |   |
| ` |       |                                                      |   |
| ` |       |                                                      |   |
| * |       |                                                      |   |
| * |       |                                                      |   |
+---+-------+------------------------------------------------------+---+
| * | (requ | The y position of the menu                           | N |
| * | ired) |                                                      | o |
| ` |       |                                                      | n |
| ` |       |                                                      | e |
| y |       |                                                      |   |
| ` |       |                                                      |   |
| ` |       |                                                      |   |
| * |       |                                                      |   |
| * |       |                                                      |   |
+---+-------+------------------------------------------------------+---+
| * | (requ | The width of the menu                                | N |
| * | ired) |                                                      | o |
| ` |       |                                                      | n |
| ` |       |                                                      | e |
| w |       |                                                      |   |
| i |       |                                                      |   |
| d |       |                                                      |   |
| t |       |                                                      |   |
| h |       |                                                      |   |
| ` |       |                                                      |   |
| ` |       |                                                      |   |
| * |       |                                                      |   |
| * |       |                                                      |   |
+---+-------+------------------------------------------------------+---+
| * | (requ | The height of the menu                               | N |
| * | ired) |                                                      | o |
| ` |       |                                                      | n |
| ` |       |                                                      | e |
| h |       |                                                      |   |
| e |       |                                                      |   |
| i |       |                                                      |   |
| g |       |                                                      |   |
| h |       |                                                      |   |
| t |       |                                                      |   |
| ` |       |                                                      |   |
| ` |       |                                                      |   |
| * |       |                                                      |   |
| * |       |                                                      |   |
+---+-------+------------------------------------------------------+---+
| * | (opti | The is the 0-based index of the button state that is | 0 |
| * | onal) | currently selected. A value of 0 means that the      |   |
| ` |       | first state is active.                               |   |
| ` |       |                                                      |   |
| v |       |                                                      |   |
| a |       |                                                      |   |
| l |       |                                                      |   |
| u |       |                                                      |   |
| e |       |                                                      |   |
| ` |       |                                                      |   |
| ` |       |                                                      |   |
| * |       |                                                      |   |
| * |       |                                                      |   |
+---+-------+------------------------------------------------------+---+
| * | (opti | The type of button we want. There are two valid      | ` |
| * | onal) | values: ``text``, ``image``.                         | ` |
| ` |       |                                                      | t |
| ` |       |                                                      | e |
| s |       |                                                      | x |
| t |       |                                                      | t |
| y |       |                                                      | ` |
| l |       |                                                      | ` |
| e |       |                                                      |   |
| ` |       |                                                      |   |
| ` |       |                                                      |   |
| * |       |                                                      |   |
| * |       |                                                      |   |
+---+-------+------------------------------------------------------+---+
| * | (req  | For ``image`` buttons only. The path of the main     | N |
| * | uired | image to display for this button. This can also be   | o |
| ` | for   | set at the state level so that it only applies to a  | n |
| ` | ``im  | specific state.                                      | e |
| m | age`` |                                                      |   |
| a | but   |                                                      |   |
| i | tons) |                                                      |   |
| n |       |                                                      |   |
| I |       |                                                      |   |
| m |       |                                                      |   |
| a |       |                                                      |   |
| g |       |                                                      |   |
| e |       |                                                      |   |
| ` |       |                                                      |   |
| ` |       |                                                      |   |
| * |       |                                                      |   |
| * |       |                                                      |   |
+---+-------+------------------------------------------------------+---+
| * | (opti | For ``image`` buttons only. The path of the main     | N |
| * | onal) | image to display when the user hovers their mouse    | o |
| ` |       | over this button. This can also be set at the state  | n |
| ` |       | level so that it only applies to a specific state.   | e |
| h |       |                                                      |   |
| o |       |                                                      |   |
| v |       |                                                      |   |
| e |       |                                                      |   |
| r |       |                                                      |   |
| I |       |                                                      |   |
| m |       |                                                      |   |
| a |       |                                                      |   |
| g |       |                                                      |   |
| e |       |                                                      |   |
| ` |       |                                                      |   |
| ` |       |                                                      |   |
| * |       |                                                      |   |
| * |       |                                                      |   |
+---+-------+------------------------------------------------------+---+
| * | (opti | For ``image`` buttons only. The path of the main     | N |
| * | onal) | image to display when the user clicks down on this   | o |
| ` |       | button. This can also be set at the state level so   | n |
| ` |       | that it only applies to a specific state.            | e |
| c |       |                                                      |   |
| l |       |                                                      |   |
| i |       |                                                      |   |
| c |       |                                                      |   |
| k |       |                                                      |   |
| I |       |                                                      |   |
| m |       |                                                      |   |
| a |       |                                                      |   |
| g |       |                                                      |   |
| e |       |                                                      |   |
| ` |       |                                                      |   |
| ` |       |                                                      |   |
| * |       |                                                      |   |
| * |       |                                                      |   |
+---+-------+------------------------------------------------------+---+
| * | (opti | This controls whether or not this button is visible. | ` |
| * | onal) | There are two valid values: ``true``, ``false``.     | ` |
| ` |       |                                                      | t |
| ` |       |                                                      | r |
| v |       |                                                      | u |
| i |       |                                                      | e |
| s |       |                                                      | ` |
| i |       |                                                      | ` |
| b |       |                                                      |   |
| l |       |                                                      |   |
| e |       |                                                      |   |
| ` |       |                                                      |   |
| ` |       |                                                      |   |
| * |       |                                                      |   |
| * |       |                                                      |   |
+---+-------+------------------------------------------------------+---+

Example:

.. code:: xml

   <button x="10" y="40"  width="120" height="30" style="image" value="0" mainImage="samples/ButtonMainImage.png" hoverImage="samples/ButtonHoverImage.png" clickImage="samples/ButtonSelectedImage.png">
       <!-- Your button states go here -->
   </button>

The <state> element
'''''''''''''''''''

In order for your button to work, it must contain at least one
``<state>`` elements.

Attributes:

+-----+----------+-----------------------------------------------------+
| Att | Required | Description                                         |
| rib |          |                                                     |
| ute |          |                                                     |
+=====+==========+=====================================================+
| **` | (        | The text to display on a text button when this      |
| `na | required | state is active                                     |
| me` | for      |                                                     |
| `** | ``text`` |                                                     |
|     | buttons) |                                                     |
+-----+----------+-----------------------------------------------------+
| **  | (        | For ``image`` buttons only. The path of the main    |
| ``m | required | image to display for this button when the button is |
| ain | for      | in the current state.                               |
| Ima | `        |                                                     |
| ge` | `image`` |                                                     |
| `** | buttons) |                                                     |
+-----+----------+-----------------------------------------------------+
| **` | (o       | For ``image`` buttons only. The path of the image   |
| `ho | ptional) | to display when the user hovers their mouse over    |
| ver |          | this button when the button is in the current       |
| Ima |          | state.                                              |
| ge` |          |                                                     |
| `** |          |                                                     |
+-----+----------+-----------------------------------------------------+
| **` | (o       | For ``image`` buttons only. The path of the image   |
| `cl | ptional) | to display when the user clicks down on this button |
| ick |          | when the button is in the current state.            |
| Ima |          |                                                     |
| ge` |          |                                                     |
| `** |          |                                                     |
+-----+----------+-----------------------------------------------------+

In order to have your ``<button>`` elements actually do something
useful, you need to attach bindings to them. Here’s an example:

.. code:: xml

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

As you can see, this example uses a button to switch between two groups.
You’ll note the liberal use of the ``fixed_value`` translation mode
above. This means that when any of these options are selected, a fixed
predetermined value is used for the value of that binding.

The <image> element
'''''''''''''''''''

The ``<image>`` element allows you to place a static image into your
user interface. It lives underneath the ``<tab>`` element. Attributes:

-  **``x``** (required): The ``x`` position of your image where (0,0) is
   the top-left corner
-  **``y``** (required): The ``y`` position of your image where (0,0) is
   the top-left corner
-  **``width``** (required): The width in pixels of the image component
-  **``height``** (required): The height in pixels of the image
   component
-  **``path``** (required): The relative path of the image file to show
   in this component
-  **``aspectRatioMode``** (required): Whether or not the engine should
   preserve the aspect ratio of the image. Note: regardless of these
   settings, you still need to specify a width and height for your image
   element. Valid values: ``preserve``, ``stretch``. Default value is
   ``preserve``.
-  **``visible``** (optional): This controls whether or not this image
   is visible. There are two valid values: ``true`` (default),
   ``false``.

The <label> element
'''''''''''''''''''

The ``<label>`` element allows you to place a static block of text into
yoru user interface. It lives underneath the ``<tab>`` element.
Attributes:

-  **``x``** (required): The ``x`` position of your control where (0,0)
   is the top-left corner
-  **``y``** (required): The ``y`` position of your control where (0,0)
   is the top-left corner
-  **``text``** (required): The actual text that should be displayed as
   part of the label.
-  **``textColor``** (optional): An 8 digit hex value indicating the
   text color to be used for the label. See `Appendix
   A <#appendix-a-the-color-format>`__ for an explanation on these hex
   values.
-  **``textSize``** (optional): A font size for the text label. Default:
   12
-  **``width``** (required): The width in pixels of the label.
-  **``height``** (required): The height in pixels of the label.
-  **``vAlign``** (optional): The vertical alignment of the text within
   the box described by the width and height attributes. Valid values:
   ``top``,\ ``bottom``, ``center``. Default is ``center``.
-  **``hAlign``** (optional): The horizontal alignment of the text
   within the box described by the width and height attributes. Valid
   values: ``left``,\ ``right``, ``center``. Default is ``center``.
-  **``visible``** (optional): This controls whether or not this text
   label is visible. There are two valid values: ``true`` (default),
   ``false``.

A label’s text can also be set dynamically using bindings using the
``TEXT`` binding parameter name.

The <labeled-knob> and <control> elements
'''''''''''''''''''''''''''''''''''''''''

The ``<labeled-knob>`` and ``<control>`` elements live underneath the
``<tab>`` element. These tags correspond to user controls (usually round
radial dials) that can be used as part of a UI. These two element types
are the same except that ``<labeled-knob>`` elements contain built-in
labels, where as ``<control>`` elements do not. Every tab can have many
``<control>`` or ``<labeled-knob>`` elements underneath it.

For precise UI creation, it may be advisable to use a combination of
``<control>`` & ``<label>`` elements rather than ``<labeled-knob>``.

Attributes:

-  **``x``** (required): The ``x`` position of your control where (0,0)
   is the top-left corner
-  **``y``** (required): The ``y`` position of your control where (0,0)
   is the top-left corner
-  **``width``** (required): The width in pixels of the knob + label.
-  **``height``** (required): The height in pixels of the knob + label.
-  **``parameterName``** (required): In a situation where the sampler
   does not have enough room to display the full UI, a shrunken down
   version of the UI will be used. In such situations, this control will
   be labeled using the ``parameterName``. It is good practice to always
   include a ``parameterName``. If not ``parameterName`` is specified
   and a value ``label`` is specified, then that will be used instead.
-  **``style``** (optional): The specific kind of control that is
   created. The following values are supported: ``linear_bar``,
   ``linear_bar_vertical``, ``linear_horizontal``, ``linear_vertical``,
   ``rotary``, ``rotary_horizontal_drag``,
   ``rotary_horizontal_vertical_drag``, ``rotary_vertical_drag``,
   ``custom_skin_vertical_drag``, ``custom_skin_horizontal_drag``.
   Default: ``rotary_vertical_drag``.
-  **``showLabel``** (optional): A true/false value dictating whether or
   not a built-in label should be displayed. Default: true for
   ``<labeled-knob>`` and false for ``<control>`` elements
-  **``label``** (optional): If ``showLabel`` is true, the actual text
   that should be displayed above the control.
-  **``parameterName``** (required): In a situation where the sampler
   does not have enough room to display the full UI, a shrunken down
   version of the UI will be used. In such situations, this control will
   be labeled using the ``parameterName``. It is good practice to always
   include a ``parameterName``.
-  **``minValue``** (optional): The minimum value of your control.
   Default: 0
-  **``maxValue``** (optional): The maximum value of your control.
   Default: 1
-  **``value``** (optional): The initial value of your control. Default:
   0
-  **``defaultValue``** (optional): If a user double-clicks on the
   control, the control’s value will be set to this default value. If no
   default value is specified, then nothing will happen on double-click.
-  **``valueType``** (optional): There are three possible values for
   this ``float`` which yields numbers with two decimal points,
   ``integer`` which yields whole numbers, and ``musical_time`` which
   yields musical time increments in beats and measures. This last
   option is for use with the built-in delay effect only. Default: float
-  **``textColor``** (optional): An 8 digit hex value indicating the
   text color to be used for the label. See `Appendix
   A <#appendix-a-the-color-format>`__ for an explanation on these hex
   values.
-  **``textSize``** (optional): A font size for the text label. Default:
   12
-  **``trackForegroundColor``** (optional): An 8 digit hex value
   indicating the foreground color to use for the knob track. See
   `Appendix A <#appendix-a-the-color-format>`__ for an explanation on
   these hex values.
-  **``trackBackgroundColor``** (optional): An 8 digit hex value
   indicating the background color to use for the knob track. See
   `Appendix A <#appendix-a-the-color-format>`__ for an explanation on
   these hex values.
-  **``visible``** (optional): This controls whether or not this control
   is visible. There are two valid values: ``true`` (default),
   ``false``.

It is also possible to use custom control graphics using the following
attributes:

-  **``customSkinImage``** (optional): This is path to an image to use
   for the control. This is expected to be a JPEG or PNG in KnobMan
   format. A huge gallery of compatible knobs can be found here
   (https://www.g200kg.com/en/webknobman/gallery.php).
-  **``customSkinNumFrames``** (optional): The number of animation
   frames contained in the KnobMan image pointed to by
   ``customSkinImage``.
-  **``customSkinImageOrientation``** (optional): The orientation of the
   frames within the KnobMan image pointed to by ``customSkinImage``.
   Valid values: ``horizontal``, ``vertical``. Default: vertical.
-  **``mouseDragSensitivity``** (optional): An integer number describing
   how sensitive the control should be to mouse drags. The higher the
   number, the less sensitive the control will be to mouse movements.

If you are using custom knobs, it’s important that you specify a
``style=`` value of either ``custom_skin_vertical_drag`` or
``custom_skin_horizontal_drag``.

Example:

.. code:: xml

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

To learn how to make knobs actually control parameters of your
instrument, see “Appendix B: Bindings” section below.

The <menu> element
^^^^^^^^^^^^^^^^^^

The ``<menu>`` element allows you to create a drop-down menu within your
UI.

Attributes:

+---+---+-----------------------------------------------------------+---+
| A | R | Description                                               |   |
| t | e |                                                           |   |
| t | q |                                                           |   |
| r | u |                                                           |   |
| i | i |                                                           |   |
| b | r |                                                           |   |
| u | e |                                                           |   |
| t | d |                                                           |   |
| e |   |                                                           |   |
+===+===+===========================================================+===+
| * | ( | The x position of the menu                                |   |
| * | r |                                                           |   |
| x | e |                                                           |   |
| * | q |                                                           |   |
| * | u |                                                           |   |
|   | i |                                                           |   |
|   | r |                                                           |   |
|   | e |                                                           |   |
|   | d |                                                           |   |
|   | ) |                                                           |   |
+---+---+-----------------------------------------------------------+---+
| * | ( | The y position of the menu                                |   |
| * | r |                                                           |   |
| y | e |                                                           |   |
| * | q |                                                           |   |
| * | u |                                                           |   |
|   | i |                                                           |   |
|   | r |                                                           |   |
|   | e |                                                           |   |
|   | d |                                                           |   |
|   | ) |                                                           |   |
+---+---+-----------------------------------------------------------+---+
| * | ( | The width of the menu                                     |   |
| * | r |                                                           |   |
| w | e |                                                           |   |
| i | q |                                                           |   |
| d | u |                                                           |   |
| t | i |                                                           |   |
| h | r |                                                           |   |
| * | e |                                                           |   |
| * | d |                                                           |   |
|   | ) |                                                           |   |
+---+---+-----------------------------------------------------------+---+
| * | ( | The height of the menu                                    |   |
| * | r |                                                           |   |
| h | e |                                                           |   |
| e | q |                                                           |   |
| i | u |                                                           |   |
| g | i |                                                           |   |
| h | r |                                                           |   |
| t | e |                                                           |   |
| * | d |                                                           |   |
| * | ) |                                                           |   |
+---+---+-----------------------------------------------------------+---+
| * | ( | The is the 1-based index of the menu option that is       |   |
| * | o | currently selected. **NOTE: Index numbers for menu items  |   |
| v | p | start at 1.** A value of 0 means that no item is          |   |
| a | t | selected.                                                 |   |
| l | i |                                                           |   |
| u | o |                                                           |   |
| e | n |                                                           |   |
| * | a |                                                           |   |
| * | l |                                                           |   |
|   | ) |                                                           |   |
+---+---+-----------------------------------------------------------+---+
| * | ( | This controls whether or not this button is visible.      | ` |
| * | o | There are two valid values: ``true``, ``false``.          | ` |
| v | p |                                                           | t |
| i | t |                                                           | r |
| s | i |                                                           | u |
| i | o |                                                           | e |
| b | n |                                                           | ` |
| l | a |                                                           | ` |
| e | l |                                                           |   |
| * | ) |                                                           |   |
| * |   |                                                           |   |
+---+---+-----------------------------------------------------------+---+

Example:

.. code:: xml

   <menu x="10" y="40"  width="120" height="30" value="2">
       <!-- Your menu options go here -->
   </menu>

The <option> element
''''''''''''''''''''

In order for your drop-down menu to have options, it must contain
``<option>`` elements.

Attributes:

========= ========== ========================
Attribute Required   Description
========= ========== ========================
name      (required) The name of this element
========= ========== ========================

That’s right. The ``<option>`` element has only one attribute. In order
to have your ``<option>`` elements actually do something useful, you
need to attach bindings to them. Here’s an example:

.. code:: xml

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

As you can see, this example uses a menu to switch between two groups.
It also sets the text of a text label. You’ll note the liberal use of
the new ``fixed_value`` translation mode above. This means that when any
of these options are selected, a fixed predetermined value is used for
the value of that binding.

The <keyboard> element
^^^^^^^^^^^^^^^^^^^^^^

The ``<keyboard>`` element lives underneath the ``<ui>`` element. This
is where you specify settings relating to the on-screen keyboard. There
should be only one ``<keyboard>`` element in your preset file. At this
point, the only settings are color ranges which are specified using
``<color>`` sub-elements.

The <color> element
'''''''''''''''''''

You can use ``<color>`` elements to change the color of portions of the
on-screen keyboard. You can have as many ``<color>`` elements as you
like. Only white keys are affected. It’s worth noting that colors
specified in the ``<color>`` elements are overlayed on top of the white
keys using a 75% transparency, so choose your colors accordingly. This
is done to preserve the readability of the key labels.

.. code:: xml

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

+----+---+------------------------------------------------------------+
| A  |   | Description                                                |
| tt |   |                                                            |
| ri |   |                                                            |
| bu |   |                                                            |
| te |   |                                                            |
+====+===+============================================================+
| ** | ( | The bottom of the range for which this color should be     |
| `` | r | displayed. Format: MIDI Note number.                       |
| lo | e |                                                            |
| No | q |                                                            |
| te | u |                                                            |
| `` | i |                                                            |
| ** | r |                                                            |
|    | e |                                                            |
|    | d |                                                            |
|    | ) |                                                            |
+----+---+------------------------------------------------------------+
| ** | ( | The top of the range for which this color should be        |
| `` | r | displayed. Format: MIDI Note number.                       |
| hi | e |                                                            |
| No | q |                                                            |
| te | u |                                                            |
| `` | i |                                                            |
| ** | r |                                                            |
|    | e |                                                            |
|    | d |                                                            |
|    | ) |                                                            |
+----+---+------------------------------------------------------------+
| *  | ( | A text representation of the color to be used for this key |
| *` | r | range. See `Appendix A <#appendix-a-the-color-format>`__   |
| `c | e | for an explanation on these hex values.                    |
| ol | q |                                                            |
| or | u |                                                            |
| `` | i |                                                            |
| ** | r |                                                            |
|    | e |                                                            |
|    | d |                                                            |
|    | ) |                                                            |
+----+---+------------------------------------------------------------+

The <groups> element (required)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Every **dspreset** file should have one and only one ``<groups>``
element. This is where you specify the samples that make up your sample
library. This element lives right underneath the top-level
``<DecentSampler>`` element. The basic structure is this:

.. code:: xml

   <DecentSampler>
       <groups>
           <group>
               <sample /> <!-- This is where -->
               <sample /> <!-- the samples   -->
               <sample /> <!-- get defined   -->
           </group>
       </groups>
   </DecentSampler>

+---------------------------+----------------+------------------------+
| Attribute                 |                | Description            |
+===========================+================+========================+
| **``volume``**            | (optional)     | The volume of the      |
|                           |                | instrument as a whole. |
|                           |                | This will be reflected |
|                           |                | in the UI in the       |
|                           |                | top-right corner.      |
|                           |                | Value can be in linear |
|                           |                | 0.0-1.0 or in          |
|                           |                | decibels. If it’s in   |
|                           |                | decibels you must      |
|                           |                | append dB after the    |
|                           |                | value (example:        |
|                           |                | “3dB”). Default: 1.0   |
|                           |                | (no volume change)     |
+---------------------------+----------------+------------------------+
| **``globalTuning``**      | (optional)     | Global pitch           |
|                           |                | adjustment for         |
|                           |                | changing note pitch.   |
|                           |                | In semitones. For      |
|                           |                | example 1.0 would be a |
|                           |                | half-step up. Default: |
|                           |                | 0                      |
+---------------------------+----------------+------------------------+
| **``glideTime``**         | (optional)     | The glide/portamento   |
|                           |                | time in seconds. A     |
|                           |                | value of 0.0 would     |
|                           |                | mean no portamento.    |
|                           |                | This value can also be |
|                           |                | set at the ``<group>`` |
|                           |                | and ``<sample>``       |
|                           |                | levels, although most  |
|                           |                | people will want to    |
|                           |                | set it globally at the |
|                           |                | ``<groups>`` level.    |
|                           |                | Default: 0.0           |
+---------------------------+----------------+------------------------+
| **``glideMode``**         | (optional)     | Controls the           |
|                           |                | glide/portamento       |
|                           |                | behavior. Possible     |
|                           |                | values are: ``always`` |
|                           |                | (glide is always       |
|                           |                | performed), ``legato`` |
|                           |                | (glide is performed    |
|                           |                | only when              |
|                           |                | transitioning from one |
|                           |                | note to another), and  |
|                           |                | ``off``. This value    |
|                           |                | can also be set at the |
|                           |                | ``<group>`` and        |
|                           |                | ``<sample>`` levels,   |
|                           |                | although most people   |
|                           |                | will want to set it    |
|                           |                | globally at the        |
|                           |                | ``<groups>`` level.    |
|                           |                | Default: ``legato``    |
+---------------------------+----------------+------------------------+

The <group> element
^^^^^^^^^^^^^^^^^^^

Samples live in groups. There can be many group elements under the
``<groups>`` element. It can be useful to sort your samples into groups
in order to apply similar settings to them or to control them with a
knob. The order of groups in a file matters insofar as bindings will
often reference groups by using an index. The first group in a file is
group 0, the second is group 1, etc.

+---+----------------------------------------------------------------+---+
| A | Description                                                    |   |
| t |                                                                |   |
| t |                                                                |   |
| r |                                                                |   |
| i |                                                                |   |
| b |                                                                |   |
| u |                                                                |   |
| t |                                                                |   |
| e |                                                                |   |
+===+================================================================+===+
| * | Whether or not this group is enabled. Possible values: true,   | ( |
| * | false. Default: true                                           | o |
| ` |                                                                | p |
| ` |                                                                | t |
| e |                                                                | i |
| n |                                                                | o |
| a |                                                                | n |
| b |                                                                | a |
| l |                                                                | l |
| e |                                                                | ) |
| d |                                                                |   |
| ` |                                                                |   |
| ` |                                                                |   |
| * |                                                                |   |
| * |                                                                |   |
+---+----------------------------------------------------------------+---+
| * | The volume of the group. Value can be in linear 0.0-1.0 or in  | ( |
| * | decibels. If it’s in decibels you must append dB after the     | o |
| ` | value (example: “3dB”). Default: 1.0                           | p |
| ` |                                                                | t |
| v |                                                                | i |
| o |                                                                | o |
| l |                                                                | n |
| u |                                                                | a |
| m |                                                                | l |
| e |                                                                | ) |
| ` |                                                                |   |
| ` |                                                                |   |
| * |                                                                |   |
| * |                                                                |   |
+---+----------------------------------------------------------------+---+
| * | The degree to which the velocity of the incoming notes affects | ( |
| * | the volume of the samples in this group. 0 = not at all. 1 =   | o |
| ` | volume is completely determined by incoming velocity. When the | p |
| ` | value is 1, a velocity of 127 (max velocity) yields a gain 1.0 | t |
| a | (full volume), a velocity of 63 (half velocity) yields a gain  | i |
| m | of 0.5 (half volume), etc.                                     | o |
| p |                                                                | n |
| V |                                                                | a |
| e |                                                                | l |
| l |                                                                | ) |
| T |                                                                |   |
| r |                                                                |   |
| a |                                                                |   |
| c |                                                                |   |
| k |                                                                |   |
| ` |                                                                |   |
| ` |                                                                |   |
| * |                                                                |   |
| * |                                                                |   |
+---+----------------------------------------------------------------+---+
| * | Group-level pitch adjustment for changing note pitch. In       | ( |
| * | semitones. For example 1.0 would be a half-step up and -1      | o |
| ` | would a half-step down. Default: 0                             | p |
| ` |                                                                | t |
| g |                                                                | i |
| r |                                                                | o |
| o |                                                                | n |
| u |                                                                | a |
| p |                                                                | l |
| T |                                                                | ) |
| u |                                                                |   |
| n |                                                                |   |
| i |                                                                |   |
| n |                                                                |   |
| g |                                                                |   |
| ` |                                                                |   |
| ` |                                                                |   |
| * |                                                                |   |
| * |                                                                |   |
+---+----------------------------------------------------------------+---+

The <sample> element
''''''''''''''''''''

Underneath the ``<group>`` elements are ``<sample>`` elements. Each
sample corresponds to a playable “zone” of your instrument. Attributes:

+----------------------------------+-------------+--------------------+
| Attribute                        |             | Description        |
+==================================+=============+====================+
| **``path``**                     | (required)  | The relative path  |
|                                  |             | of the sample file |
|                                  |             | to play for this   |
|                                  |             | zone.              |
+----------------------------------+-------------+--------------------+
| **``rootNote``**                 | (required)  | The MIDI note      |
|                                  |             | number (from 1 to  |
|                                  |             | 127) of the note.  |
+----------------------------------+-------------+--------------------+
| **``loNote``**                   | (optional)  | The MIDI note      |
|                                  |             | number (from 1 to  |
|                                  |             | 127) of the lowest |
|                                  |             | note for which the |
|                                  |             | zone should be     |
|                                  |             | triggered.         |
|                                  |             | Default: 0.        |
+----------------------------------+-------------+--------------------+
| **``hiNote``**                   | (optional)  | The MIDI note      |
|                                  |             | number (from 1 to  |
|                                  |             | 127) of the        |
|                                  |             | highest note for   |
|                                  |             | which the zone     |
|                                  |             | should be          |
|                                  |             | triggered.         |
|                                  |             | Default: 127.      |
+----------------------------------+-------------+--------------------+
| **``loVel``**                    | (optional)  | The lowest         |
|                                  |             | velocity for which |
|                                  |             | this zone should   |
|                                  |             | be triggered.      |
|                                  |             | Default: 0         |
+----------------------------------+-------------+--------------------+
| **``hiVel``**                    | (optional)  | The highest        |
|                                  |             | velocity for which |
|                                  |             | this zone should   |
|                                  |             | be triggered.      |
|                                  |             | Default: 127       |
+----------------------------------+-------------+--------------------+
| **``loCCN``** **``hiCCN``**      | (optional)  | Using these        |
|                                  |             | parameter, you can |
|                                  |             | use MIDI           |
|                                  |             | continuous         |
|                                  |             | controllers to     |
|                                  |             | filter whether or  |
|                                  |             | not a note should  |
|                                  |             | be played. This    |
|                                  |             | lets you, for      |
|                                  |             | example, have one  |
|                                  |             | set of samples     |
|                                  |             | that get played    |
|                                  |             | when the piano     |
|                                  |             | sustain pedal is   |
|                                  |             | down and another   |
|                                  |             | set that get       |
|                                  |             | played when it is  |
|                                  |             | up. Each time a    |
|                                  |             | MIDI CC value      |
|                                  |             | comes for a        |
|                                  |             | specific CC#, the  |
|                                  |             | engine stores that |
|                                  |             | value. When a      |
|                                  |             | “note on” signal   |
|                                  |             | is received, the   |
|                                  |             | engine makes a     |
|                                  |             | decision (based on |
|                                  |             | the last received  |
|                                  |             | value and the      |
|                                  |             | range defined by   |
|                                  |             | these attributes)  |
|                                  |             | about whether or   |
|                                  |             | not this sample    |
|                                  |             | should be played.  |
|                                  |             | If you use         |
|                                  |             | ``loCCN``, you     |
|                                  |             | must also use a    |
|                                  |             | corresponding      |
|                                  |             | ``hiCCN`` for the  |
|                                  |             | same MIDI CC       |
|                                  |             | number so that you |
|                                  |             | are defining a     |
|                                  |             | range of values.   |
|                                  |             | Example:           |
|                                  |             | ``loCC64="90"``    |
|                                  |             | and                |
|                                  |             | ``hiCC64="127"``   |
|                                  |             | would mean that a  |
|                                  |             | “note on” message  |
|                                  |             | will only trigger  |
|                                  |             | this sample if the |
|                                  |             | last received      |
|                                  |             | value for CC64     |
|                                  |             | (Sustain Pedal) is |
|                                  |             | between 90 and     |
|                                  |             | 127. This can also |
|                                  |             | be set at the      |
|                                  |             | ``<group>`` level. |
|                                  |             | Default:-1 (off)   |
+----------------------------------+-------------+--------------------+
| **``start``**                    | (optional)  | The frame/sample   |
|                                  |             | position of the    |
|                                  |             | start of the       |
|                                  |             | sample audio. This |
|                                  |             | is useful if the   |
|                                  |             | sample starts      |
|                                  |             | midway through the |
|                                  |             | audio file.        |
|                                  |             | Default: 0         |
+----------------------------------+-------------+--------------------+
| **``end``**                      | (optional)  | The frame/sample   |
|                                  |             | position of the    |
|                                  |             | end of the sample  |
|                                  |             | audio. The is      |
|                                  |             | useful is the zone |
|                                  |             | ends before the    |
|                                  |             | end of the audio   |
|                                  |             | file. Default: the |
|                                  |             | file’s length in   |
|                                  |             | samples minus 1.   |
+----------------------------------+-------------+--------------------+
| **``tuning``**                   | (optional)  | A fine-tuning      |
|                                  |             | number (in         |
|                                  |             | semitones) for     |
|                                  |             | changing the note  |
|                                  |             | pitch. e.g 1.0     |
|                                  |             | would be a         |
|                                  |             | half-step up.      |
|                                  |             | Default: 0         |
+----------------------------------+-------------+--------------------+
| **``volume``**                   | (optional)  | The volume of the  |
|                                  |             | sample. Value can  |
|                                  |             | be in linear       |
|                                  |             | 0.0-1.0 or in      |
|                                  |             | decibels. If it’s  |
|                                  |             | in decibels you    |
|                                  |             | must append dB     |
|                                  |             | after the value    |
|                                  |             | (example: “3dB”).  |
|                                  |             | Default: 1.0       |
+----------------------------------+-------------+--------------------+
| **``pan``**                      | (optional)  | A number of -100   |
|                                  |             | to 100. -100 in    |
|                                  |             | panned all the way |
|                                  |             | to the left, 100   |
|                                  |             | is panned all the  |
|                                  |             | way to the right.  |
|                                  |             | This can also be   |
|                                  |             | set at the         |
|                                  |             | ``<group>`` or     |
|                                  |             | ``<groups>``       |
|                                  |             | levels. Default: 0 |
+----------------------------------+-------------+--------------------+
| **``pitchKeyTrack``**            | (optional)  | A number from 0.0  |
|                                  |             | to 1.0. 0 means    |
|                                  |             | that the pitch     |
|                                  |             | will stay the same |
|                                  |             | regardless of what |
|                                  |             | note is played. 1  |
|                                  |             | means that the     |
|                                  |             | pitch will         |
|                                  |             | increase by one    |
|                                  |             | semitone when the  |
|                                  |             | note increases by  |
|                                  |             | one semitone       |
|                                  |             | (i.e. normal key   |
|                                  |             | pitch tracking).   |
|                                  |             | This can also be   |
|                                  |             | set at the         |
|                                  |             | ``<group>`` level. |
|                                  |             | Default: 1         |
+----------------------------------+-------------+--------------------+
| **``trigger``**                  | (optional)  | Valid values:      |
|                                  |             | ``attack`` means a |
|                                  |             | sample is played   |
|                                  |             | when the *note on* |
|                                  |             | message is         |
|                                  |             | received.          |
|                                  |             | ``release`` means  |
|                                  |             | the sample is      |
|                                  |             | played when the    |
|                                  |             | *note off* message |
|                                  |             | is received (aka a |
|                                  |             | release trigger).  |
|                                  |             | ``first`` means    |
|                                  |             | that the sample    |
|                                  |             | will only be       |
|                                  |             | played if no other |
|                                  |             | notes are playing. |
|                                  |             | ``legato`` means   |
|                                  |             | that the sample    |
|                                  |             | will only be       |
|                                  |             | played if *some*   |
|                                  |             | other notes are    |
|                                  |             | already playing.   |
|                                  |             | This can also be   |
|                                  |             | set at the         |
|                                  |             | ``<group>`` level. |
|                                  |             | Default:           |
|                                  |             | ``attack``.        |
+----------------------------------+-------------+--------------------+
| **``tags``**                     | (optional)  | A                  |
|                                  |             | command-separated  |
|                                  |             | list of tags.      |
|                                  |             | Example:           |
|                                  |             | tags=“rt,mic1”.    |
|                                  |             | These are useful   |
|                                  |             | when controlling   |
|                                  |             | volumes using      |
|                                  |             | tags. See Appendix |
|                                  |             | D.                 |
+----------------------------------+-------------+--------------------+
| **``onLoCCN``** **``onHiCCN``**  | (optional)  | If you want a      |
|                                  |             | sample to be       |
|                                  |             | triggered when a   |
|                                  |             | MIDI CC controller |
|                                  |             | message comes in,  |
|                                  |             | for example for    |
|                                  |             | piano pedal down   |
|                                  |             | and pedal up       |
|                                  |             | samples, you use   |
|                                  |             | these attributes   |
|                                  |             | to specify the     |
|                                  |             | range of values    |
|                                  |             | that should        |
|                                  |             | trigger the        |
|                                  |             | sample. If you use |
|                                  |             | ``onLoCCN``, you   |
|                                  |             | must also use a    |
|                                  |             | corresponding      |
|                                  |             | ``onHiCCN`` for    |
|                                  |             | the same MIDI CC   |
|                                  |             | number. Example:   |
|                                  |             | ``onLoCC64="90"``  |
|                                  |             | and                |
|                                  |             | ``onHiCC64="127"`` |
|                                  |             | would mean that    |
|                                  |             | values of CC64     |
|                                  |             | (Sustain Pedal)    |
|                                  |             | between 90 and 127 |
|                                  |             | will trigger the   |
|                                  |             | given sample. This |
|                                  |             | can also be set at |
|                                  |             | the ``<group>``    |
|                                  |             | level. Default:-1  |
|                                  |             | (off)              |
+----------------------------------+-------------+--------------------+
| **``playbackMode``**             | (optional)  | By default, the    |
|                                  |             | choice of playback |
|                                  |             | engine is left up  |
|                                  |             | to the discretion  |
|                                  |             | of the user–they   |
|                                  |             | can set this in    |
|                                  |             | the Preferences    |
|                                  |             | screen. However, a |
|                                  |             | sample creator can |
|                                  |             | override this      |
|                                  |             | setting by setting |
|                                  |             | the playbackMode   |
|                                  |             | for a specific     |
|                                  |             | sample, a group,   |
|                                  |             | or the entire      |
|                                  |             | instrument.        |
|                                  |             | Possible values    |
|                                  |             | are ``memory``,    |
|                                  |             | `                  |
|                                  |             | `disk_streaming``, |
|                                  |             | and ``auto``       |
|                                  |             | (default).         |
+----------------------------------+-------------+--------------------+

**Looping**

+--------------------------------+--------------+----------------------+
| Attribute                      |              | Description          |
+================================+==============+======================+
| **``loopStart``**              | (optional)   | The frame/sample     |
|                                |              | position of the      |
|                                |              | start of the         |
|                                |              | sample’s loop. If    |
|                                |              | this is not          |
|                                |              | specified, but the   |
|                                |              | sample is a wave     |
|                                |              | file with embedded   |
|                                |              | loop markers, those  |
|                                |              | will be used         |
|                                |              | instead. Default: 0  |
+--------------------------------+--------------+----------------------+
| **``loopEnd``**                | (optional)   | The frame/sample     |
|                                |              | position of the end  |
|                                |              | of the sample’s      |
|                                |              | loop. If this is not |
|                                |              | specified, but the   |
|                                |              | sample is a wave     |
|                                |              | file with embedded   |
|                                |              | loop markers, those  |
|                                |              | will be used         |
|                                |              | instead. Default:    |
|                                |              | the file’s length in |
|                                |              | samples minus 1.     |
+--------------------------------+--------------+----------------------+
| **``loopCrossfade``**          | (optional)   | When loop crossfades |
|                                |              | are used, instead of |
|                                |              | simply looping at a  |
|                                |              | specific end point,  |
|                                |              | a portion of the     |
|                                |              | audio from before    |
|                                |              | the loop point is    |
|                                |              | faded in just as the |
|                                |              | audio from the end   |
|                                |              | of the loop is faded |
|                                |              | out. In this way,    |
|                                |              | smooth audio loops   |
|                                |              | can be achieved on   |
|                                |              | samples that weren’t |
|                                |              | specifically         |
|                                |              | prepared as looping. |
|                                |              | This parameter is    |
|                                |              | used for specifying  |
|                                |              | the length of the    |
|                                |              | crossade region in   |
|                                |              | frames/samples. This |
|                                |              | can also be set at   |
|                                |              | the ``<group>``      |
|                                |              | level. Default: 0    |
|                                |              | (crossfades off).    |
+--------------------------------+--------------+----------------------+
| **``loopCrossfadeMode``**      | (optional)   | This parameter is    |
|                                |              | used to specify the  |
|                                |              | curve used for       |
|                                |              | crossfading when     |
|                                |              | loop crossfades are  |
|                                |              | turned on. This can  |
|                                |              | also be set at the   |
|                                |              | ``<group>`` level.   |
|                                |              | Value values:        |
|                                |              | ``linear``,          |
|                                |              | ``equal_power``.     |
|                                |              | Default:             |
|                                |              | ``equal_power``.     |
+--------------------------------+--------------+----------------------+
| **``loopEnabled``**            | (optional)   | A boolean value      |
|                                |              | indicating whether   |
|                                |              | or not the loop      |
|                                |              | should be used.      |
|                                |              | Valid values: true,  |
|                                |              | false                |
+--------------------------------+--------------+----------------------+

**Amplitude Envelope**

Each sample has its own ADSR amplitude envelope.

+--------------------------+----------------+-------------------------+
| Attribute                |                | Description             |
+==========================+================+=========================+
| **``attack``**           | (optional)     | The attack time in      |
|                          |                | seconds of the          |
|                          |                | amplitude envelope of   |
|                          |                | this zone. This can     |
|                          |                | also be set at the      |
|                          |                | ``<group>`` or          |
|                          |                | ``<groups>`` levels.    |
+--------------------------+----------------+-------------------------+
| **``decay``**            | (optional)     | The decay time in       |
|                          |                | seconds of the          |
|                          |                | amplitude envelope of   |
|                          |                | this zone. This can     |
|                          |                | also be set at the      |
|                          |                | ``<group>`` or          |
|                          |                | ``<groups>`` levels.    |
+--------------------------+----------------+-------------------------+
| **``sustain``**          | (optional)     | The sustain level (0.0  |
|                          |                | - 1.0) of the amplitude |
|                          |                | envelope of this zone.  |
|                          |                | This can also be set at |
|                          |                | the ``<group>`` or      |
|                          |                | ``<groups>`` levels.    |
+--------------------------+----------------+-------------------------+
| **``release``**          | (optional)     | The release time in     |
|                          |                | seconds of the          |
|                          |                | amplitude envelope of   |
|                          |                | this zone. This can     |
|                          |                | also be set at the      |
|                          |                | ``<group>`` or          |
|                          |                | ``<groups>`` levels.    |
+--------------------------+----------------+-------------------------+

The curve shapes of the attack, decay, and release zones can be changed
as well. All three of the of the following parameters use the same
system: a value from -100 to 100 that determines the shape of the curve.
**-100 is a logarithmic curve, 0 is a linear curve, and 100 is an
exponential curve.**

.. raw:: html

   <p align="center">

.. raw:: html

   </p>

+--------------------+------------+-------------------+---------------+
| Attribute          |            | Description       | Default Value |
+====================+============+===================+===============+
| *                  | (optional) | A value from -100 | -100          |
| *``attackCurve``** |            | to 100 that       | (logarithmic) |
|                    |            | determines the    |               |
|                    |            | shape of the      |               |
|                    |            | attack curve.     |               |
|                    |            | This can also be  |               |
|                    |            | set at the        |               |
|                    |            | ``<group>`` or    |               |
|                    |            | ``<groups>``      |               |
|                    |            | levels.           |               |
+--------------------+------------+-------------------+---------------+
| **``decayCurve``** | (optional) | A value from -100 | 100           |
|                    |            | to 100 that       | (exponential) |
|                    |            | determines the    |               |
|                    |            | shape of the      |               |
|                    |            | decay curve. The  |               |
|                    |            | decay time in     |               |
|                    |            | seconds of the    |               |
|                    |            | amplitude         |               |
|                    |            | envelope of this  |               |
|                    |            | zone. This can    |               |
|                    |            | also be set at    |               |
|                    |            | the ``<group>``   |               |
|                    |            | or ``<groups>``   |               |
|                    |            | levels.           |               |
+--------------------+------------+-------------------+---------------+
| **                 | (optional) | A value from -100 | 100           |
| ``releaseCurve``** |            | to 100 that       | (exponential) |
|                    |            | determines the    |               |
|                    |            | shape of the      |               |
|                    |            | release curves.   |               |
|                    |            | The release time  |               |
|                    |            | in seconds of the |               |
|                    |            | amplitude         |               |
|                    |            | envelope of this  |               |
|                    |            | zone. This can    |               |
|                    |            | also be set at    |               |
|                    |            | the ``<group>``   |               |
|                    |            | or ``<groups>``   |               |
|                    |            | levels.           |               |
+--------------------+------------+-------------------+---------------+

**Round Robins**

Round robins allow different samples to be played each time a zone is
triggered. This is especially useful with sounds that have short attacks
(such as drums), and is a great way to keep your sample libraries from
sounding fake. In order for round robins to work, you must specify both
a ``seqMode`` and a ``seqPosition`` for all samples. If you have several
different sets of round robins with different lengths, you’ll want to
set the ``seqLength`` value as well. There are several round-robin
modes:

-  ``round_robin``: This causes samples to be triggered sequentially
   according to their ``seqPosition`` values.
-  ``random``: This causes random samples to be chosen from within the
   group of samples. If there are more than two round robins, then the
   algorithm makes sure not to hit the same one twice in a row.
-  ``true_random``: This causes random samples to be chosen from within
   the group of samples.
-  ``always``: This just turns round robins off.

+----+---+--------------------------------------------------------------+
| A  |   | Description                                                  |
| tt |   |                                                              |
| ri |   |                                                              |
| bu |   |                                                              |
| te |   |                                                              |
+====+===+==============================================================+
| *  | ( | Valid values are ``random``, ``true_random``,                |
| *` | o | ``round_robin``, and ``always``. A value indicating the      |
| `s | p | desired round robin behavior for this sample or group of     |
| eq | t | samples. This can also be set at the ``<group>`` and         |
| Mo | i | ``<groups>`` levels. Default: ``always``                     |
| de | o |                                                              |
| `` | n |                                                              |
| ** | a |                                                              |
|    | l |                                                              |
|    | ) |                                                              |
+----+---+--------------------------------------------------------------+
| *  | ( | The length of the round robin queue. This can also be set at |
| *` | o | the ``<group>`` or ``<groups>`` levels. If this is left out, |
| `s | p | then the engine will try to auto-detect the length of the    |
| eq | t | roudn robin sequence. Default: 0                             |
| Le | i |                                                              |
| ng | o |                                                              |
| th | n |                                                              |
| `` | a |                                                              |
| ** | l |                                                              |
|    | ) |                                                              |
+----+---+--------------------------------------------------------------+
| *  | ( | A number indicating this zone’s position in the round robin  |
| *` | o | queue. This can also be set at the ``<group>`` level.        |
| `s | p | Default: 1                                                   |
| eq | t |                                                              |
| Po | i |                                                              |
| si | o |                                                              |
| ti | n |                                                              |
| on | a |                                                              |
| `` | l |                                                              |
| ** | ) |                                                              |
+----+---+--------------------------------------------------------------+

**Voice-Muting / Legato**

**Looping**

+---+---+------------------------------------------------------------------+
| A |   | Description                                                      |
| t |   |                                                                  |
| t |   |                                                                  |
| r |   |                                                                  |
| i |   |                                                                  |
| b |   |                                                                  |
| u |   |                                                                  |
| t |   |                                                                  |
| e |   |                                                                  |
+===+===+==================================================================+
| * | ( | A command-separated list of tags. Example: tags=“rt,mic1”. If a  |
| * | o | sample containing one of these tags gets triggered, then this    |
| ` | p | sample will be stopped. This is useful when setting up drums as  |
| ` | t | it will allow you mute one hi-hat when another hi-hat plays. See |
| s | i | Appendix D.                                                      |
| i | o |                                                                  |
| l | n |                                                                  |
| e | a |                                                                  |
| n | l |                                                                  |
| c | ) |                                                                  |
| e |   |                                                                  |
| d |   |                                                                  |
| B |   |                                                                  |
| y |   |                                                                  |
| T |   |                                                                  |
| a |   |                                                                  |
| g |   |                                                                  |
| s |   |                                                                  |
| ` |   |                                                                  |
| ` |   |                                                                  |
| * |   |                                                                  |
| * |   |                                                                  |
+---+---+------------------------------------------------------------------+
| * | ( | Controls how quickly voices get silenced. ``fast`` =             |
| * | o | immediately; ``normal`` = triggers the sample’s release phase.   |
| ` | p | This second option, when used in conjunction with the            |
| ` | t | ``release`` attribute, allows you to specify a longer release    |
| s | i | time. Values: ``fast``, ``normal``. Default: ``fast``            |
| i | o |                                                                  |
| l | n |                                                                  |
| e | a |                                                                  |
| n | l |                                                                  |
| c | ) |                                                                  |
| i |   |                                                                  |
| n |   |                                                                  |
| g |   |                                                                  |
| M |   |                                                                  |
| o |   |                                                                  |
| d |   |                                                                  |
| e |   |                                                                  |
| ` |   |                                                                  |
| ` |   |                                                                  |
| * |   |                                                                  |
| * |   |                                                                  |
+---+---+------------------------------------------------------------------+
| * | ( | Only play this sample if the previously triggered note equals    |
| * | o | one of these notes. Format: a comma-separated list of MIDI note  |
| ` | p | numbers (from 1 to 127) of the note.                             |
| ` | t |                                                                  |
| p | i |                                                                  |
| r | o |                                                                  |
| e | n |                                                                  |
| v | a |                                                                  |
| i | l |                                                                  |
| o | ) |                                                                  |
| u |   |                                                                  |
| s |   |                                                                  |
| N |   |                                                                  |
| o |   |                                                                  |
| t |   |                                                                  |
| e |   |                                                                  |
| s |   |                                                                  |
| ` |   |                                                                  |
| ` |   |                                                                  |
| * |   |                                                                  |
| * |   |                                                                  |
+---+---+------------------------------------------------------------------+
| * | ( | This is similar to the ``previousNote`` attribute. This causes   |
| * | o | the engine to only play the sample if the previously triggered   |
| ` | p | note is exactly this semitone distance from the previous note.   |
| ` | t | For example, if the note for which this sample is being          |
| l | i | triggered is a C3 and the ``legatoInterval`` is set to ``-2``,   |
| e | o | then the sample will only play if the previous note was a D3     |
| g | n | because D3 minus 2 semitones equals C3. Format: This can be a    |
| a | a | positive or negative whole number.                               |
| t | l |                                                                  |
| o | ) |                                                                  |
| I |   |                                                                  |
| n |   |                                                                  |
| t |   |                                                                  |
| e |   |                                                                  |
| r |   |                                                                  |
| v |   |                                                                  |
| a |   |                                                                  |
| l |   |                                                                  |
| ` |   |                                                                  |
| ` |   |                                                                  |
| * |   |                                                                  |
| * |   |                                                                  |
+---+---+------------------------------------------------------------------+

The <effects> element
~~~~~~~~~~~~~~~~~~~~~

Adding global, instrument-wide effects is easy: just add an
``<effects>`` element right below your top-level ``<DecentSampler>``
element.

It is also possible to have effects that only get added to a specific
group. To adding effects that only apply to a specific group, all you
need to do is create an ``<effects>`` group that lives underneath the
``<group>`` element for the group you want to affect.

Group level effects are initialized every time a note is started and
destroyed every time a note is stopped. If you play two notes
simultaneously, two instances of this effect will be created and these
will be independent of eachother. As a result, they use more CPU than
global effects.

NOTE: Only certain effects will work as group-level effects: lowpass
filter, hipass filter, bandpass filter, gain, and chorus. Delay and
reverb cannot work properly as they will be deleted before their tail
peters out.

The <effect> element
^^^^^^^^^^^^^^^^^^^^

Within the ``<effects>`` element, you can have any number of
``<effect>`` sub-elements. These specify parameters for each individual
effect that you would like to have in your global effects chain. There
are currently only a handful effects available although more could
definitely be added on request:

Low-pass, Band pass, and Hi-pass filter
'''''''''''''''''''''''''''''''''''''''

A 2-pole resonant filter that can be either a lowpass, bandpass, or
highpass filter

Example:

.. code:: xml

   <DecentSampler>
     <effects>
       <effect type="lowpass" resonance="0.7" frequency="22000" />
     </effects>
   </DecentSampler>

There is also a single pole version of the lowpass filter that can be
accessed using the ``lowpass_1pl`` effect type. This version does not
have a resonance parameter.

.. code:: xml

   <DecentSampler>
     <effects>
       <effect type="lowpass_1pl" frequency="22000" />
     </effects>
   </DecentSampler>

Attributes:

+-----+---+------------+------------------------------------------+---+
| Att |   | Type       | Valid Range                              | D |
| rib |   |            |                                          | e |
| ute |   |            |                                          | f |
|     |   |            |                                          | a |
|     |   |            |                                          | u |
|     |   |            |                                          | l |
|     |   |            |                                          | t |
+=====+===+============+==========================================+===+
| ``  | R | The type   | Must be either ``lowpass`` (legacy:      |   |
| typ | e | of filter  | ``lowpass_4pl``), ``lowpass_1pl``,       |   |
| e`` | q |            | ``bandpass``, or ``highpass``            |   |
|     | u |            |                                          |   |
|     | i |            |                                          |   |
|     | r |            |                                          |   |
|     | e |            |                                          |   |
|     | d |            |                                          |   |
+-----+---+------------+------------------------------------------+---+
| `   | O | The filter | 0 - 1.0, where 1.0 is big, 0 is small.   | 0 |
| `re | p | resonance  |                                          | . |
| son | t | (Q)        |                                          | 7 |
| anc | i |            |                                          |   |
| e`` | o |            |                                          |   |
|     | n |            |                                          |   |
|     | a |            |                                          |   |
|     | l |            |                                          |   |
+-----+---+------------+------------------------------------------+---+
| `   | O | The filter | 0 - 1.0, where 0 is not damped, 1.0 is   | 0 |
| `fr | p | frequency  | fully damped.                            | . |
| equ | t |            |                                          | 3 |
| enc | i |            |                                          |   |
| y`` | o |            |                                          |   |
|     | n |            |                                          |   |
|     | a |            |                                          |   |
|     | l |            |                                          |   |
+-----+---+------------+------------------------------------------+---+

Notch EQ Filter
'''''''''''''''

A simple notch filter.

Example:

.. code:: xml

   <DecentSampler>
     <effects>
       <effect type="notch"  q="0.7" frequency="22000" />
     </effects>
   </DecentSampler>

Attributes:

+-----+---+------------+------------------------------------------+---+
| Att |   | Type       | Valid Range                              | D |
| rib |   |            |                                          | e |
| ute |   |            |                                          | f |
|     |   |            |                                          | a |
|     |   |            |                                          | u |
|     |   |            |                                          | l |
|     |   |            |                                          | t |
+=====+===+============+==========================================+===+
| ``  | R | The type   | Must be ``notch``                        |   |
| typ | e | of filter  |                                          |   |
| e`` | q |            |                                          |   |
|     | u |            |                                          |   |
|     | i |            |                                          |   |
|     | r |            |                                          |   |
|     | e |            |                                          |   |
|     | d |            |                                          |   |
+-----+---+------------+------------------------------------------+---+
| `   | R | The filter | 60 - 22000.0                             | 1 |
| `fr | e | frequency  |                                          | 0 |
| equ | q |            |                                          | 0 |
| enc | u |            |                                          | 0 |
| y`` | i |            |                                          | 0 |
|     | r |            |                                          |   |
|     | e |            |                                          |   |
|     | d |            |                                          |   |
+-----+---+------------+------------------------------------------+---+
| ``  | O | Q is the   | 0.01 - 18.0                              | 0 |
| Q`` | p | ratio of   |                                          | . |
|     | t | center     |                                          | 7 |
|     | i | frequency  |                                          |   |
|     | o | to         |                                          |   |
|     | n | bandwidth  |                                          |   |
|     | a |            |                                          |   |
|     | l |            |                                          |   |
+-----+---+------------+------------------------------------------+---+

Peak EQ Filter
''''''''''''''

A peak filter centred around a given frequency, with a variable Q and
gain.

Example:

.. code:: xml

   <DecentSampler>
     <effects>
       <effect type="peak"  q="0.7" frequency="22000" gain="2.0" />
     </effects>
   </DecentSampler>

Attributes:

+-----+---+------------+------------------------------------------+---+
| Att |   | Type       | Valid Range                              | D |
| rib |   |            |                                          | e |
| ute |   |            |                                          | f |
|     |   |            |                                          | a |
|     |   |            |                                          | u |
|     |   |            |                                          | l |
|     |   |            |                                          | t |
+=====+===+============+==========================================+===+
| ``  | R | The type   | Must be ``peak``                         |   |
| typ | e | of filter  |                                          |   |
| e`` | q |            |                                          |   |
|     | u |            |                                          |   |
|     | i |            |                                          |   |
|     | r |            |                                          |   |
|     | e |            |                                          |   |
|     | d |            |                                          |   |
+-----+---+------------+------------------------------------------+---+
| `   | R | The filter | 60 - 22000.0                             | 1 |
| `fr | e | frequency  |                                          | 0 |
| equ | q |            |                                          | 0 |
| enc | u |            |                                          | 0 |
| y`` | i |            |                                          | 0 |
|     | r |            |                                          |   |
|     | e |            |                                          |   |
|     | d |            |                                          |   |
+-----+---+------------+------------------------------------------+---+
| ``  | O | Q is the   | 0.01 - 18.0                              | 0 |
| Q`` | p | ratio of   |                                          | . |
|     | t | center     |                                          | 7 |
|     | i | frequency  |                                          |   |
|     | o | to         |                                          |   |
|     | n | bandwidth  |                                          |   |
|     | a |            |                                          |   |
|     | l |            |                                          |   |
+-----+---+------------+------------------------------------------+---+
| ``  | R | Values     | 0 - 1.0                                  | 1 |
| gai | e | greater    |                                          | . |
| n`` | q | than 1.0   |                                          | 0 |
|     | u | will boost |                                          |   |
|     | i | the high   |                                          |   |
|     | r | fr         |                                          |   |
|     | e | equencies, |                                          |   |
|     | d | values     |                                          |   |
|     |   | less than  |                                          |   |
|     |   | 1.0 will   |                                          |   |
|     |   | attenuate  |                                          |   |
|     |   | them.      |                                          |   |
+-----+---+------------+------------------------------------------+---+

Gain effect
'''''''''''

Applies a volume boost or cut to the output signal.

Example:

.. code:: xml

   <DecentSampler>
     <effects>
       <effect type="gain" level="-6" />
     </effects>
   </DecentSampler>

Attributes:

+---+---+-----------------------------------------------------+---+----+
| A |   | Type                                                | D | V  |
| t |   |                                                     | e | al |
| t |   |                                                     | f | id |
| r |   |                                                     | a | R  |
| i |   |                                                     | u | an |
| b |   |                                                     | l | ge |
| u |   |                                                     | t |    |
| t |   |                                                     |   |    |
| e |   |                                                     |   |    |
+===+===+=====================================================+===+====+
| ` | R | Must be ``gain``                                    |   | `` |
| ` | e |                                                     |   | ga |
| t | q |                                                     |   | in |
| y | u |                                                     |   | `` |
| p | i |                                                     |   |    |
| e | r |                                                     |   |    |
| ` | e |                                                     |   |    |
| ` | d |                                                     |   |    |
+---+---+-----------------------------------------------------+---+----+
| ` | R | The amount of gain to be applied expressed in       | 0 | -  |
| ` | e | decibels. In other words, gain of -6dB reduces      |   | 99 |
| l | q | sound by 50%                                        |   | -  |
| e | u |                                                     |   | 24 |
| v | i |                                                     |   |    |
| e | r |                                                     |   |    |
| l | e |                                                     |   |    |
| ` | d |                                                     |   |    |
| ` |   |                                                     |   |    |
+---+---+-----------------------------------------------------+---+----+

Reverb effect
'''''''''''''

Example:

.. code:: xml

   <DecentSampler>
     <effects>
       <effect type="reverb" roomSize="" damping="" wetLevel="" />
     </effects>
   </DecentSampler>

Attributes:

+-----+----+-----------------+-----------------------------------+---+
| Att |    | Type            | Valid Range                       | D |
| rib |    |                 |                                   | e |
| ute |    |                 |                                   | f |
|     |    |                 |                                   | a |
|     |    |                 |                                   | u |
|     |    |                 |                                   | l |
|     |    |                 |                                   | t |
+=====+====+=================+===================================+===+
| ``  | Re | Must be         | ``reverb``                        |   |
| typ | qu | ``reverb``      |                                   |   |
| e`` | ir |                 |                                   |   |
|     | ed |                 |                                   |   |
+-----+----+-----------------+-----------------------------------+---+
| ``r | Op | The reverb      | 0 - 1.0, where 1.0 is big, 0 is   | 0 |
| oom | ti | “room size”     | small.                            | . |
| Siz | on |                 |                                   | 7 |
| e`` | al |                 |                                   |   |
+-----+----+-----------------+-----------------------------------+---+
| ``  | Op | The reverb      | 0 - 1.0, where 0 is not damped,   | 0 |
| dam | ti | damping level   | 1.0 is fully damped.              | . |
| pin | on |                 |                                   | 3 |
| g`` | al |                 |                                   |   |
+-----+----+-----------------+-----------------------------------+---+
| ``w | Op | The volume of   | 0 - 1.0                           | 0 |
| etL | ti | reverb signal   |                                   |   |
| eve | on |                 |                                   |   |
| l`` | al |                 |                                   |   |
+-----+----+-----------------+-----------------------------------+---+

Delay effect
''''''''''''

A simple delay effect that can be controlled either in seconds or using
musical time increments based on the host tempo. For a complete
explanation of how to use tempo-syncing, see
`here <https://www.decentsamples.com/2024/02/08/for-sample-creators-how-to-add-tempo-synced-delay-to-your-instruments/>`__.

**Attributes:**

+---+---+----------------------------------------------------------+-----+---+
| A |   | Type                                                     | Va  | D |
| t |   |                                                          | lid | e |
| t |   |                                                          | Ra  | f |
| r |   |                                                          | nge | a |
| i |   |                                                          |     | u |
| b |   |                                                          |     | l |
| u |   |                                                          |     | t |
| t |   |                                                          |     |   |
| e |   |                                                          |     |   |
+===+===+==========================================================+=====+===+
| ` | R | Must be ``delay``                                        | ``d |   |
| ` | e |                                                          | ela |   |
| t | q |                                                          | y`` |   |
| y | u |                                                          |     |   |
| p | i |                                                          |     |   |
| e | r |                                                          |     |   |
| ` | e |                                                          |     |   |
| ` | d |                                                          |     |   |
+---+---+----------------------------------------------------------+-----+---+
| ` | O | Determines whether the delay will be synced to DAW tempo | ``s | ` |
| ` | p | or not, as well as what format you will be using for the | eco | ` |
| d | t | ``delayTime`` parameter. There are two possible values:  | nds | s |
| e | i | 1) ``seconds``, which is the default, means that         | ``, | e |
| l | o | ``delayTime`` will be specified in seconds and will not  | `   | c |
| a | n | change when the DAW tempo changes; 2) ``musical_time``   | `mu | o |
| y | a | means that delay time will be specified using an integer | sic | n |
| T | l | value generated by a control which is setup to use the   | al_ | d |
| i |   | ``musical_time`` ``valueType`` parameter. In order for   | tim | s |
| m |   | this to work, you will need to be using the plug-in      | e`` | ` |
| e |   | within a DAW that provides a tempo to the plug-in.       |     | ` |
| F |   |                                                          |     |   |
| o |   |                                                          |     |   |
| r |   |                                                          |     |   |
| m |   |                                                          |     |   |
| a |   |                                                          |     |   |
| t |   |                                                          |     |   |
| ` |   |                                                          |     |   |
| ` |   |                                                          |     |   |
+---+---+----------------------------------------------------------+-----+---+
| ` | O | The delay time in seconds                                | 0 - | 0 |
| ` | p |                                                          | 2   | . |
| d | t |                                                          | 0.0 | 7 |
| e | i |                                                          |     |   |
| l | o |                                                          |     |   |
| a | n |                                                          |     |   |
| y | a |                                                          |     |   |
| T | l |                                                          |     |   |
| i |   |                                                          |     |   |
| m |   |                                                          |     |   |
| e |   |                                                          |     |   |
| ` |   |                                                          |     |   |
| ` |   |                                                          |     |   |
+---+---+----------------------------------------------------------+-----+---+
| ` | O | The feedback level.                                      | 0 - | 0 |
| ` | p |                                                          | 1   | . |
| f | t |                                                          | .0, | 2 |
| e | i |                                                          | wh  |   |
| e | o |                                                          | ere |   |
| d | n |                                                          | 0   |   |
| b | a |                                                          | is  |   |
| a | l |                                                          | no  |   |
| c |   |                                                          | fee |   |
| k |   |                                                          | dba |   |
| ` |   |                                                          | ck, |   |
| ` |   |                                                          | 1.0 |   |
|   |   |                                                          | is  |   |
|   |   |                                                          | max |   |
|   |   |                                                          | fee |   |
|   |   |                                                          | dba |   |
|   |   |                                                          | ck. |   |
+---+---+----------------------------------------------------------+-----+---+
| ` | O | The parameter allows you to introduce delay variations   | -10 | 0 |
| ` | p | between the left and right channels. Half of this amount | -   |   |
| s | t | is subtracted from the left channel’s delay time and     | 10  |   |
| t | i | half of this amount is added to the right channel’s      |     |   |
| e | o | delay time. For example, if the ``delayTime`` is 0.5     |     |   |
| r | n | seconds and the ``stereoOffset`` is 0.02 s, then the     |     |   |
| e | a | actual left channel delay time will be 0.49s and the     |     |   |
| o | l | actual right channel delay time will be 0.51s so that    |     |   |
| O |   | the two channels are offset by 0.02 seconds.             |     |   |
| f |   |                                                          |     |   |
| f |   |                                                          |     |   |
| s |   |                                                          |     |   |
| e |   |                                                          |     |   |
| t |   |                                                          |     |   |
| ` |   |                                                          |     |   |
| ` |   |                                                          |     |   |
+---+---+----------------------------------------------------------+-----+---+
| ` | O | The volume of the delay signal                           | 0 - | 0 |
| ` | p |                                                          | 1.0 | . |
| w | t |                                                          |     | 5 |
| e | i |                                                          |     |   |
| t | o |                                                          |     |   |
| L | n |                                                          |     |   |
| e | a |                                                          |     |   |
| v | l |                                                          |     |   |
| e |   |                                                          |     |   |
| l |   |                                                          |     |   |
| ` |   |                                                          |     |   |
| ` |   |                                                          |     |   |
+---+---+----------------------------------------------------------+-----+---+

Example of using the delay effect when specifying time in seconds:

.. code:: xml

   <DecentSampler>
     <effects>
       <effect type="delay" delayTime="0.5" stereoOffset="0.01" feedback="0.2" wetLevel="0.5" />
     </effects>
   </DecentSampler>

Example of using the delay effect when specifying time in musical time:

.. code:: xml

   <DecentSampler>
     <ui>
       <tab>
         <labeled-knob x="180" y="40" label="Delay Time" valueType="musical_time" 
                       minValue="0" maxValue="20" value="10" defaultValue="10">
           <binding type="effect" level="instrument" position="0" parameter="FX_DELAY_TIME" />
         </labeled-knob>
       </tab>
     </ui>
     <effects>
       <effect type="delay" delayTimeFormat="musical_time" delayTime="10" stereoOffset="0.01" 
               feedback="0.3" wetLevel="0.5" />
     </effects>
   </DecentSampler>

Chorus effect
'''''''''''''

Example:

.. code:: xml

   <DecentSampler>
     <effects>
       <effect type="chorus" mix="0.5" modDepth="0.2" modRate="0.2"/>
     </effects>
   </DecentSampler>

Attributes:

+---------+-------+----------------------------+-------------+-------+
| At      |       | Type                       | Valid Range | De    |
| tribute |       |                            |             | fault |
+=========+=======+============================+=============+=======+
| `       | Req   | Must be ``chorus``         | ``chorus``  |       |
| `type`` | uired |                            |             |       |
+---------+-------+----------------------------+-------------+-------+
| ``mix`` | Opt   | The wet/dry mix which      | 0 - 1.0,    | 0.5   |
|         | ional | controls how much of the   | where 1.0   |       |
|         |       | chorus signal we hear      | is just     |       |
|         |       |                            | chorus, 0   |       |
|         |       |                            | is just dry |       |
|         |       |                            | signal.     |       |
+---------+-------+----------------------------+-------------+-------+
| ``mod   | Opt   | The modulation depth of    | 0 - 1.0,    | 0.2   |
| Depth`` | ional | the effect                 | where 0 is  |       |
|         |       |                            | no          |       |
|         |       |                            | modulation, |       |
|         |       |                            | 1.0 is max  |       |
|         |       |                            | modulation. |       |
+---------+-------+----------------------------+-------------+-------+
| ``mo    | Opt   | The modulation speed in    | 0 - 10.0    | 0.2   |
| dRate`` | ional | Hz.                        |             |       |
+---------+-------+----------------------------+-------------+-------+

Phaser effect
'''''''''''''

Example:

.. code:: xml

   <DecentSampler>
     <effects>
       <effect type="phaser" mix="0.5" modDepth="0.2" modRate="0.2" centerFrequency="400" feedback="0.7" />
     </effects>
   </DecentSampler>

Attributes:

+---------------+------+-------------------------+-----------+------+
| Attribute     |      | Type                    | Valid     | Def  |
|               |      |                         | Range     | ault |
+===============+======+=========================+===========+======+
| ``type``      | Requ | Must be ``phaser``      | `         |      |
|               | ired |                         | `phaser`` |      |
+---------------+------+-------------------------+-----------+------+
| ``mix``       | Opti | The wet/dry mix which   | 0 - 1.0,  | 0.5  |
|               | onal | controls how much of    | where 1.0 |      |
|               |      | the chorus signal we    | is just   |      |
|               |      | hear                    | chorus, 0 |      |
|               |      |                         | is just   |      |
|               |      |                         | dry       |      |
|               |      |                         | signal.   |      |
+---------------+------+-------------------------+-----------+------+
| ``modDepth``  | Opti | The modulation depth of | 0 - 1.0,  | 0.2  |
|               | onal | the effect              | where 0   |      |
|               |      |                         | is no     |      |
|               |      |                         | mo        |      |
|               |      |                         | dulation, |      |
|               |      |                         | 1.0 is    |      |
|               |      |                         | max       |      |
|               |      |                         | mo        |      |
|               |      |                         | dulation. |      |
+---------------+------+-------------------------+-----------+------+
| ``modRate``   | Opti | The modulation speed in | 0 - 10.0  | 0.2  |
|               | onal | Hz.                     |           |      |
+---------------+------+-------------------------+-----------+------+
| ``cent        | Opti | The center frequency    | 0 - 1.0   | 400  |
| erFrequency`` | onal | (in Hz) of the phaser   |           |      |
|               |      | all-pass filters        |           |      |
|               |      | modulation              |           |      |
+---------------+------+-------------------------+-----------+------+
| ``feedback``  | Opti | Sets the feedback       | -1 - 1.0  | 0.7  |
|               | onal | volume of the phaser.   |           |      |
+---------------+------+-------------------------+-----------+------+

Convolution effect
''''''''''''''''''

This effect allows you to use a convolution reverb or amp simulation to
your sample library. Depending on the length of the impulse response,
the convolution effect can use substantial CPU, so you’ll definitely
want to do some testing both with and without the convolution effect
turned on.

Example:

.. code:: xml

   <DecentSampler>
     <effects>
       <effect type="convolution" mix="0.5" irFile="Samples/Hall 3.wav" />
     </effects>
   </DecentSampler>

Attributes:

+---+---+-----------------------------+--------------------------+----+
| A |   | Type                        | Valid Range              | D  |
| t |   |                             |                          | ef |
| t |   |                             |                          | au |
| r |   |                             |                          | lt |
| i |   |                             |                          |    |
| b |   |                             |                          |    |
| u |   |                             |                          |    |
| t |   |                             |                          |    |
| e |   |                             |                          |    |
+===+===+=============================+==========================+====+
| ` | R | Must be ``convolution``     | ``convolution``          |    |
| ` | e |                             |                          |    |
| t | q |                             |                          |    |
| y | u |                             |                          |    |
| p | i |                             |                          |    |
| e | r |                             |                          |    |
| ` | e |                             |                          |    |
| ` | d |                             |                          |    |
+---+---+-----------------------------+--------------------------+----+
| ` | O | The wet/dry mix controls    | 0 - 1.0, where 1.0 is    | 0  |
| ` | p | how much of the convolution | just convolution, 0 is   | .5 |
| m | t | signal we hear              | just dry signal.         |    |
| i | i |                             |                          |    |
| x | o |                             |                          |    |
| ` | n |                             |                          |    |
| ` | a |                             |                          |    |
|   | l |                             |                          |    |
+---+---+-----------------------------+--------------------------+----+
| ` | R | The path of the WAV or AIFF | String                   | No |
| ` | e | to use as an Impulse        |                          | d  |
| i | q | Response (IR) file          |                          | ef |
| r | u |                             |                          | au |
| F | i |                             |                          | lt |
| i | r |                             |                          |    |
| l | e |                             |                          |    |
| e | d |                             |                          |    |
| ` |   |                             |                          |    |
| ` |   |                             |                          |    |
+---+---+-----------------------------+--------------------------+----+

Wave Folder effect
''''''''''''''''''

Introduced in version 1.7.2. This effect allows you to fold a waveform
back on itself. This is very useful for generating additional harmonic
content.

Attributes:

+---+---+-------------------+--------------------------------------+---+
| A |   | Type              | Valid Range                          | D |
| t |   |                   |                                      | e |
| t |   |                   |                                      | f |
| r |   |                   |                                      | a |
| i |   |                   |                                      | u |
| b |   |                   |                                      | l |
| u |   |                   |                                      | t |
| t |   |                   |                                      |   |
| e |   |                   |                                      |   |
+===+===+===================+======================================+===+
| ` | R | Must be           | ``wave_folder``                      |   |
| ` | e | ``wave_folder``   |                                      |   |
| t | q |                   |                                      |   |
| y | u |                   |                                      |   |
| p | i |                   |                                      |   |
| e | r |                   |                                      |   |
| ` | e |                   |                                      |   |
| ` | d |                   |                                      |   |
+---+---+-------------------+--------------------------------------+---+
| ` | O | The volume of the | 1 - 100, where 100 means the signal  | 1 |
| ` | p | input signal      | is amplified by a factor of 100 and  |   |
| d | t |                   | 1 means no amplification is applied  |   |
| r | i |                   |                                      |   |
| i | o |                   |                                      |   |
| v | n |                   |                                      |   |
| e | a |                   |                                      |   |
| ` | l |                   |                                      |   |
| ` |   |                   |                                      |   |
+---+---+-------------------+--------------------------------------+---+
| ` | O | The amplitude     | 0 - 10.0                             | 0 |
| ` | p | above which wave  |                                      | . |
| t | t | folding should    |                                      | 2 |
| h | i | take place        |                                      | 5 |
| r | o |                   |                                      |   |
| e | n |                   |                                      |   |
| s | a |                   |                                      |   |
| h | l |                   |                                      |   |
| o |   |                   |                                      |   |
| l |   |                   |                                      |   |
| d |   |                   |                                      |   |
| ` |   |                   |                                      |   |
| ` |   |                   |                                      |   |
+---+---+-------------------+--------------------------------------+---+

Because wave folding tends to sound better when applied on a per-voice
basis, it usually makes sense to set up the wave folder at the group
level (separate group effects get created for each keypress). Example:

.. code:: xml

   <DecentSampler pluginVersion="1">
     <ui>
       <tab>
         <labeled-knob x="180" y="40" label="Drive" type="float" minValue="1" maxValue="100" textColor="FF000000" value="1">
           <binding type="effect" level="group" groupIndex="0" effectIndex="0" parameter="FX_DRIVE" translation="linear" />
         </labeled-knob>
         <labeled-knob x="280" y="40" label="Threshold" type="float" minValue="0" maxValue="1" value="1" textColor="FF000000">
           <binding type="effect" level="group" groupIndex="0" effectIndex="0" parameter="FX_THRESHOLD" translation="linear" />
         </labeled-knob>
       </tab>
     </ui>
     <groups>
       <group>
           <!-- Samples go here. -->
       <effects>
           <effect type="wave_folder" drive="1" threshold="1" />
         </effects>
       </group>
     </groups>

Wave Shaper effect
''''''''''''''''''

Introduced in version 1.7.2. This effect allows you to distort an audio
signal. This is very useful for generating additional harmonic content.

Attributes:

+---+---+----------------------------------+---------------------------+---+
| A |   | Type                             | Valid Range               | D |
| t |   |                                  |                           | e |
| t |   |                                  |                           | f |
| r |   |                                  |                           | a |
| i |   |                                  |                           | u |
| b |   |                                  |                           | l |
| u |   |                                  |                           | t |
| t |   |                                  |                           |   |
| e |   |                                  |                           |   |
+===+===+==================================+===========================+===+
| ` | R | Must be ``wave_shaper``          | ``wave_folder``           |   |
| ` | e |                                  |                           |   |
| t | q |                                  |                           |   |
| y | u |                                  |                           |   |
| p | i |                                  |                           |   |
| e | r |                                  |                           |   |
| ` | e |                                  |                           |   |
| ` | d |                                  |                           |   |
+---+---+----------------------------------+---------------------------+---+
| ` | O | The amount of distortion. This   | 1 - 1000, where 1000      | 1 |
| ` | p | really just controls the volume  | means the signal is       |   |
| d | t | of the input signal. The volume  | amplified by a factor of  |   |
| r | i | of the input signal              | 1000 and 1 means no       |   |
| i | o |                                  | amplification is applied  |   |
| v | n |                                  |                           |   |
| e | a |                                  |                           |   |
| ` | l |                                  |                           |   |
| ` |   |                                  |                           |   |
+---+---+----------------------------------+---------------------------+---+
| ` | O | Introduces an extra gain boost   | 0 - 1.0                   | 1 |
| ` | p | to the drive                     |                           |   |
| d | t |                                  |                           |   |
| r | i |                                  |                           |   |
| i | o |                                  |                           |   |
| v | n |                                  |                           |   |
| e | a |                                  |                           |   |
| B | l |                                  |                           |   |
| o |   |                                  |                           |   |
| o |   |                                  |                           |   |
| s |   |                                  |                           |   |
| t |   |                                  |                           |   |
| ` |   |                                  |                           |   |
| ` |   |                                  |                           |   |
+---+---+----------------------------------+---------------------------+---+
| ` | O | The linear output level of the   | 0 - 1.0                   | 0 |
| ` | p | signal                           |                           | . |
| o | t |                                  |                           | 1 |
| u | i |                                  |                           |   |
| t | o |                                  |                           |   |
| p | n |                                  |                           |   |
| u | a |                                  |                           |   |
| t | l |                                  |                           |   |
| L |   |                                  |                           |   |
| e |   |                                  |                           |   |
| v |   |                                  |                           |   |
| e |   |                                  |                           |   |
| l |   |                                  |                           |   |
| ` |   |                                  |                           |   |
| ` |   |                                  |                           |   |
+---+---+----------------------------------+---------------------------+---+
| ` | O | Whether or not oversampling is   | true, false               | t |
| ` | p | performed. Oversampling sounds   |                           | r |
| h | t | better, but it’s CPU intensive.  |                           | u |
| i | i | If you want to save CPU, set     |                           | e |
| g | o | this to false.                   |                           |   |
| h | n |                                  |                           |   |
| Q | a |                                  |                           |   |
| u | l |                                  |                           |   |
| a |   |                                  |                           |   |
| l |   |                                  |                           |   |
| i |   |                                  |                           |   |
| t |   |                                  |                           |   |
| y |   |                                  |                           |   |
| ` |   |                                  |                           |   |
| ` |   |                                  |                           |   |
+---+---+----------------------------------+---------------------------+---+

Because wave shaping tends to sound better when applied on a per-voice
basis, it usually makes sense to set up the wave shaper at the group
level (separate group effects get created for each keypress). Example:

.. code:: xml

   <DecentSampler pluginVersion="1">
     <ui>
       <tab>
         <labeled-knob x="180" y="40" label="Drive" type="float" minValue="1" maxValue="40" textColor="FF000000" value="0.5473124980926514">
           <binding type="effect" level="group" groupIndex="0" effectIndex="0" parameter="FX_DRIVE" translation="linear"/>
         </labeled-knob>
         <labeled-knob x="280" y="40" label="Drive Boost" type="float" minValue="0" maxValue="1" value="0.328312486410141" textColor="FF000000">
           <binding type="effect" level="group" groupIndex="0" effectIndex="0" parameter="FX_DRIVE_BOOST" translation="linear"/>
         </labeled-knob>
         <labeled-knob x="380" y="40" label="Output Lvl" type="float" minValue="0" maxValue="1" value="0.328312486410141" textColor="FF000000">
           <binding type="effect" level="group" groupIndex="0" effectIndex="0" parameter="FX_OUTPUT_LEVEL" translation="linear"/>
         </labeled-knob>
       </tab>
     </ui>
     <groups>
       <group>
           <!-- Samples go here. -->
         <effects>
           <effect type="wave_shaper" drive="0.5473124980926514" shape="0.328312486410141" outputLevel="0.1"/>
         </effects>
       </group>
     </groups>

The <midi> element
~~~~~~~~~~~~~~~~~~

MIDI mappings can be added to your instrument by adding a ``<midi>``
element right below your top-level ``<DecentSampler>`` element.

The <cc> element
^^^^^^^^^^^^^^^^

Within the ``<midi>`` element, you can have any number of ``<cc>``
elements. These allow you to map changes in incoming continuous
controller messages to specific parameters of your instrument. To use
this functionality, you’ll want to add a separate ``<cc>`` element for
each CC number you would like to respond to. The ``<cc>`` element has a
single required attribute ``number=""`` which specifies the number (from
0 to 127) of the continuous controller you would like to listen on.
Beneath the ``<cc>`` element, you can have any number of bindings.

.. code:: xml

   <midi>
     <cc number="11">
       <binding level="ui" type="control" position="0" parameter="VALUE" translation="linear" 
                translationOutputMin="0" translationOutputMax="1"/>
     </cc>
     <cc number="1">
       <binding level="ui" type="control" position="1" parameter="VALUE" translation="linear" 
                translationOutputMin="0" translationOutputMax="1"/>
     </cc>
   </midi>

The <note> element
^^^^^^^^^^^^^^^^^^

Within the ``<midi>`` element, you can have any number of ``<note>``
elements. These allow you to map specific notes to specific parameters
of your instrument. To use this functionality, you’ll want to add a
separate ``<note>`` element for each MIDI note you would like to respond
to. The ``<note>`` element has a single required attribute ``note=""``
which specifies the number (from 0 to 127) of the MIDI note you would
like to listen on. Beneath the ``<note>`` element, you can have any
number of bindings. Here is an example of how keyswitches could be set
up:

.. code:: xml

   <midi>
     <note note="11">
       <binding type="general" level="group" position="0" parameter="ENABLED" translation="fixed_value" translationValue="true" />
       <binding type="general" level="group" position="1" parameter="ENABLED" translation="fixed_value" translationValue="false" />
     </note>
     <note note="12">
       <binding type="general" level="group" position="0" parameter="ENABLED" translation="fixed_value" translationValue="false" />
       <binding type="general" level="group" position="1" parameter="ENABLED" translation="fixed_value" translationValue="true" />
     </note>
   </midi>

In the above keyswitch example, MIDI note 11 turns on group 0 and turns
off group 1, whereas MIDI note 12 does the opposite. Note the use of the
``fixed_value`` translation type.

Bindings within the ``<midi>`` section
''''''''''''''''''''''''''''''''''''''

The bindings that the ``<cc>`` and ``<note>`` element listens on are the
same as those used by the UI controls. See `Appendix
B <#appendix-b-the-binding-element>`__ for a complete description of
these.

If you have a UI control mapped to the same internal parameter as a MIDI
mapping, you’ll want to have your MIDI mapping control the UI control
instead of the parameter directly. The benefit of doing this is that, as
the MIDI CC input is received, the UI control will be updated as well as
the desired internal parameter.

The way to accomplish this is to make use of the ``labeled_knob`` or
``control`` binding types (``control`` was introduced in version 1.1.7)
as follows:

.. code:: xml

   <binding level="ui" type="control" position="0" parameter="VALUE" translation="linear" translationOutputMin="0" translationOutputMax="1"/>

You’ll notice that the ``control`` type has a ``level`` value of ``ui``
and a ``parameter`` value of ``VALUE``. Another thing to notice is the
``position=""`` parameter. This contains the 0-based index of the
control to be modified. **NOTE: The indexes of the parameter list
includes all UI controls, including ``<label>`` and ``menu`` controls,
so you’ll want to account for that when calculating your positions.**

An example of changing a menu option based on a MIDI note (keyswitch)
would look like this:

.. code:: xml

   <midi>
     <note note="11">
       <binding type="control" level="ui" position="1" parameter="VALUE" translation="fixed_value" translationValue="1" />
     </note>
     <note note="12">
       <binding type="control" level="ui" position="1" parameter="VALUE" translation="fixed_value" translationValue="2" />
     </note>
   </midi>

The <modulators> element
~~~~~~~~~~~~~~~~~~~~~~~~

Version 1.6.0 of Decent Sampler officially introduces the new
``<modulators>`` section into the ``.dspreset`` format. This section
lives below the top-level ``<DecentSampler>`` element and it is where
all modulators for the entire sample library live.

.. code:: xml

   <DecentSampler>
       <modulators>
           <!-- Your modulators go here. -->
       </modulators>
   </DecentSampler>

The <lfo> element
^^^^^^^^^^^^^^^^^

Underneath the ``<modulators>`` section, you can have any number of
different LFOs, which are defined using an ``<lfo>`` element, for
example:

.. code:: xml

   <modulators>1
     <lfo shape="sine" frequency="2" modAmount="1.0"></lfo>
   </modulators>

This element has the following attributes:

-  **``shape``**: controls the oscillator shape. Possible values are
   ``sine``, ``square``, ``saw``.
-  **``frequency``**: The speed of the LFO in cycles per second. For
   example, a value of 10 would mean that the waveform repeats ten times
   per second.
-  **``modAmount``**: This value between 0 and 1 controls how much the
   modulation affects the things it is targeting. In conventional terms,
   this is like the modulation depth. Default value: 1.0.
-  **``scope``**: Whether or not this LFO exists for all notes or
   whether each keypress gets its own LFO. Possible values are
   ``global`` (default for LFOs) and ``voice``. If ``voice`` is chosen,
   a new LFO is started each time a new note is pressed.

The <envelope> element
^^^^^^^^^^^^^^^^^^^^^^

In addition to LFOs, you can also have additional ADSR envelopes. These
can be useful for controlling group-level effects, such as low-pass
filters. If this is what you wish to achieve, make sure you check out
the section on group-level effects below.

To create an envelope, use an ``<envelope>`` element:

This element has the following attributes: - **``attack``**: The length
in seconds of the attack portion of the ADSR envelope - **``decay``**:
The length in seconds of the decay portion of the ADSR envelope -
**``sustain``**: The height of the sustain portion of the ADSR envelope.
This is expressed as a value between 0 and 1. - **``release``**: The
length in seconds of the release portion of the ADSR envelope -
**``modAmount``**: This value between 0 and 1 controls how much the
modulation affects the things it is targeting. In conventional terms,
this is like the modulation depth. Default value: 1.0. - **``scope``**:
Whether or not this LFO exists for all notes or whether each keypress
gets its own LFO. Possible values are ``global`` and ``voice`` (default
for envelopes). If ``voice`` is chosen, a new LFO is started each time a
new note is pressed.

How to use <binding>s in conjunction with modulators
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In order to actually have your LFOs and envelopes do anything, you need
to have bindings under them. If you are not familiar with the concept of
bindings, you may want to read `this
section <https://www.decentsamples.com/wp-content/uploads/2020/06/format-documentation.html#appendix-b-the-binding-element>`__
then return here. Bindings tell the engine which parameters the LFO
should be affecting and how. Here is an example:

.. code:: xml

   <modulators>
       <lfo shape="sine" frequency="2" modAmount="1.0">
           <!-- This binding modifies the frequency of a low-pass filter  -->
           <binding type="effect" level="instrument" effectIndex="0" parameter="FX_FILTER_FREQUENCY" modBehavior="add" translation="linear" translationOutputMin="0" translationOutputMax="2000.0"  />
       </lfo>
   </modulators>

There are a few differences between bindings as they are used by knobs
and the ones used by modulators. Specifically, when you move a UI
control that has a binding attached, the engine actually goes out and
changes the value of the parameter that is targeted by that binding. For
example, if you have a knob that controls a lowpass filter’s cutoff
frequency, moving that knob will cause that actual frequency of that
filter to change. In other words, the changes that the knob is making on
the underlying sample library are *permanent*. The same is also true for
bindings associated with MIDI continuous controllers.

Modulators, on the other hand, do not work this way. If a modulator
(such as an LFO) changes its value, the engine looks at the bindings
associated with that LFO and then makes a list of *temporary* changes to
the underlying data. When it comes time to render out the effect, it
consults both the *permanent* value and the *temporary* modulation
values. As a result of this difference in the way bindings are handled,
only some parameters are “modulatable.” At time of press, the following
parameters are modulatable:

-  All gain effect parameters
-  All delay effect parameters
-  All phaser effect parameters
-  All filter effect parameters
-  All reverb effect parameters
-  All chorus effect parameters
-  Group Volume
-  Global Volume
-  Group Pan
-  Global Pan
-  Group Tuning
-  Global Tuning

Modulator scope: global or voice-level
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

By default, all modulators will be created at the global level. This
means that there will be exactly one modulator that is shared by all
voices. In many situations, such as an LFO modulating a single low-pass
filter which is shared by all of voices, this is often what we want.

But there are other situations where we don’t want our modulator to be
global. In such cases we

.. code:: xml

   <modulators>
       <envelope attack="2" decay="0" sustain="1" release="0.5" modAmount="1.0">
           <binding type="effect" level="group" groupIndex="0" effectIndex="0" parameter="FX_FILTER_FREQUENCY" translation="linear" translationOutputMin="0" translationOutputMax="4000.0" modBehavior="add" />
       </envelope>
   </modulators>

Note that this voice-level modulator is now targeting a group level
effect.

The <tags> element
~~~~~~~~~~~~~~~~~~

The ``<tags>`` element lives right below your top-level
``<DecentSampler>`` element. It allows you to specify details about the
tags you use throughout your instrument. It is however not actually
necessary to include a ``<tags>`` element for every tag you use. You
only need to create this if you want to specify additional details about
your tags.

The <tag> element
^^^^^^^^^^^^^^^^^

Underneath the ``<tags>`` element, you can have any number of ``<tag>``
elements. These specify details for each individual tag that you use
throughout your sample mapping. Attributes:

+---------+------+----------------------------------------------------+
| At      |      | Description                                        |
| tribute |      |                                                    |
+=========+======+====================================================+
| *       | (o   | A number for 0.0 to 1.0 that specifies the initial |
| *``enab | ptio | volume for a tag. Default: 1.0                     |
| led``** | nal) |                                                    |
+---------+------+----------------------------------------------------+
| **``vol | (o   | A number for 0.0 to 1.0 that specifies the initial |
| ume``** | ptio | volume for a tag. Default: 1.0                     |
|         | nal) |                                                    |
+---------+------+----------------------------------------------------+
| **`     | (o   | A whole number that specifies the number of voices |
| `polyph | ptio | allowed for this tag.                              |
| ony``** | nal) |                                                    |
+---------+------+----------------------------------------------------+

--------------

Appendix A: The Color Format
----------------------------

Colors are represented throughout the **dspreset** files using an
8-digit ARGB color format. These are identical to web color hex codes
except with an additional 2-digit hex number in front of them. The first
two digits are a hexadecimal representation of alpha level with 00 being
fully transparent, 80 being 50% transparent, and FF being fully opaque.

Examples:

-  Black (solid): FF000000
-  Black (90% transparency): E6000000
-  Red (solid): FFFF0000
-  Red (50% transparency): 80FF0000
-  Blue (solid): FF0000FF

Appendix B: The <binding> element
---------------------------------

Adding a binding to a UI control, MIDI handler, or modulator tells the
DecentSampler engine that it should take input from a source and use it
to change values in another part of the engine. An example of this would
be a knob which controls the volume of a group, a CC controller that
changes an effect parameter, or an LFO that modulates an effect
parameter.

In order to set up a binding for a specific source, create a
``<binding>`` element within the source element.

In this example, a labeled knob is controlling the volume of the first
group of samples (group 0):

.. code:: xml

   <DecentSampler>
     <ui>
       <tab>
         <labeled-knob x="420" y="100" label="RT" type="float" minValue="0" maxValue="1" value="0.3" textSize="20">
           <binding type="amp" level="group" position="0" parameter="AMP_VOLUME" translation="linear" translationOutputMin="0" translationOutputMax="1.0"  />
         </labeled-knob>
       </tab>
     </ui>
   </DecentSampler>

Here’s a full list of parameters for the ``<binding>`` element:

+----+---------------------------------------------------------------+---+
| A  | Description                                                   |   |
| tt |                                                               |   |
| ri |                                                               |   |
| bu |                                                               |   |
| te |                                                               |   |
+====+===============================================================+===+
| ** | This tells the engine what type of parameter this is. Valid   | R |
| `` | values are: “amp”, “effect”, “control”.                       | e |
| ty |                                                               | q |
| pe |                                                               | u |
| `` |                                                               | i |
| ** |                                                               | r |
|    |                                                               | e |
|    |                                                               | d |
+----+---------------------------------------------------------------+---+
| *  | Valid values are ``ui``, ``instrument``, ``group``            | R |
| *` |                                                               | e |
| `l |                                                               | q |
| ev |                                                               | u |
| el |                                                               | i |
| `` |                                                               | r |
| ** |                                                               | e |
|    |                                                               | d |
+----+---------------------------------------------------------------+---+
| ** | The specific 0-based index of the element to be modified by   | R |
| `` | this binding. If you are targeting a group, for example, the  | e |
| po | first group would be 0, the second group would be 1, etc.     | q |
| si |                                                               | u |
| ti |                                                               | i |
| on |                                                               | r |
| `` |                                                               | e |
| ** |                                                               | d |
+----+---------------------------------------------------------------+---+
| ** | When a binding is targeting a control, this is the same thing | O |
| `` | as the ``position`` attribute. It is a specific 0-based index | p |
| co | of the control to be modified by this binding. If you are     | t |
| nt | targeting an group-level effect, this would specified the     | i |
| ro | group under which the effect lives.                           | o |
| lI |                                                               | n |
| nd |                                                               | a |
| ex |                                                               | l |
| `` |                                                               |   |
| ** |                                                               |   |
+----+---------------------------------------------------------------+---+
| ** | When a binding is targeting a group, this is the same thing   | O |
| `` | as the ``position`` attribute. It is a specific 0-based index | p |
| gr | of the group to be modified by this binding. If you are       | t |
| ou | targeting an group-level effect, this would specified the     | i |
| pI | group under which the effect lives.                           | o |
| nd |                                                               | n |
| ex |                                                               | a |
| `` |                                                               | l |
| ** |                                                               |   |
+----+---------------------------------------------------------------+---+
| *  | When a binding is targeting an effect, this is the same thing | O |
| *` | as the ``position`` attribute. It is a specific 0-based index | p |
| `e | of the effect to be modified by this binding.                 | t |
| ff |                                                               | i |
| ec |                                                               | o |
| tI |                                                               | n |
| nd |                                                               | a |
| ex |                                                               | l |
| `` |                                                               |   |
| ** |                                                               |   |
+----+---------------------------------------------------------------+---+
| ** | When a binding is targeting a modulator, this is the same     | O |
| `` | thing as the ``position`` attribute. It is a specific 0-based | p |
| mo | index of the modulator to be modified by this binding.        | t |
| du |                                                               | i |
| la |                                                               | o |
| to |                                                               | n |
| rI |                                                               | a |
| nd |                                                               | l |
| ex |                                                               |   |
| `` |                                                               |   |
| ** |                                                               |   |
+----+---------------------------------------------------------------+---+
| ** | A comma-separated list of tags to be modified by this         | O |
| `` | binding. This allows you to set values for multiple groups at | p |
| ta | once by targeting a tag that is assigned to the groups.       | t |
| gs |                                                               | i |
| `` |                                                               | o |
| ** |                                                               | n |
|    |                                                               | a |
|    |                                                               | l |
+----+---------------------------------------------------------------+---+
| ** | A string identifying the specific parameter that you wish to  | R |
| `` | change. If you are modulating based on tags, you would put    | e |
| id | the tag you are targeting here. See Appendix D for example.   | q |
| en |                                                               | u |
| ti |                                                               | i |
| fi |                                                               | r |
| er |                                                               | e |
| `` |                                                               | d |
| ** |                                                               |   |
+----+---------------------------------------------------------------+---+
| *  | A token describing the specific kind of parameter that you    | R |
| *` | wish to change. A list of controller parameters are below.    | e |
| `p |                                                               | q |
| ar |                                                               | u |
| am |                                                               | i |
| et |                                                               | r |
| er |                                                               | e |
| `` |                                                               | d |
| ** |                                                               |   |
+----+---------------------------------------------------------------+---+
| *  | Valid values are ``fixed_value``, ``linear`` and ``table``.   | O |
| *` | Explanation of both translation modes is in a separate        | p |
| `t | section below. Default: ``linear``                            | t |
| ra |                                                               | i |
| ns |                                                               | o |
| la |                                                               | n |
| ti |                                                               | a |
| on |                                                               | l |
| `` |                                                               |   |
| ** |                                                               |   |
+----+---------------------------------------------------------------+---+
| ** | This is the min value this binding should send to the target  | O |
| `` | parameter. This is only looked at if translation is set to    | p |
| tr | ``linear``.                                                   | t |
| an |                                                               | i |
| sl |                                                               | o |
| at |                                                               | n |
| io |                                                               | a |
| nO |                                                               | l |
| ut |                                                               |   |
| pu |                                                               |   |
| tM |                                                               |   |
| in |                                                               |   |
| `` |                                                               |   |
| ** |                                                               |   |
+----+---------------------------------------------------------------+---+
| ** | This is the max value this binding should send to the target  | O |
| `` | parameter. This is only looked at if translation is set to    | p |
| tr | ``linear``.                                                   | t |
| an |                                                               | i |
| sl |                                                               | o |
| at |                                                               | n |
| io |                                                               | a |
| nO |                                                               | l |
| ut |                                                               |   |
| pu |                                                               |   |
| tM |                                                               |   |
| ax |                                                               |   |
| `` |                                                               |   |
| ** |                                                               |   |
+----+---------------------------------------------------------------+---+
| *  | Valid values are ``true`` and ``false``. Default: ``false``.  | O |
| *` | This is only looked at if translation is set to ``linear``.   | p |
| `t |                                                               | t |
| ra |                                                               | i |
| ns |                                                               | o |
| la |                                                               | n |
| ti |                                                               | a |
| on |                                                               | l |
| Re |                                                               |   |
| ve |                                                               |   |
| rs |                                                               |   |
| ed |                                                               |   |
| `` |                                                               |   |
| ** |                                                               |   |
+----+---------------------------------------------------------------+---+
| ** | A list of input-output pairs that make up the translation     | O |
| `` | table. The input and output are separated by commas. The      | p |
| tr | groups of coordinates themselves are separated by             | t |
| an | semi-colons. Default: ``0,0;1,1``. You must have at least two | i |
| sl | coordinates in your list. This is only looked at if           | o |
| at | translation is set to ``table``.                              | n |
| io |                                                               | a |
| nT |                                                               | l |
| ab |                                                               |   |
| le |                                                               |   |
| `` |                                                               |   |
| ** |                                                               |   |
+----+---------------------------------------------------------------+---+
| ** | The value that should be passed along when ``translation`` is | O |
| `` | set to ``fixed_value``.                                       | p |
| tr |                                                               | t |
| an |                                                               | i |
| sl |                                                               | o |
| at |                                                               | n |
| io |                                                               | a |
| nV |                                                               | l |
| al |                                                               |   |
| ue |                                                               |   |
| `` |                                                               |   |
| ** |                                                               |   |
+----+---------------------------------------------------------------+---+

Controllable Parameters
~~~~~~~~~~~~~~~~~~~~~~~

This is a list of parameters that can be used in conjunction with the
``<binding>`` element above.

+----------+---+------+-----+-------+---+----------------------------+
| Des      | ` | `    | `   | Valid | M | Additional required        |
| cription | ` | `lev | `pa | Range | o | parameters                 |
|          | t | el`` | ram |       | d |                            |
|          | y |      | ete |       | u |                            |
|          | p |      | r`` |       | l |                            |
|          | e |      |     |       | a |                            |
|          | ` |      |     |       | t |                            |
|          | ` |      |     |       | a |                            |
|          |   |      |     |       | b |                            |
|          |   |      |     |       | l |                            |
|          |   |      |     |       | e |                            |
+==========+===+======+=====+=======+===+============================+
| Global   | ` | ``   | ``  | 0.0 - | N | N/A                        |
| Volume   | ` | inst | AMP | 16.0  | o |                            |
|          | a | rume | _VO |       |   |                            |
|          | m | nt`` | LUM |       |   |                            |
|          | p |      | E`` |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Global   | ` | ``   | ``  | -36.0 | N | N/A                        |
| Tuning   | ` | inst | GLO | -     | o |                            |
|          | a | rume | BAL | 36.0  |   |                            |
|          | m | nt`` | _TU |       |   |                            |
|          | p |      | NIN |       |   |                            |
|          | ` |      | G`` |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Global   | ` | ``   | `   | -100  | N | N/A                        |
| Pan      | ` | inst | `PA | - 100 | o |                            |
|          | a | rume | N`` |       |   |                            |
|          | m | nt`` |     |       |   |                            |
|          | p |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Sample   | ` | ``   | `   | 0 -   | N | This value will be in      |
| Start    | ` | inst | `SA | last  | o | number of raw samples      |
| (see     | g | rume | MPL | s     |   | where 0 is the beginning.  |
| note 2   | e | nt`` | E_S | ample |   | See note 2 below.          |
| below)   | n | or   | TAR |       |   |                            |
|          | e | `    | T`` |       |   |                            |
|          | r | `gro |     |       |   |                            |
|          | a | up`` |     |       |   |                            |
|          | l |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Sample   | ` | ``   | ``  | 0 -   | N | This value will be in      |
| End (see | ` | inst | SAM | last  | o | number of raw samples      |
| note 2   | g | rume | PLE | s     |   | where 0 is the beginning.  |
| below)   | e | nt`` | _EN | ample |   | See note 2 below.          |
|          | n | or   | D`` |       |   |                            |
|          | e | `    |     |       |   |                            |
|          | r | `gro |     |       |   |                            |
|          | a | up`` |     |       |   |                            |
|          | l |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Loop     | ` | ``   | ``  | 0 -   | N | This value will be in      |
| Start    | ` | inst | LOO | last  | o | number of raw samples      |
| (see     | g | rume | P_S | s     |   | where 0 is the beginning.  |
| note 2   | e | nt`` | TAR | ample |   | See note 2 below.          |
| below)   | n | or   | T`` |       |   |                            |
|          | e | `    |     |       |   |                            |
|          | r | `gro |     |       |   |                            |
|          | a | up`` |     |       |   |                            |
|          | l |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Loop End | ` | ``   | ``L | 0 -   | N | This value will be in      |
| (see     | ` | inst | OOP | last  | o | number of raw samples      |
| note 2   | g | rume | _EN | s     |   | where 0 is the beginning.  |
| below)   | e | nt`` | D`` | ample |   | See note 2 below.          |
|          | n | or   |     |       |   |                            |
|          | e | `    |     |       |   |                            |
|          | r | `gro |     |       |   |                            |
|          | a | up`` |     |       |   |                            |
|          | l |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| A        | ` | ``   | ``  | 0.0 - | N | N/A                        |
| mplitude | ` | inst | AMP | 1.0   | o |                            |
| Velocity | a | rume | _VE |       |   |                            |
| Tracking | m | nt`` | L_T |       |   |                            |
|          | p |      | RAC |       |   |                            |
|          | ` |      | K`` |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Global   | ` | ``   | ``  | 0.0 - | N | N/A                        |
| Amp      | ` | inst | ENV | 10.0  | o |                            |
| Envelope | a | rume | _AT |       |   |                            |
| Attack   | m | nt`` | TAC |       |   |                            |
|          | p |      | K`` |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Global   | ` | ``   | ``  | -100  | N | N/A                        |
| Amp      | ` | inst | ENV | - 100 | o |                            |
| Envelope | a | rume | _AT |       |   |                            |
| Attack   | m | nt`` | TAC |       |   |                            |
| Curve    | p |      | K_C |       |   |                            |
| Shape    | ` |      | URV |       |   |                            |
|          | ` |      | E`` |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Global   | ` | ``   | `   | 0.0 - | N | N/A                        |
| Amp      | ` | inst | `EN | 25.0  | o |                            |
| Envelope | a | rume | V_D |       |   |                            |
| Decay    | m | nt`` | ECA |       |   |                            |
|          | p |      | Y`` |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Global   | ` | ``   | `   | -100  | N | N/A                        |
| Amp      | ` | inst | `EN | - 100 | o |                            |
| Envelope | a | rume | V_D |       |   |                            |
| Decay    | m | nt`` | ECA |       |   |                            |
| Curve    | p |      | Y_C |       |   |                            |
| Shape    | ` |      | URV |       |   |                            |
|          | ` |      | E`` |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Global   | ` | ``   | ``E | 0.0 - | N | N/A                        |
| Amp      | ` | inst | NV_ | 1.0   | o |                            |
| Envelope | a | rume | SUS |       |   |                            |
| Sustain  | m | nt`` | TAI |       |   |                            |
|          | p |      | N`` |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Global   | ` | ``   | ``E | 0.0 - | N | N/A                        |
| Amp      | ` | inst | NV_ | 25.0  | o |                            |
| Envelope | a | rume | REL |       |   |                            |
| Release  | m | nt`` | EAS |       |   |                            |
|          | p |      | E`` |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Global   | ` | ``   | ``E | -100  | N | N/A                        |
| Amp      | ` | inst | NV_ | - 100 | o |                            |
| Envelope | a | rume | REL |       |   |                            |
| Release  | m | nt`` | EAS |       |   |                            |
| Curve    | p |      | E_C |       |   |                            |
| Shape    | ` |      | URV |       |   |                            |
|          | ` |      | E`` |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Glide/Po | ` | ``   | ``  | 0.0 - | N | N/A                        |
| rtamento | ` | inst | GLI | 10.0  | o |                            |
| Time     | a | rume | DE_ |       |   |                            |
|          | m | nt`` | TIM |       |   |                            |
|          | p |      | E`` |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Effect   | ` | ``   | ``  | f     | Y | ``effectIndex`` or         |
| Enabled  | ` | inst | ENA | alse, | e | ``position`` contains the  |
| (all     | e | rume | BLE | true  | s | 0-based index of the       |
| effects) | f | nt`` | D`` |       |   | effect                     |
|          | f |      |     |       |   |                            |
|          | e |      |     |       |   |                            |
|          | c |      |     |       |   |                            |
|          | t |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Con      | ` | ``   | `   | 0.0 - | Y | ``effectIndex`` or         |
| volution | ` | inst | `FX | 1.0   | e | ``position`` contains the  |
| Mix      | e | rume | _MI |       | s | 0-based index of the       |
| Level    | f | nt`` | X`` |       |   | effect                     |
|          | f |      |     |       |   |                            |
|          | e |      |     |       |   |                            |
|          | c |      |     |       |   |                            |
|          | t |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Con      | ` | ``   | ``  | Text  | N | ``effectIndex`` or         |
| volution | ` | inst | FX_ |       | o | ``position`` contains the  |
| IR File  | e | rume | IR_ |       |   | 0-based index of the       |
|          | f | nt`` | FIL |       |   | effect                     |
|          | f |      | E`` |       |   |                            |
|          | e |      |     |       |   |                            |
|          | c |      |     |       |   |                            |
|          | t |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Filter   | ` | ``   | ``  | 0.0 - | Y | ``effectIndex`` or         |
| F        | ` | inst | FX_ | 22    | e | ``position`` contains the  |
| requency | e | rume | FIL | 000.0 | s | 0-based index of the       |
| (for     | f | nt`` | TER |       |   | effect                     |
| several  | f |      | _FR |       |   |                            |
| filters) | e |      | EQU |       |   |                            |
|          | c |      | ENC |       |   |                            |
|          | t |      | Y`` |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Peak or  | ` | ``   | ``F | 0.01  | Y | ``effectIndex`` or         |
| Notch    | ` | inst | X_F | -     | e | ``position`` contains the  |
| Filter Q | e | rume | ILT | 18.0  | s | 0-based index of the       |
|          | f | nt`` | ER_ |       |   | effect                     |
|          | f |      | Q`` |       |   |                            |
|          | e |      |     |       |   |                            |
|          | c |      |     |       |   |                            |
|          | t |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Peak or  | ` | ``   | ``F | 0.0 - | Y | ``effectIndex`` or         |
| Notch    | ` | inst | X_F | 10.0  | e | ``position`` contains the  |
| Filter   | e | rume | ILT |       | s | 0-based index of the       |
| Gain     | f | nt`` | ER_ |       |   | effect                     |
|          | f |      | GAI |       |   |                            |
|          | e |      | N`` |       |   |                            |
|          | c |      |     |       |   |                            |
|          | t |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Low-pass | ` | ``   | ``  | 0.0 - | Y | ``effectIndex`` or         |
| or       | ` | inst | FX_ | 5.0   | e | ``position`` contains the  |
| H        | e | rume | FIL |       | s | 0-based index of the       |
| igh-pass | f | nt`` | TER |       |   | effect                     |
| Filter   | f |      | _RE |       |   |                            |
| R        | e |      | SON |       |   |                            |
| esonance | c |      | ANC |       |   |                            |
|          | t |      | E`` |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Reverb   | ` | ``   | ``  | 0.0 - | Y | ``effectIndex`` or         |
| Wet      | ` | inst | FX_ | 1.0   | e | ``position`` contains the  |
| Level    | e | rume | REV |       | s | 0-based index of the       |
|          | f | nt`` | ERB |       |   | effect                     |
|          | f |      | _WE |       |   |                            |
|          | e |      | T_L |       |   |                            |
|          | c |      | EVE |       |   |                            |
|          | t |      | L`` |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Reverb   | ` | ``   | ``  | 0.0 - | Y | ``effectIndex`` or         |
| Room     | ` | inst | FX_ | 1.0   | e | ``position`` contains the  |
| Size     | e | rume | REV |       | s | 0-based index of the       |
|          | f | nt`` | ERB |       |   | effect                     |
|          | f |      | _RO |       |   |                            |
|          | e |      | OM_ |       |   |                            |
|          | c |      | SIZ |       |   |                            |
|          | t |      | E`` |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Reverb   | ` | ``   | ``F | 0.0 - | Y | ``effectIndex`` or         |
| Damping  | ` | inst | X_R | 1.0   | e | ``position`` contains the  |
|          | e | rume | EVE |       | s | 0-based index of the       |
|          | f | nt`` | RB_ |       |   | effect                     |
|          | f |      | DAM |       |   |                            |
|          | e |      | PIN |       |   |                            |
|          | c |      | G`` |       |   |                            |
|          | t |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| C        | ` | ``   | `   | 0.0 - | Y | ``effectIndex`` or         |
| horus/Ph | ` | inst | `FX | 1.0   | e | ``position`` contains the  |
| aser/Con | e | rume | _MI |       | s | 0-based index of the       |
| volution | f | nt`` | X`` |       |   | effect                     |
| Mix      | f |      |     |       |   |                            |
| Level    | e |      |     |       |   |                            |
|          | c |      |     |       |   |                            |
|          | t |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Choru    | ` | ``   | `   | 0.0 - | Y | ``effectIndex`` or         |
| s/Phaser | ` | inst | `FX | 1.0   | e | ``position`` contains the  |
| Mod      | e | rume | _MO |       | s | 0-based index of the       |
| Depth    | f | nt`` | D_D |       |   | effect                     |
|          | f |      | EPT |       |   |                            |
|          | e |      | H`` |       |   |                            |
|          | c |      |     |       |   |                            |
|          | t |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Choru    | ` | ``   | ``F | 0.0 - | Y | ``effectIndex`` or         |
| s/Phaser | ` | inst | X_M | 10.0  | e | ``position`` contains the  |
| Mod Rate | e | rume | OD_ |       | s | 0-based index of the       |
|          | f | nt`` | RAT |       |   | effect                     |
|          | f |      | E`` |       |   |                            |
|          | e |      |     |       |   |                            |
|          | c |      |     |       |   |                            |
|          | t |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Phaser   | ` | ``   | ``  | 0.0 - | Y | ``effectIndex`` or         |
| Center   | ` | inst | FX_ | 22    | e | ``position`` contains the  |
| F        | e | rume | CEN | 000.0 | s | 0-based index of the       |
| requency | f | nt`` | TER |       |   | effect                     |
|          | f |      | _FR |       |   |                            |
|          | e |      | EQU |       |   |                            |
|          | c |      | ENC |       |   |                            |
|          | t |      | Y`` |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Phas     | ` | ``   | ``F | 0.0 - | Y | ``effectIndex`` or         |
| er/Delay | ` | inst | X_F | 1.0   | e | ``position`` contains the  |
| Feedback | e | rume | EED |       | s | 0-based index of the       |
|          | f | nt`` | BAC |       |   | effect                     |
|          | f |      | K`` |       |   |                            |
|          | e |      |     |       |   |                            |
|          | c |      |     |       |   |                            |
|          | t |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Delay    | ` | ``   | ``  | 0.0 - | Y | ``effectIndex`` or         |
| Time     | ` | inst | FX_ | 1.0   | e | ``position`` contains the  |
|          | e | rume | DEL |       | s | 0-based index of the       |
|          | f | nt`` | AY_ |       |   | effect                     |
|          | f |      | TIM |       |   |                            |
|          | e |      | E`` |       |   |                            |
|          | c |      |     |       |   |                            |
|          | t |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Delay    | ` | ``   | ``F | sec   | Y | ``effectIndex`` or         |
| Time     | ` | inst | X_D | onds, | e | ``position`` contains the  |
| Format   | e | rume | ELA | mu    | s | 0-based index of the       |
|          | f | nt`` | Y_T | sical |   | effect                     |
|          | f |      | IME | _time |   |                            |
|          | e |      | _FO |       |   |                            |
|          | c |      | RMA |       |   |                            |
|          | t |      | T`` |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Delay    | ` | ``   | ``  | 0.0 - | Y | ``effectIndex`` or         |
| Stereo   | ` | inst | FX_ | 1.0   | e | ``position`` contains the  |
| Offset   | e | rume | STE |       | s | 0-based index of the       |
|          | f | nt`` | REO |       |   | effect                     |
|          | f |      | _OF |       |   |                            |
|          | e |      | FSE |       |   |                            |
|          | c |      | T`` |       |   |                            |
|          | t |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Delay    | ` | ``   | `   | 0.0 - | Y | ``effectIndex`` or         |
| Wet      | ` | inst | `FX | 1.0   | e | ``position`` contains the  |
| Level    | e | rume | _WE |       | s | 0-based index of the       |
|          | f | nt`` | T_L |       |   | effect                     |
|          | f |      | EVE |       |   |                            |
|          | e |      | L`` |       |   |                            |
|          | c |      |     |       |   |                            |
|          | t |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Gain     | ` | ``   | ``L | 0.0 - | Y | ``groupIndex`` or          |
| Level    | ` | inst | EVE | 8.0   | e | ``position`` contains the  |
|          | e | rume | L`` |       | s | 0-based index of the group |
|          | f | nt`` |     |       |   |                            |
|          | f |      |     |       |   |                            |
|          | e |      |     |       |   |                            |
|          | c |      |     |       |   |                            |
|          | t |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Wave     | ` | ``   | ``F | 1 -   | Y | ``groupIndex`` or          |
| Folder   | ` | inst | X_D | 100   | e | ``position`` contains the  |
| Drive    | e | rume | RIV |       | s | 0-based index of the group |
| Level    | f | nt`` | E`` |       |   |                            |
|          | f |      |     |       |   |                            |
|          | e |      |     |       |   |                            |
|          | c |      |     |       |   |                            |
|          | t |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Wave     | ` | ``   | `   | 1 -   | Y | ``groupIndex`` or          |
| Folder   | ` | inst | `FX | 100   | e | ``position`` contains the  |
| T        | e | rume | _TH |       | s | 0-based index of the group |
| hreshold | f | nt`` | RES |       |   |                            |
|          | f |      | HOL |       |   |                            |
|          | e |      | D`` |       |   |                            |
|          | c |      |     |       |   |                            |
|          | t |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Wave     | ` | ``   | ``F | 0.0 - | Y | ``groupIndex`` or          |
| Shaper   | ` | inst | X_D | 1     | e | ``position`` contains the  |
| Drive    | e | rume | RIV | 000.0 | s | 0-based index of the group |
| Level    | f | nt`` | E`` |       |   |                            |
|          | f |      |     |       |   |                            |
|          | e |      |     |       |   |                            |
|          | c |      |     |       |   |                            |
|          | t |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Wave     | ` | ``   | `   | 0.0 - | Y | ``groupIndex`` or          |
| Shaper   | ` | inst | `FX | 8.0   | e | ``position`` contains the  |
| Output   | e | rume | _OU |       | s | 0-based index of the group |
| Level    | f | nt`` | TPU |       |   |                            |
|          | f |      | T_L |       |   |                            |
|          | e |      | EVE |       |   |                            |
|          | c |      | L`` |       |   |                            |
|          | t |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Wave     | ` | ``   | ``F | 0.0 - | Y | ``groupIndex`` or          |
| Shaper   | ` | inst | X_D | 1.0   | e | ``position`` contains the  |
| Drive    | e | rume | RIV |       | s | 0-based index of the group |
| Boost    | f | nt`` | E_B |       |   |                            |
|          | f |      | OOS |       |   |                            |
|          | e |      | T`` |       |   |                            |
|          | c |      |     |       |   |                            |
|          | t |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Group    | ` | `    | ``  | true, |   | ``groupIndex`` or          |
| Enabled  | ` | `gro | ENA | false |   | ``position`` contains the  |
| /        | a | up`` | BLE |       |   | 0-based index of the group |
| Disabled | m |      | D`` |       |   |                            |
|          | p |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Group    | ` | `    | ``  | 0.0 - | Y | ``groupIndex`` or          |
| Volume   | ` | `gro | AMP | 16.0  | e | ``position`` contains the  |
|          | a | up`` | _VO |       | s | 0-based index of the group |
|          | m |      | LUM |       |   |                            |
|          | p |      | E`` |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Group    | ` | `    | `   | -36.0 | Y | ``groupIndex`` or          |
| Tuning   | ` | `gro | `GR | -     | e | ``position`` contains the  |
|          | a | up`` | OUP | 36.0  | s | 0-based index of the group |
|          | m |      | _TU |       |   |                            |
|          | p |      | NIN |       |   |                            |
|          | ` |      | G`` |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Pan      | ` | `    | `   | -100  | Y | ``groupIndex`` or          |
|          | ` | `gro | `PA | - 100 | e | ``position`` contains the  |
|          | a | up`` | N`` |       | s | 0-based index of the group |
|          | m |      |     |       |   |                            |
|          | p |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| A        | ` | `    | ``  | 0.0 - |   | ``groupIndex`` or          |
| mplitude | ` | `gro | AMP | 1.0   |   | ``position`` contains the  |
| Velocity | a | up`` | _VE |       |   | 0-based index of the group |
| Tracking | m |      | L_T |       |   |                            |
|          | p |      | RAC |       |   |                            |
|          | ` |      | K`` |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Group    | ` | `    | ``  | 0.0 - |   | ``groupIndex`` or          |
| Amp      | ` | `gro | ENV | 10.0  |   | ``position`` contains the  |
| Envelope | a | up`` | _AT |       |   | 0-based index of the group |
| Attack   | m |      | TAC |       |   |                            |
|          | p |      | K`` |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Group    | ` | `    | `   | 0.0 - |   | ``groupIndex`` or          |
| Amp      | ` | `gro | `EN | 25.0  |   | ``position`` contains the  |
| Envelope | a | up`` | V_D |       |   | 0-based index of the group |
| Decay    | m |      | ECA |       |   |                            |
|          | p |      | Y`` |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Group    | ` | `    | ``E | 0.0 - |   | ``groupIndex`` or          |
| Amp      | ` | `gro | NV_ | 1.0   |   | ``position`` contains the  |
| Envelope | a | up`` | SUS |       |   | 0-based index of the group |
| Sustain  | m |      | TAI |       |   |                            |
|          | p |      | N`` |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Group    | ` | `    | ``E | 0.0 - |   | ``groupIndex`` or          |
| Amp      | ` | `gro | NV_ | 25.0  |   | ``position`` contains the  |
| Envelope | a | up`` | REL |       |   | 0-based index of the group |
| Release  | m |      | EAS |       |   |                            |
|          | p |      | E`` |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Group    | ` | `    | ``  | 0.0 - |   | ``groupIndex`` or          |
| Glide/Po | ` | `gro | GLI | 10.0  |   | ``position`` contains the  |
| rtamento | a | up`` | DE_ |       |   | 0-based index of the group |
| Time     | m |      | TIM |       |   |                            |
|          | p |      | E`` |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Tag      | ` | ``t  | ``T | true, |   | ``identifier`` contains    |
| Enabled  | ` | ag`` | AG_ | false |   | the tag name               |
|          | a |      | ENA |       |   |                            |
|          | m |      | BLE |       |   |                            |
|          | p |      | D`` |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Tag      | ` | ``t  | ``  | 0.0 - |   | ``identifier`` contains    |
| Volume   | ` | ag`` | TAG | 16.0  |   | the tag name               |
|          | a |      | _VO |       |   |                            |
|          | m |      | LUM |       |   |                            |
|          | p |      | E`` |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| UI       | ` | ``   | ``  | true, |   | ``controlIndex`` or        |
| Control  | ` | ui`` | VIS | false |   | ``position`` contains the  |
| Visibile | c |      | IBL |       |   | 0-based index of the       |
|          | o |      | E`` |       |   | control in question (see   |
|          | n |      |     |       |   | note 1 below)              |
|          | t |      |     |       |   |                            |
|          | r |      |     |       |   |                            |
|          | o |      |     |       |   |                            |
|          | l |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| UI       | ` | ``   | ``V | Any   |   | ``controlIndex`` or        |
| Control  | ` | ui`` | ALU | n     |   | ``position`` contains the  |
| Value    | c |      | E`` | umber |   | 0-based index of the       |
|          | o |      |     |       |   | control in question (see   |
|          | n |      |     |       |   | note 1 below)              |
|          | t |      |     |       |   |                            |
|          | r |      |     |       |   |                            |
|          | o |      |     |       |   |                            |
|          | l |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| UI       | ` | ``   | ``  | Text  |   | ``controlIndex`` or        |
| Control  | ` | ui`` | TEX |       |   | ``position`` contains the  |
| Text     | c |      | T`` |       |   | 0-based index of the       |
|          | o |      |     |       |   | control in question (see   |
|          | n |      |     |       |   | note 1 below)              |
|          | t |      |     |       |   |                            |
|          | r |      |     |       |   |                            |
|          | o |      |     |       |   |                            |
|          | l |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| UI       | ` | ``   | `   | Any   |   | ``controlIndex`` or        |
| Control  | ` | ui`` | `MI | n     |   | ``position`` contains the  |
| Minimum  | c |      | N_V | umber |   | 0-based index of the       |
| Value    | o |      | ALU |       |   | control in question (see   |
|          | n |      | E`` |       |   | note 1 below)              |
|          | t |      |     |       |   |                            |
|          | r |      |     |       |   |                            |
|          | o |      |     |       |   |                            |
|          | l |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| UI       | ` | ``   | `   | Any   |   | ``controlIndex`` or        |
| Control  | ` | ui`` | `MA | n     |   | ``position`` contains the  |
| Maximum  | c |      | X_V | umber |   | 0-based index of the       |
| Value    | o |      | ALU |       |   | control in question (see   |
|          | n |      | E`` |       |   | note 1 below)              |
|          | t |      |     |       |   |                            |
|          | r |      |     |       |   |                            |
|          | o |      |     |       |   |                            |
|          | l |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| UI       | ` | ``   | ``  | f     |   | ``controlIndex`` or        |
| Control  | ` | ui`` | VAL | loat, |   | ``position`` contains the  |
| Value    | c |      | UE_ | int   |   | 0-based index of the       |
| Type     | o |      | TYP | eger, |   | control in question (see   |
|          | n |      | E`` | mu    |   | note 1 below)              |
|          | t |      |     | sical |   |                            |
|          | r |      |     | _time |   |                            |
|          | o |      |     |       |   |                            |
|          | l |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| UI Image | ` | ``   | ``  | Text  |   | ``controlIndex`` or        |
| Control  | ` | ui`` | PAT |       |   | ``position`` contains the  |
| Path     | c |      | H`` |       |   | 0-based index of the       |
|          | o |      |     |       |   | control in question (see   |
|          | n |      |     |       |   | note 1 below)              |
|          | t |      |     |       |   |                            |
|          | r |      |     |       |   |                            |
|          | o |      |     |       |   |                            |
|          | l |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| M        | ` | ``   | ``  | 0.0 - |   | ``modulatorIndex``         |
| odulator | ` | inst | MOD | 1.0   |   | contains the 0-based index |
| Amount   | m | rume | _AM |       |   | of the modulator in        |
| (Depth)  | o | nt`` | OUN |       |   | question                   |
|          | d |      | T`` |       |   |                            |
|          | u |      |     |       |   |                            |
|          | l |      |     |       |   |                            |
|          | a |      |     |       |   |                            |
|          | t |      |     |       |   |                            |
|          | o |      |     |       |   |                            |
|          | r |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| LFO      | ` | ``   | `   | 0.0 - |   | ``modulatorIndex``         |
| M        | ` | inst | `FR | 22    |   | contains the 0-based index |
| odulator | m | rume | EQU | 000.0 |   | of the modulator in        |
| Rate (or | o | nt`` | ENC |       |   | question                   |
| Fr       | d |      | Y`` |       |   |                            |
| equency) | u |      |     |       |   |                            |
|          | l |      |     |       |   |                            |
|          | a |      |     |       |   |                            |
|          | t |      |     |       |   |                            |
|          | o |      |     |       |   |                            |
|          | r |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Envelope | ` | ``   | ``  | 0.0 - |   | ``modulatorIndex``         |
| M        | ` | inst | ENV | 10.0  |   | contains the 0-based index |
| odulator | m | rume | _AT |       |   | of the modulator in        |
| Attack   | o | nt`` | TAC |       |   | question                   |
|          | d |      | K`` |       |   |                            |
|          | u |      |     |       |   |                            |
|          | l |      |     |       |   |                            |
|          | a |      |     |       |   |                            |
|          | t |      |     |       |   |                            |
|          | o |      |     |       |   |                            |
|          | r |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Envelope | ` | ``   | ``  | -100  |   | ``modulatorIndex``         |
| M        | ` | inst | ENV | - 100 |   | contains the 0-based index |
| odulator | m | rume | _AT |       |   | of the modulator in        |
| Attack   | o | nt`` | TAC |       |   | question                   |
| Curve    | d |      | K_C |       |   |                            |
| Shape    | u |      | URV |       |   |                            |
|          | l |      | E`` |       |   |                            |
|          | a |      |     |       |   |                            |
|          | t |      |     |       |   |                            |
|          | o |      |     |       |   |                            |
|          | r |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Envelope | ` | ``   | `   | 0.0 - |   | ``modulatorIndex``         |
| M        | ` | inst | `EN | 25.0  |   | contains the 0-based index |
| odulator | m | rume | V_D |       |   | of the modulator in        |
| Decay    | o | nt`` | ECA |       |   | question                   |
|          | d |      | Y`` |       |   |                            |
|          | u |      |     |       |   |                            |
|          | l |      |     |       |   |                            |
|          | a |      |     |       |   |                            |
|          | t |      |     |       |   |                            |
|          | o |      |     |       |   |                            |
|          | r |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Envelope | ` | ``   | `   | -100  |   | ``modulatorIndex``         |
| M        | ` | inst | `EN | - 100 |   | contains the 0-based index |
| odulator | m | rume | V_D |       |   | of the modulator in        |
| Decay    | o | nt`` | ECA |       |   | question                   |
| Curve    | d |      | Y_C |       |   |                            |
| Shape    | u |      | URV |       |   |                            |
|          | l |      | E`` |       |   |                            |
|          | a |      |     |       |   |                            |
|          | t |      |     |       |   |                            |
|          | o |      |     |       |   |                            |
|          | r |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Envelope | ` | ``   | ``E | 0.0 - |   | ``modulatorIndex``         |
| M        | ` | inst | NV_ | 1.0   |   | contains the 0-based index |
| odulator | m | rume | SUS |       |   | of the modulator in        |
| Sustain  | o | nt`` | TAI |       |   | question                   |
|          | d |      | N`` |       |   |                            |
|          | u |      |     |       |   |                            |
|          | l |      |     |       |   |                            |
|          | a |      |     |       |   |                            |
|          | t |      |     |       |   |                            |
|          | o |      |     |       |   |                            |
|          | r |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Envelope | ` | ``   | ``E | 0.0 - |   | ``modulatorIndex``         |
| M        | ` | inst | NV_ | 25.0  |   | contains the 0-based index |
| odulator | m | rume | REL |       |   | of the modulator in        |
| Release  | o | nt`` | EAS |       |   | question                   |
|          | d |      | E`` |       |   |                            |
|          | u |      |     |       |   |                            |
|          | l |      |     |       |   |                            |
|          | a |      |     |       |   |                            |
|          | t |      |     |       |   |                            |
|          | o |      |     |       |   |                            |
|          | r |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+
| Envelope | ` | ``   | ``E | -100  |   | ``modulatorIndex``         |
| M        | ` | inst | NV_ | - 100 |   | contains the 0-based index |
| odulator | m | rume | REL |       |   | of the modulator in        |
| Release  | o | nt`` | EAS |       |   | question                   |
| Curve    | d |      | E_C |       |   |                            |
| Shape    | u |      | URV |       |   |                            |
|          | l |      | E`` |       |   |                            |
|          | a |      |     |       |   |                            |
|          | t |      |     |       |   |                            |
|          | o |      |     |       |   |                            |
|          | r |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
|          | ` |      |     |       |   |                            |
+----------+---+------+-----+-------+---+----------------------------+

1. NOTE: The indexes of the parameter list also include UI controls that
   are not editable, such as ``<label>`` UI controls, so you’ll want to
   account for that when calculating your positions.

   Here’s a quick example:

   If your UI’s ``<tab>`` section has the following elements under it:
   ``<label>``, ``<control>``,\ ``<label>``,\ ``<control>``. The
   ``position`` indexes of the four elements will be 0, 1, 2, 3.
   Therefore, the indexes of the two ``<control>`` elements will be 1
   and 3, respectively.

2. NOTE: If your sample library manipulates ``start``, ``end``,
   ``loopStart``, or ``loopEnd`` after a sample library’s initial load,
   the sample playback engine must be in RAM/Memory mode (not disk
   streaming) or you will get very unpredictable results. In order to
   enforce this, sample creators should use the ``playbackEngine``
   attribute.

Translation Modes
~~~~~~~~~~~~~~~~~

There are currently three binding translation modes: ``linear``,
``table``, ``fixed_value``

Mode #1: ``linear``
^^^^^^^^^^^^^^^^^^^

**linear** mode allows values that come in to be scaled up or down
before they get passed along to the binding’s target. If you set your
translation mode to ``linear`` you should also ``translationOutputMin``
and ``translationOutputMax``.

Example usage:

.. code:: xml

   <binding level="ui" type="control" position="0" parameter="value" translation="linear" 
                  translationOutputMin="0" translationOutputMax="1"/>

Mode #2: ``table``
^^^^^^^^^^^^^^^^^^

**table** mode allows you to transform the binding’s input in a more
complex fashion before it gets passed along to the binding’s target. If
you set your translation mode to ``table`` you must define the
``translationTable`` parameter as well. This consists of a series of
input-output pairs, separated by semi-colons.

Mode #3: ``fixed_value``
^^^^^^^^^^^^^^^^^^^^^^^^

**fixed_value** mode allows you to completely disregard the input of a
binding and instead always use a supplied value. In order to use this
translation mode, you must also specify a ``translationValue``. This can
be very useful when trying to have menu options enable and disable
groups. An example usage:

.. code:: xml

   <binding type="general" level="group" position="1" parameter="ENABLED" translation="fixed_value" translationValue="true" />

Appendix C: Boilerplate .dspreset File
--------------------------------------

.. code:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <DecentSampler minVersion="1.0.0">
     <ui width="812" height="375" layoutMode="relative"
         bgMode="top_left">
       <tab name="main">
         <labeled-knob x="445" y="75" width="90" textSize="16" textColor="AA000000" 
                       trackForegroundColor="CC000000" trackBackgroundColor="66999999" 
                       label="Attack" type="float" minValue="0.0" maxValue="4.0" value="0.01" >
           <binding type="amp" level="instrument" position="0" parameter="ENV_ATTACK" />
         </labeled-knob>
         <labeled-knob x="515" y="75" width="90" textSize="16" textColor="AA000000" 
                       trackForegroundColor="CC000000" trackBackgroundColor="66999999" 
                       label="Release" type="float" minValue="0.0" maxValue="20.0" value="1" >
           <binding type="amp" level="instrument" position="0" parameter="ENV_RELEASE" />
         </labeled-knob>
         <labeled-knob x="585" y="75" width="90" textSize="16" textColor="AA000000" 
                       trackForegroundColor="CC000000" trackBackgroundColor="66999999" 
                       label="Chorus" type="float" minValue="0.0" maxValue="1" value="0" >
           <binding type="effect" level="instrument" position="1" parameter="FX_MIX" />
         </labeled-knob>
         <labeled-knob x="655" y="75" width="90" textSize="16" textColor="FF000000"
                       trackForegroundColor="CC000000" trackBackgroundColor="66999999"
                       label="Tone" type="float" minValue="0" maxValue="1" value="1">
           <binding type="effect" level="instrument" position="0" parameter="FX_FILTER_FREQUENCY"
                    translation="table" 
                    translationTable="0,33;0.3,150;0.4,450;0.5,1100;0.7,4100;0.9,11000;1.0001,22000"/>
         </labeled-knob>
         <labeled-knob x="725" y="75" width="90" textSize="16" textColor="AA000000" 
                       trackForegroundColor="CC000000" trackBackgroundColor="66999999" 
                       label="Reverb" type="percent" minValue="0" maxValue="100" 
                       textColor="FF000000" value="50">
           <binding type="effect" level="instrument" position="2" 
                    parameter="FX_REVERB_WET_LEVEL" translation="linear" 
                    translationOutputMin="0" translationOutputMax="1" />
         </labeled-knob>
       </tab>
     </ui>
     <groups attack="0.000" decay="25" sustain="1.0" release="0.430" volume="-3dB">
       <group>
         <sample loNote="21" hiNote="21" rootNote="21" path="DefaultPiano-21.aif"
                 length="805888"/>
         <sample loNote="22" hiNote="33" rootNote="33" path="DefaultPiano-33.aif"
                 length="807552"/>
         <sample loNote="34" hiNote="45" rootNote="45" path="DefaultPiano-45.aif"
                 length="759168"/>
         <sample loNote="46" hiNote="57" rootNote="57" path="DefaultPiano-57.aif"
                 length="756480"/>
         <sample loNote="58" hiNote="69" rootNote="69" path="DefaultPiano-69.aif"
                 length="758656"/>
         <sample loNote="70" hiNote="77" rootNote="77" path="DefaultPiano-77.aif"
                 length="595328"/>
         <sample loNote="78" hiNote="89" rootNote="89" path="DefaultPiano-89.aif"
                 length="457600"/>
         <sample loNote="90" hiNote="96" rootNote="96" path="DefaultPiano-96.aif"
                 length="469888"/>
         <sample loNote="94" hiNote="108" rootNote="108" path="DefaultPiano-108.aif"
                 length="75264"/>
       </group>
     </groups>
     <effects>
       <effect type="lowpass" frequency="22000.0"/>
       <effect type="chorus"  mix="0.0" modDepth="0.2" modRate="0.2" />
       <effect type="reverb" wetLevel="0.5"/>
     </effects>
     <midi>
       <!-- This causes MIDI CC 1 to control the 4th knob (cutoff) -->
       <cc number="1">
         <binding level="ui" type="control" parameter="VALUE" position="3" 
                  translation="linear" translationOutputMin="0" 
                  translationOutputMax="1" />
       </cc>
     </midi>
   </DecentSampler>

Appendix D: How to Control Parameters Using Tags (Example: Mic-level Knobs)
---------------------------------------------------------------------------

As of version 1.0.2, the best way to implement mic-level knobs is using
the new sample tagging feature. It is possible to assign tags to
specific samples. In this way, you can specify which type of sound they
are:

.. code:: xml

   <sample volume="0.0dB" tags="note,mic1" />
   <sample volume="0.0dB" tags="rt,mic1" />
   <sample volume="0.0dB" tags="note,mic2" />
   <sample volume="0.0dB" tags="rt,mic2" />

You can also assign tags at the group level. You can also mix and match,
and the tags specified at the group level will be added to the list of
tags already specified at the sample level:

.. code:: xml

   <group tags="note">
     <sample volume="0.0dB" tags="mic1" />
     <sample volume="0.0dB" tags="mic2" />
   </group>
   <group tags="rt">
     <sample volume="0.0dB" tags="mic1" />
     <sample volume="0.0dB" tags="mic2" />
   </group>

Then you can make controls with bindings that reference those tags:

.. code:: xml

   <control x="246" y="115" parameterName="MIC 1" style="linear_bar_vertical" type="float" minValue="0" maxValue="100" value="60" width="20" height="70" trackForegroundColor="FFFFFFFF" trackBackgroundColor="FF888888">
       <binding type="amp" level="tag" identifier="mic1" parameter="AMP_VOLUME" />
   </control>

   <control x="346" y="115" parameterName="MIC 2" style="linear_bar_vertical" type="float" minValue="0" maxValue="100" value="60" width="20" height="70" trackForegroundColor="FFFFFFFF" trackBackgroundColor="FF888888">
       <binding type="amp" level="tag" identifier="mic2" parameter="AMP_VOLUME" />
   </control>

Appendix E: How to do voice-muting for drums
--------------------------------------------

Voice muting makes use of the tags functionality, these are text labels
that you can use to identify samples or groups of samples. You start by
adding tags to all of your groups (you can also add them to individual
samples if you’d like). Next, you add a ``silencedByTags`` attribute to
groups or sample elements that you want to be silenced by other samples.
When a sample with a tag matching one of the tags in the
``silencedByTags`` is played, it will silence the current sample (or
group).

Here’s an example:

.. code:: xml

   <DecentSampler>
       <groups>
           <group tags="hihat" silencedByTags="hihat" silencingMode="fast">
               <!-- Your samples go here. -->
           </group>
       </groups>
   </DecentSampler>

Note the use of the ``silencingMode`` attribute as well (a value of
“fast” means we immediately silence, whereas “normal” means we trigger
the ADSR release phase).

Appendix F: How to implement true legato
----------------------------------------

Let’s walk through how to build a basic true legato instrument. Legato
instruments generally consist of either two or three groups:

1. First, there’s the initial looping sustain sample that will get
   played when the note is first pressed down.
2. Then there is the legato transition sample that will get played when
   a second note gets pressed
3. (optional) Depending on the implementation, there may be a third
   looping sustain group that gets played after the legato transition
   sample plays. In such cases, this third group usually contains the
   same samples as were used in group 1.

**Step 1: Voice muting**

Before we get into legato, let’s talk about voice muting. This is the
behavior wherein one set of samples causes another set of samples to
stop playing. This can be desirable in situations such as legato
instruments where two samples should not be sounding at the same time.

In Decent Sampler, voice-muting makes use of tags. These are text labels
that you can use to identify samples or groups of samples. You can add a
``silencedByTags`` attribute to groups or sample elements. This consists
of a comma-separated list of tags that specify which samples should
silence the current samples. When a sample with a tag matching one of
the tags in the ``silencedByTags`` is played, it will silence the
current sample (or group). Here’s an example:

.. code:: xml

   <DecentSampler>
       <groups>
           <group tags="sustain" silencedByTags="legato" silencingMode="normal" release="1.0">
               <!-- Your sustain samples go here. -->
           </group>
           <group tags="legato" silencedByTags="legato" silencingMode="normal" release="1.0">
               <!-- Your legato samples go here. -->
           </group>
       </groups>
   </DecentSampler>

In the above scenario, if a legato sample is matched, any sample that
might be playing from the “sustain” group will be stopped.

It’s also worth mention the ``silencingMode`` attribute as well (a value
of ``fast`` means we immediately silence that sample, whereas ``normal``
means we trigger the ADSR release phase).

**Step 2: Legato**

In order to specify which samples should be triggered first, we use the
``trigger`` attribute. This can that can be added to the ``<group>`` or
individual ``<sample>`` tags. The default value is ``attack``, but there
are two useful new values:

-  **``first``**: This value means that this sample will only trigger if
   no other notes are playing
-  **``legato``**: This value means that this sample will only trigger
   if other notes are already playing

Here is an example of how this might look in cunjunction with our
example from above:

.. code:: xml

   <DecentSampler>
       <groups>
           <group trigger="first" tags="sustain" silencedByTags="legato" silencingMode="normal" release="1.0">
               <!-- Your sustain samples go here. -->
           </group>
           <group trigger="legato" tags="legato" silencedByTags="legato" silencingMode="normal" release="1.0">
               <!-- Your legato samples go here. -->
           </group>
       </groups>
   </DecentSampler>

**Step 3: Specifying previous notes**

When creating a legato instrument, it is often essential to limit which
legato transition gets played based on which note we are transition
from. This can be achieved using either the ``previousNote`` or the
``legatoInterval`` attributes. The ``previousNote`` attribute causes the
engine to only play this sample if the previously triggered note matches
the specified note. Example usage:

.. code:: xml

   <sample path="Samples/LV_Legato_F2_G2.wav" rootNote="G2" loNote="G2" hiNote="G2" previousNote="F2" start="43000" />
   <sample path="Samples/LV_Legato_G2_A2.wav" rootNote="A2" loNote="A2" hiNote="A2" previousNote="G2" start="43000" />

The ``legatoInterval`` attribute causes the engine to only play the
sample if distance between the current note and the previously triggered
note is exactly this semitone distance. For example, if the note for
which this sample is being triggered is a C3 and the ``legatoInterval``
is set to ``-2``, then the sample will only play if the previous note
was a D3 because D3 minus two semitones equals C3.

**Step 4: Polyphony**

In legato instruments, it is sometimes useful to limit polyphony for a
specific sample or set of samples. This is achieve using tags. At the
top-level of your file, you can specify a element as follows:

.. code:: xml

   <DecentSampler>
       <groups>
           <group tags="some-tag" >
               <!-- ... -->
           </group>
       </groups>
       <tags>
           <tag name="some-tag" polyphony="1" />
       </tags>
   </DecentSampler>

**Putting it all together**

Here is a full example of a legato instrument:

.. code:: xml

   <?xml version="1.0" encoding="UTF-8"?>

   <DecentSampler pluginVersion="1">
     <groups>
       <group tags="sustain" silencedByTags="legato" trigger="first"
              silencingMode="normal" release="0.3">
         <sample path="Samples/LV_Sustain_D3.wav" rootNote="D3" loNote="C3" hiNote="D3"/>
         <sample path="Samples/LV_Sustain_E3.wav" rootNote="E3" loNote="D#3" hiNote="E3"/>
         <!-- More samples go here -->
       </group>
       <group tags="legato" silencedByTags="legato" trigger="legato"
              silencingMode="normal" attack="0.1" decay="0.2" sustain="0" release="1">
                 previousNote="C#3"/>
         <sample path="Samples/LV_Legato_D3_E3.wav" rootNote="E3" loNote="E3"
                 hiNote="E3" start="43000" previousNote="D3"/>
         <sample path="Samples/LV_Legato_E3_F3.wav" rootNote="F3" loNote="F3"
                 hiNote="F3" start="43000" previousNote="E3"/>
         <!-- More samples go here -->
       </group>
       <group tags="legato-tails" silencedByTags="legato-tails" trigger="legato"
              attack="0.3" silencingMode="normal" release="0.3" start="5000">
         <sample path="Samples/LV_Sustain_D3.wav" rootNote="D3" loNote="C3" hiNote="D3"/>
         <sample path="Samples/LV_Sustain_E3.wav" rootNote="E3" loNote="D#3" hiNote="E3"/>
         <!-- More samples go here -->
       </group>
     </groups>
   </DecentSampler>

Appendix G: Useful Tutorials and Resources
==========================================

-  `How to add Dropdown Menus, Colored Keys, and
   Keyswitches <https://www.decentsamples.com/2022/01/25/dropdown-menus-colored-keys-and-keyswitches-come-to-decent-sampler/>`__
-  `How to add Buttons to your user
   interfaces <https://www.decentsamples.com/2022/11/27/for-sample-creators-how-to-create-buttons-in-your-sample-libraries/?swcfpc=1>`__
-  `Example: How to control two groups with
   knobs <https://www.decentsamples.com/decent-sampler-developer-resources/how-to-control-two-groups-with-knobs/?swcfpc=1>`__
-  `How to add LFOs and
   Envelopes <https://www.decentsamples.com/2022/08/19/how-to-add-lfos-and-extra-envelopes-to-your-decent-sampler-instruments/>`__
-  `How to use Legato in Decent Sampler
   [Video] <https://www.youtube.com/watch?v=uLPBcbsT6cU&feature=emb_title>`__
-  `How to do Voice-Muting (like for hi-hats) and create Legato
   samples <https://www.decentsamples.com/2021/06/14/decent-sampler-now-has-experimental-support-for-legato-samples-and-voice-muting/?swcfpc=1>`__
-  `How to use Convolution Reverb in Decent
   Sampler <https://www.decentsamples.com/2022/11/23/for-sample-creators-how-to-use-convolution-in-your-decent-sampler-presets/?swcfpc=1>`__
-  `How to add Tempo-synced
   Delay <https://www.decentsamples.com/2024/02/08/for-sample-creators-how-to-add-tempo-synced-delay-to-your-instruments/>`__
