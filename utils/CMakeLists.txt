
add_executable(sofaexamples sofaexamples.cpp)
add_executable(sofainfo sofainfo.cpp)
add_executable(sofamisc sofamisc.cpp)

# Does not compile, maybe pulled by mistake in some pull request? 
# add_executable(json2sofa json2sofa.cpp)
add_executable(sofa2json sofa2json.cpp)


target_link_libraries(sofaexamples libsofa)
target_link_libraries(sofainfo libsofa)
target_link_libraries(sofamisc libsofa)

# Some of the tests require json-c
find_library(JSON_LIB NAMES libjson-c.a json-c.lib)
find_path(JSON_HEADERS NAMES json-c)

#target_include_directories(json2sofa SYSTEM 
#    PUBLIC ${JSON_HEADERS})
target_include_directories(sofa2json SYSTEM 
    PUBLIC ${JSON_HEADERS})

#target_link_libraries(json2sofa 
#    PUBLIC libsofa
#    PUBLIC ${JSON_LIB})
target_link_libraries(sofa2json 
    PUBLIC libsofa
    PUBLIC ${JSON_LIB})

