cmake_minimum_required(VERSION 3.16)
project(SQLiteLibrary VERSION 3.49.0  LANGUAGES C)

# 设置C标准
set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED ON)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

option(BUILD_SHARED_LIBS "Build the shared library, Default is ON" ON)

include_directories(${CMAKE_SOURCE_DIR}/include)

# 添加 SQLite 的 amalgamation 源文件
set(SQLITE_SOURCES ${CMAKE_SOURCE_DIR}/src/sqlite3.c)
set(SQLITE_CMD_SOURCES ${CMAKE_SOURCE_DIR}/src/shell.c ${CMAKE_SOURCE_DIR}/src/sqlite3.c)

add_executable(sqlite3-cli ${SQLITE_CMD_SOURCES})

if (${BUILD_SHARED_LIBS})
        # 创建动态库
        add_library(sqlite3 SHARED ${SQLITE_SOURCES})

        # 设置库的版本号
        set_target_properties(sqlite3 PROPERTIES
                VERSION ${PROJECT_VERSION}
                SOVERSION 3
        )

        # 安装库和头文件
        install(TARGETS sqlite3
                LIBRARY DESTINATION lib
                ARCHIVE DESTINATION lib
        )

        install(TARGETS 
                        sqlite3
                LIBRARY DESTINATION 
                        ${CMAKE_INSTALL_PREFIX}/lib
                ARCHIVE DESTINATION 
                        ${CMAKE_INSTALL_PREFIX}/lib
        )
endif()

#install(FILES sqlite3.h sqlite3ext.h
#        DESTINATION include
#)
install(FILES 
                ${CMAKE_SOURCE_DIR}/include/sqlite3.h 
                ${CMAKE_SOURCE_DIR}/include/sqlite3ext.h
        DESTINATION 
                ${CMAKE_INSTALL_PREFIX}/include/sqlite)
install(TARGETS 
                sqlite3-cli
        DESTINATION 
                ${CMAKE_INSTALL_PREFIX}/bin
)