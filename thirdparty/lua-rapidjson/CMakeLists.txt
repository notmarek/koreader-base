list(APPEND PATCH_CMD COMMAND ${KO_PATCH} ${CMAKE_CURRENT_SOURCE_DIR}/cmake_tweaks.patch)

list(APPEND CMAKE_ARGS
    -DCMAKE_TOOLCHAIN_FILE=${CMAKE_TOOLCHAIN_FILE}
    -DLUA_INCLUDE_DIR=${LUAJIT_INC}
    -DLUA_LIBRARIES=${LUAJIT_LIB}
)

list(APPEND BUILD_CMD COMMAND ${KO_MAKE_PROGRAM})

append_binary_install_command(INSTALL_CMD rapidjson.so DESTINATION common)

ko_write_gitclone_script(
    GIT_CLONE_SCRIPT_FILENAME
    https://github.com/NiLuJe/lua-rapidjson
    c082a876f15536ad3b7d305b6db3b128a8dc7445
    ${SOURCE_DIR}
)

external_project(
    DOWNLOAD_COMMAND ${CMAKE_COMMAND} -P ${GIT_CLONE_SCRIPT_FILENAME}
    PATCH_COMMAND ${PATCH_CMD}
    CMAKE_ARGS ${CMAKE_ARGS}
    BUILD_COMMAND ${BUILD_CMD}
    INSTALL_COMMAND ${INSTALL_CMD}
)
