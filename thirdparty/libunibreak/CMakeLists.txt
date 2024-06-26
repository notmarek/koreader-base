list(APPEND CFG_CMD COMMAND env)
append_autotools_vars(CFG_CMD)
list(APPEND CFG_CMD
    ${SOURCE_DIR}/configure --host=${CHOST} --prefix=${STAGING_DIR}
    --disable-static --enable-shared
    --enable-silent-rules
)

if(ANDROID)
    list(APPEND CFG_CMD COMMAND ${ANDROID_LIBTOOL_FIX_CMD})
endif()

list(APPEND BUILD_CMD COMMAND ${KO_MAKE_RECURSIVE})

list(APPEND INSTALL_CMD COMMAND ${KO_MAKE_RECURSIVE} install)

set(LIB_SPEC unibreak VERSION 6)
if(DARWIN)
    append_shared_lib_fix_commands(INSTALL_CMD ${LIB_SPEC} ID)
endif()
append_shared_lib_install_commands(INSTALL_CMD ${LIB_SPEC})

external_project(
    URL https://github.com/adah1972/libunibreak/releases/download/libunibreak_6_1/libunibreak-6.1.tar.gz
    URL_MD5 8df410d010e03de1a339a400a920335e
    CONFIGURE_COMMAND ${CFG_CMD}
    BUILD_COMMAND ${BUILD_CMD}
    INSTALL_COMMAND ${INSTALL_CMD}
)
