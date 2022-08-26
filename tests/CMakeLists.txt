if(WITH_ARITH)
	add_executable(canonicity_test canonicity_test.cpp)
	target_setup(canonicity_test)
	target_link_libraries(canonicity_test ${OBJECT_LIB_NAME})

	add_executable(cache_test cache_test.cpp)
	target_setup(cache_test)
	target_link_libraries(cache_test ${OBJECT_LIB_NAME})
endif()
