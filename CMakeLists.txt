cmake_minimum_required(VERSION 3.20)
project(gumbo C CXX)

set(CMAKE_C_FLAGS "-Wall")
set(CMAKE_LD_FLAGS "-no-undefined")
add_library(gumbo)
target_sources(gumbo
	PRIVATE
		src/attribute.c
		src/attribute.h
		src/char_ref.c
		src/char_ref.h
		src/error.c
		src/error.h
		src/insertion_mode.h
		src/parser.c
		src/parser.h
		src/string_buffer.c
		src/string_buffer.h
		src/string_piece.c
		src/string_piece.h
		src/tag.c
		src/tag_enum.h
		src/tag_gperf.h
		src/tag_strings.h
		src/tag_sizes.h
		src/token_type.h
		src/tokenizer.c
		src/tokenizer.h
		src/tokenizer_states.h
		src/utf8.c
		src/utf8.h
		src/util.c
		src/util.h
		src/vector.c
		src/vector.h
)
target_sources(gumbo
	INTERFACE
	src/gumbo.h
	src/tag_enum.h
	)

#pkgconfigdir = $(libdir)/pkgconfig
#pkgconfig_DATA = gumbo.pc

#check_PROGRAMS = gumbo_test
#TESTS = gumbo_test
#gumbo_test_CPPFLAGS = \
#				-I"$(srcdir)/." \
#				-I"$(srcdir)/src" \
#				-I"$(srcdir)/gtest/include"
add_executable(test_cases
	tests/attribute.cc
	tests/char_ref.cc
	tests/parser.cc
	tests/string_buffer.cc
	tests/string_piece.cc
	tests/tokenizer.cc
	tests/test_utils.cc
	tests/utf8.cc
	tests/vector.cc
)
target_include_directories(test_cases PRIVATE src)
find_package(GTest REQUIRED)
target_link_libraries(test_cases PRIVATE GTest::GTest GTest::Main gumbo)
enable_testing()
add_test(gumbo_tests test_cases)

add_executable(clean_text examples/clean_text.cc)
target_link_libraries(clean_text gumbo)
target_include_directories(clean_text PUBLIC src)

add_executable(find_links examples/find_links.cc)
target_link_libraries(find_links gumbo)
target_include_directories(find_links PUBLIC src)

add_executable(get_title examples/get_title.c)
target_link_libraries(get_title gumbo)
target_include_directories(get_title PUBLIC src)

add_executable(positions_of_class examples/positions_of_class.cc)
target_link_libraries(positions_of_class gumbo)
target_include_directories(positions_of_class PUBLIC src)

add_executable(benchmark benchmarks/benchmark.cc)
target_link_libraries(benchmark gumbo)
target_include_directories(benchmark PUBLIC src)

add_executable(serialize examples/serialize.cc)
target_link_libraries(serialize gumbo)
target_include_directories(serialize PUBLIC src)

add_executable(prettyprint examples/prettyprint.cc)
target_link_libraries(prettyprint gumbo)
target_include_directories(prettyprint PUBLIC src)

