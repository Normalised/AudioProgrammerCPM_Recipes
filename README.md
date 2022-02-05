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
