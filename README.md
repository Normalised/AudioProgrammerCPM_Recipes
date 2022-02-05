# Audio Programmer CPM Recipes

Collection of CMake - CPM recipes for libraries commonly used in audio development

## JUCE

    CPMAddPackage(
        NAME JUCE
        GITHUB_REPOSITORY juce-framework/JUCE
        GIT_TAG origin/master
    )

## Ableton Link

    CPMAddPackage(
        NAME link
        GIT_TAG origin/master
        GITHUB_REPOSITORY Ableton/link
    )

## Doctest - Unit testing library

    CPMAddPackage(
        NAME doctest
        GIT_TAG 2.4.6
        GITHUB_REPOSITORY onqtam/doctest
    )
    
## JSON

    CPMAddPackage(
        NAME nlohmann_json
        GITHUB_REPOSITORY nlohmann/json
        VERSION 3.9.1
        OPTIONS 
            "JSON_BuildTests OFF"
    )

## YAML

    CPMAddPackage(
        NAME yaml-cpp
        GITHUB_REPOSITORY jbeder/yaml-cpp
        GIT_TAG origin/master
        OPTIONS "YAML_CPP_INSTALL NO" "YAML_CPP_BUILD_TOOLS NO" "YAML_CPP_BUILD_TESTS NO" "YAML_CPP_CLANG_FORMAT_EXE NO")
        
        
## Lock Free Queue - MoodyCamel

    CPMAddPackage(
        NAME readerwriterqueue
        GITHUB_REPOSITORY cameron314/readerwriterqueue
        GIT_TAG origin/master
    )
    

## Steinberg ASIO SDK

This isn't via CPM, instead it downloads the ASIO SDK Zip file and extracts it into the source tree at Libs/include/asiosdk

call the macro from your CMakeLists.txt file using a the filename_version_date format. e.g.

getAndIncludeASIOSDK(asiosdk_2.3.3_2019-06-14)


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

## RxCpp

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
