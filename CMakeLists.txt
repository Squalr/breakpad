# CMake wrapper for Google Breakpad
cmake_minimum_required(VERSION 3.8 FATAL_ERROR)
project(breakpad-cmake VERSION 0.0.1)

# breakpad_client must be build static (shared library calls ARE BAD in exception land)
add_library(breakpad_client STATIC
    $<$<PLATFORM_ID:Linux>:${CMAKE_CURRENT_LIST_DIR}/src/client/linux/crash_generation/crash_generation_client.cc>
    $<$<PLATFORM_ID:Linux>:${CMAKE_CURRENT_LIST_DIR}/src/client/linux/crash_generation/crash_generation_server.cc>
    $<$<PLATFORM_ID:Linux>:${CMAKE_CURRENT_LIST_DIR}/src/client/linux/dump_writer_common/thread_info.cc>
    $<$<PLATFORM_ID:Linux>:${CMAKE_CURRENT_LIST_DIR}/src/client/linux/dump_writer_common/ucontext_reader.cc>
    $<$<PLATFORM_ID:Linux>:${CMAKE_CURRENT_LIST_DIR}/src/client/linux/handler/exception_handler.cc>
    $<$<PLATFORM_ID:Linux>:${CMAKE_CURRENT_LIST_DIR}/src/client/linux/handler/minidump_descriptor.cc>
    $<$<PLATFORM_ID:Linux>:${CMAKE_CURRENT_LIST_DIR}/src/client/linux/log/log.cc>
    $<$<PLATFORM_ID:Linux>:${CMAKE_CURRENT_LIST_DIR}/src/client/linux/microdump_writer/microdump_writer.cc>
    $<$<PLATFORM_ID:Linux>:${CMAKE_CURRENT_LIST_DIR}/src/client/linux/minidump_writer/linux_core_dumper.cc>
    $<$<PLATFORM_ID:Linux>:${CMAKE_CURRENT_LIST_DIR}/src/client/linux/minidump_writer/linux_dumper.cc>
    $<$<PLATFORM_ID:Linux>:${CMAKE_CURRENT_LIST_DIR}/src/client/linux/minidump_writer/linux_ptrace_dumper.cc>
    $<$<PLATFORM_ID:Linux>:${CMAKE_CURRENT_LIST_DIR}/src/client/linux/minidump_writer/minidump_writer.cc>
    $<$<PLATFORM_ID:Linux>:${CMAKE_CURRENT_LIST_DIR}/src/common/linux/elf_core_dump.cc>
    $<$<PLATFORM_ID:Linux>:${CMAKE_CURRENT_LIST_DIR}/src/common/linux/elfutils.cc>
    $<$<PLATFORM_ID:Linux>:${CMAKE_CURRENT_LIST_DIR}/src/common/linux/file_id.cc>
    $<$<PLATFORM_ID:Linux>:${CMAKE_CURRENT_LIST_DIR}/src/common/linux/guid_creator.cc>
    $<$<PLATFORM_ID:Linux>:${CMAKE_CURRENT_LIST_DIR}/src/common/linux/linux_libc_support.cc>
    $<$<PLATFORM_ID:Linux>:${CMAKE_CURRENT_LIST_DIR}/src/common/linux/memory_mapped_file.cc>
    $<$<PLATFORM_ID:Linux>:${CMAKE_CURRENT_LIST_DIR}/src/common/linux/safe_readlink.cc>

    ${CMAKE_CURRENT_LIST_DIR}/src/client/minidump_file_writer.cc

    # EVIL cocos already uses the symbol so we'll use it's implementation OH MY GERD!!!
    #${CMAKE_CURRENT_LIST_DIR}/src/common/convert_UTF.c

    ${CMAKE_CURRENT_LIST_DIR}/src/common/md5.cc
    ${CMAKE_CURRENT_LIST_DIR}/src/common/string_conversion.cc
)

# be a good boy and actually check these at some point
#define HAVE_A_OUT_H
#define HAVE_CXX11
#define HAVE_GETRANDOM
#define HAVE_INTTYPES_H
#define HAVE_MEMORY_H
#define HAVE_PTHREAD
#define HAVE_STDINT_H
#define HAVE_STDLIB_H
#define HAVE_STRINGS_H
#define HAVE_STRING_H
#define HAVE_SYS_RANDOM_H
#define HAVE_SYS_STAT_H
#define HAVE_SYS_TYPES_H
#define HAVE_UNISTD_H

# add definitions related to system configuration (forced for now, MUST check these eventually)
target_compile_definitions(breakpad_client PUBLIC
    $<$<PLATFORM_ID:Linux>:HAVE_A_OUT_H>
    $<$<PLATFORM_ID:Linux>:HAVE_CXX11>
    $<$<PLATFORM_ID:Linux>:HAVE_GETRANDOM>
    $<$<PLATFORM_ID:Linux>:HAVE_INTTYPES_H>
    $<$<PLATFORM_ID:Linux>:HAVE_MEMORY_H>
    $<$<PLATFORM_ID:Linux>:HAVE_PTHREAD>
    $<$<PLATFORM_ID:Linux>:HAVE_STDINT_H>
    $<$<PLATFORM_ID:Linux>:HAVE_STDLIB_H>
    $<$<PLATFORM_ID:Linux>:HAVE_STRINGS_H>
    $<$<PLATFORM_ID:Linux>:HAVE_STRING_H>
    $<$<PLATFORM_ID:Linux>:HAVE_SYS_RANDOM_H>
    $<$<PLATFORM_ID:Linux>:HAVE_SYS_STAT_H>
    $<$<PLATFORM_ID:Linux>:HAVE_SYS_TYPES_H>
    $<$<PLATFORM_ID:Linux>:HAVE_UNISTD_H  >
)

# include public interface
target_include_directories(breakpad_client PUBLIC
    ${CMAKE_CURRENT_LIST_DIR}/src
)
