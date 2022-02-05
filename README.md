# Audio Programmer CPM Recipes

Collection of CMake - CPM recipes for libraries commonly used in audio development

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
    
