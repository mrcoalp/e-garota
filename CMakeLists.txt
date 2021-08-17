cmake_minimum_required(VERSION 3.15)

set(PLUGIN_NAME "e-garota")

project(${PLUGIN_NAME} VERSION 0.0.1)

set(JUCE_SOURCE_DIR "${CMAKE_CURRENT_LIST_DIR}/JUCE")

add_subdirectory(JUCE)

juce_add_plugin(${PLUGIN_NAME}
        # VERSION ...                               # Set this if the plugin version is different to the project version
        # ICON_BIG ...                              # ICON_* arguments specify a path to an image file to use as an icon for the Standalone
        # ICON_SMALL ...
        # COMPANY_NAME ...                          # Specify the name of the plugin's author
        # IS_SYNTH TRUE/FALSE                       # Is this a synth or an effect?
        # NEEDS_MIDI_INPUT TRUE/FALSE               # Does the plugin need midi input?
        # NEEDS_MIDI_OUTPUT TRUE/FALSE              # Does the plugin need midi output?
        # IS_MIDI_EFFECT TRUE/FALSE                 # Is this plugin a MIDI effect?
        # EDITOR_WANTS_KEYBOARD_FOCUS TRUE/FALSE    # Does the editor need keyboard focus?
        # COPY_PLUGIN_AFTER_BUILD TRUE/FALSE        # Should the plugin be installed to a default location after building?
        PLUGIN_MANUFACTURER_CODE MRCI               # A four-character manufacturer id with at least one upper-case character
        PLUGIN_CODE Egrt                            # A unique four-character plugin id with exactly one upper-case character
        # GarageBand 10.3 requires the first letter to be upper-case, and the remaining letters to be lower-case
        FORMATS AU VST3 Standalone                  # The formats to build. Other valid formats are: AAX Unity VST AU AUv3
        PRODUCT_NAME "E-Garota")                    # The name of the final executable, which can differ from the target name

target_sources(${PLUGIN_NAME}
        PRIVATE
        src/PluginEditor.cpp
        src/PluginProcessor.cpp)

target_include_directories(${PLUGIN_NAME} PRIVATE include)

set_property(TARGET ${PLUGIN_NAME} PROPERTY CXX_STANDARD 17)

target_compile_definitions(${PLUGIN_NAME}
        PUBLIC
        # JUCE_WEB_BROWSER and JUCE_USE_CURL would be on by default, but you might not need them.
        JUCE_WEB_BROWSER=0  # If you remove this, add `NEEDS_WEB_BROWSER TRUE` to the `juce_add_plugin` call
        JUCE_USE_CURL=0     # If you remove this, add `NEEDS_CURL TRUE` to the `juce_add_plugin` call
        JUCE_VST3_CAN_REPLACE_VST2=0)

target_link_libraries(${PLUGIN_NAME}
        PRIVATE
        # AudioPluginData           # If we'd created a binary data target, we'd link to it here
        juce::juce_audio_utils
        PUBLIC
        juce::juce_recommended_config_flags
        juce::juce_recommended_lto_flags
        juce::juce_recommended_warning_flags)
