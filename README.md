# Audio Programmer CPM Recipes

Collection of CMake - CPM recipes for libraries commonly used in audio development

## JUCE
```cmake
    CPMAddPackage(
        NAME JUCE
        GITHUB_REPOSITORY juce-framework/JUCE
        GIT_TAG origin/master
    )
```

You can also pass options directly to JUCE from the CPM call
```cmake
    CPMAddPackage (NAME JUCE
       GITHUB_REPOSITORY juce-framework/JUCE
       GIT_TAG origin/master
       OPTIONS 
       "JUCE_ENABLE_MODULE_SOURCE_GROUPS ON"
       "JUCE_BUILD_EXAMPLES OFF"
       "JUCE_BUILD_EXTRAS OFF")
```
               
See below for some JUCE Modules.

## Ableton Link
```cmake
    CPMAddPackage(
        NAME link
        GIT_TAG origin/master
        GITHUB_REPOSITORY Ableton/link
    )
```

## Doctest - Unit testing library
```cmake
    CPMAddPackage(
        NAME doctest
        GIT_TAG 2.4.6
        GITHUB_REPOSITORY onqtam/doctest
    )
```
## JSON
```cmake
    CPMAddPackage(
        NAME nlohmann_json
        GITHUB_REPOSITORY nlohmann/json
        VERSION 3.9.1
        OPTIONS 
            "JSON_BuildTests OFF"
    )
```
## YAML
```cmake
    CPMAddPackage(
        NAME yaml-cpp
        GITHUB_REPOSITORY jbeder/yaml-cpp
        GIT_TAG origin/master
        OPTIONS "YAML_CPP_INSTALL NO" "YAML_CPP_BUILD_TOOLS NO" "YAML_CPP_BUILD_TESTS NO" "YAML_CPP_CLANG_FORMAT_EXE NO")     
```

## Lock Free Queue - MoodyCamel
```cmake
    CPMAddPackage(
        NAME readerwriterqueue
        GITHUB_REPOSITORY cameron314/readerwriterqueue
        GIT_TAG origin/master
    )
```
## MTS-ESP Tuning Library
```cmake
    CPMAddPackage (NAME MTS-ESP 
               GITHUB_REPOSITORY ODDSound/MTS-ESP 
               GIT_TAG origin/main)

    target_include_directories (<YOUR_TARGET> INTERFACE "${MTS-ESP_SOURCE_DIR}/Client")
```
## Steinberg ASIO SDK

This isn't via CPM, instead it downloads the ASIO SDK Zip file and extracts it into the source tree at Libs/include/asiosdk

call the macro from your CMakeLists.txt file using a the filename_version_date format. e.g.

and set the JUCE_ASIO flag on your target

```cmake
target_compile_definitions(yourTarget
    PUBLIC
    JUCE_ASIO=1
)
```

getAndIncludeASIOSDK(asiosdk_2.3.3_2019-06-14)

```cmake
    macro(getAndIncludeASIOSDK ASIO_SDK_VERSION)

        list(APPEND CMAKE_MESSAGE_INDENT "  ")

        if(NOT EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/Libs/include/asiosdk/common)
            file(
                MAKE_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/Libs/include/asiosdk
            )
            message("Fetching ASIO SDK")
            file(
                DOWNLOAD https://download.steinberg.net/sdk_downloads/${ASIO_SDK_VERSION}.zip "${CMAKE_CURRENT_BINARY_DIR}/asiosdk.zip"
                TIMEOUT 60
            )
            message("Extracting ASIO SDK")
            file(ARCHIVE_EXTRACT INPUT "${CMAKE_CURRENT_BINARY_DIR}/asiosdk.zip")

            file(
                RENAME ${CMAKE_CURRENT_BINARY_DIR}/${ASIO_SDK_VERSION}/common ${CMAKE_CURRENT_SOURCE_DIR}/Libs/include/asiosdk/common
            )
            # tidy up the download and extracted files that no longer needed
            file(
                REMOVE ${CMAKE_CURRENT_BINARY_DIR}/asiosdk.zip
            )
            file(
                REMOVE_RECURSE ${CMAKE_CURRENT_BINARY_DIR}/${ASIO_SDK_VERSION}
            )
        endif()
        include_directories(Libs/include/asiosdk/common)
    endmacro(getAndIncludeASIOSDK)
```
## RxCpp
```cmake
    CPMAddPackage(
        NAME RxCpp
        VERSION 4.1.0
        GITHUB_REPOSITORY ReactiveX/RxCpp
        DOWNLOAD_ONLY YES
    )

    if(RxCpp_ADDED)
        # We only want the headers from Rx/v2/src
        add_library(RxCpp INTERFACE IMPORTED)
        set(RxSourceDir "${RxCpp_SOURCE_DIR}/Rx/v2/src")
        target_include_directories(RxCpp INTERFACE ${RxSourceDir})

    endif()
```
# JUCE Modules

## Raw Keyboard Input JUCE Module
```cmake
    CPMAddPackage (
        NAME raw_keyboard_module
        GITHUB_REPOSITORY izzyreal/juce-raw-keyboard-input-module
        GIT_TAG origin/main
    )
```
Then link against the `raw_keyboard_input` target.
```cmake
    target_link_libraries(MyApp
        PRIVATE
        raw_keyboard_input
```
## Foleys Gui Magic
```cmake
    CPMAddPackage (
        NAME PluginGuiMagic
        GITHUB_REPOSITORY ffAudio/PluginGuiMagic
        GIT_TAG origin/main
        DOWNLOAD_ONLY YES
    )

    juce_add_module(${PluginGuiMagic_SOURCE_DIR}/modules/foleys_gui_magic)
```    
Then link against the `foleys_gui_magic` target
```cmake
    target_link_libraries(MyApp
        PRIVATE
        foleys_gui_magic
```
