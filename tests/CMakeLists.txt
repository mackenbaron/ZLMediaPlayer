aux_source_directory(. TEST_SRC_LIST)

foreach(TEST_SRC ${TEST_SRC_LIST})
STRING(REGEX REPLACE ".cpp" "" TEST_EXE_NAME ${TEST_SRC})
STRING(REGEX REPLACE "./" "" TEST_EXE_NAME ${TEST_EXE_NAME})
message(STATUS "add test program:${TEST_EXE_NAME}")
add_executable(${TEST_EXE_NAME} ${TEST_SRC})
if(ANDROID)
target_link_libraries(${TEST_EXE_NAME} ${CMAKE_PROJECT_NAME}_static ${LINK_LIB_LIST})
elseif(WIN32)
target_link_libraries(${TEST_EXE_NAME} ${CMAKE_PROJECT_NAME}_static ${LINK_LIB_LIST} WS2_32 Iphlpapi shlwapi)
else()
target_link_libraries(${TEST_EXE_NAME} ${CMAKE_PROJECT_NAME}_shared ${LINK_LIB_LIST} pthread)
endif()

if(WIN32)
#set_target_properties(${TEST_EXE_NAME} PROPERTIES COMPILE_FLAGS "/wd4819 /wd4996" )
#set_target_properties(${TEST_EXE_NAME} PROPERTIES LINK_FLAGS "/SAFESEH:NO /SUBSYSTEM:WINDOWS" )
endif(WIN32)

endforeach(TEST_SRC ${TEST_SRC_LIST})
