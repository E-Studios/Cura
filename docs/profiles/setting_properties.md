Setting Properties
====
Each setting in Cura has a number of properties. It's not just a key and a value. This page lists the properties that a setting can define.

* `key` (string): The identifier by which the setting is referenced. This is not a human-readable name, but just a reference string, such as `layer_height_0`. Typically these are named with the most significant category first, in order to sort them better, such as `material_print_temperature`. This is not actually a real property but just an identifier; it can't be changed.
* `value` (optional): The current value of the setting. This can be a function, an arbitrary Python expression that depends on the values of other settings. If it's not present, the `default_value` is used.
* `default_value`: A default value for the setting if `value` is undefined. This property is required however. It can't be a Python expression, but it can be any JSON type. This is made separate so that CuraEngine can read it out as well for its debugging mode via the command line, without needing a complete Python interpreter.
* `label` (string): The human-readable name for the setting. This label is translated.
* `description` (string): A longer description of what the setting does when you change it. This description is translated as well.
* `type` (string): The type of value that this setting contains. Allowed types are: `bool`, `str`, `float`, `int`, `enum`, `category`, `[int]`, `vec3`, `polygon` and `polygons`.
* `unit` (optional string): A unit that is displayed at the right-hand side of the text field where the user enters the setting value.
* `resolve` (optional string): A Python expression that resolves disagreements for global settings if multiple per-extruder profiles define different values for a setting. Typically this takes the values for the setting from all stacks and computes one final value for it that will be used for the global setting. For instance, the `resolve` function for the build plate temperature is `max(extruderValues('material_bed_temperature')`, meaning that it will use the hottest bed temperature of all materials of the extruders in use.
* `limit_to_extruder` (optional): A Python expression that indicates which extruder a setting will be obtained from. This is used for settings that may be extruder-specific but the extruder is not necessarily the current extruder. For instance, support settings need to be evaluated for the support extruder. Infill settings need to be evaluated for the infill extruder if the infill extruder is changed.
* `enabled` (optional string or boolean): Whether the setting can currently be made visible for the user. This can be a simple true/false, or a Python expression that depends on other settings. Typically used for settings that don't apply when another setting is disabled, such as to hide the support settings if support is disabled.
* `minimum_value` (optional): The lowest acceptable value for this setting. If it's any lower, Cura will not allow the user to slice. By convention this is used to prevent setting values that are technically or physically impossible, such as a layer height of 0mm. This property only applies to numerical settings.
* `maximum_value` (optional): The highest acceptable value for this setting. If it's any higher, Cura will not allow the user to slice. By convention this is used to prevent setting values that are technically or physically impossible, such as a support overhang angle of more than 90 degrees. This property only applies to numerical settings.
* `minimum_value_warning` (optional): The threshold under which a warning is displayed to the user. By convention this is used to indicate that it will probably not print very nicely with such a low setting value. This property only applies to numerical settings.
* `maximum_value_warning` (optional): The threshold above which a warning is displayed to the user. By convention this is used to indicate that it will probably not print very nicely with such a high setting value. This property only applies to numerical settings.
* `settable_globally` (optional boolean): Whether the setting can be changed globally. For some mesh-type settings such as `support_mesh` this doesn't make sense, so those can't be changed globally. They are not displayed in the main settings list then.
* `settable_per_meshgroup` (optional boolean): Whether a setting can be changed per group of meshes. Currently unused in Cura.
* `settable_per_extruder` (optional boolean): Whether a setting can be changed per extruder. Some settings, like the build plate temperature, can't be adjusted separately for each extruder. An icon is shown in the interface to indicate this. If the user changes these settings they are stored in the global stack.
* `settable_per_mesh` (optional boolean): Whether a setting can be changed per mesh. The settings that can be changed per mesh are shown in the list of available settings in the per-object settings tool.
* `children` (optional list): A list of child settings. These are displayed with an indentation. If all child settings are overridden by the user, the parent setting gets greyed out to indicate that the parent setting has no effect any more. This is not strictly always the case though, because that would depend on the inheritance functions in the `value`.
* `icon` (optional string): A path to an icon to be displayed. Only applies to setting categories.
* `allow_empty` (optional bool): Whether the setting is allowed to be empty. If it's not, this will be treated as a setting error and Cura will not allow the user to slice. Only applies to string-type settings.
* `warning_description` (optional string): A warning message to display when the setting has a warning value. This is currently unused by Cura.
* `error_description` (optional string): An error message to display when the setting has an error value. This is currently unused by Cura.
* `options` (dictionary): A list of values that the user can choose from. The keys of this dictionary are keys that CuraEngine identifies the option with. The values are human-readable strings and will be translated. Only applies to (and only required for) enum-type settings.
* `comments` (optional string): Comments to other programmers about the setting. This is not used by Cura.
* `is_uuid` (optional boolean): Whether or not this setting indicates a UUID-4. If it is, the setting will indicate an error if it's not in the correct format. Only applies to string-type settings.
* `regex_blacklist_pattern` (optional string): A regular expression, where if the setting value matches with this regular expression, it gets an error state. Only applies to string-type settings.
* `error_value` (optional): If the setting value is equal to this value, it will show a setting error. This is used to display errors for non-numerical settings such as checkboxes.
* `warning_value` (optional): If the setting value is equal to this value, it will show a setting warning. This is used to display warnings for non-numerical settings such as checkboxes.