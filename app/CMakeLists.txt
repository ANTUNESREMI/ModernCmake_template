set(EXECUTABLE_OUTPUT_PATH ${MyProject_SOURCE_DIR})

add_executable( exe )

target_sources( exe
    PRIVATE 
        "main.cpp" 
) 

target_link_libraries( exe
    PRIVATE 
        Source::source
)