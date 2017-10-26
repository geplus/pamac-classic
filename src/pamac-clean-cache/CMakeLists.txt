### CMakeLists automatically created with AutoVala
### Do not edit

set (DATADIR "${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_DATAROOTDIR}")
set (PKGDATADIR "${DATADIR}/pamac")
set (GETTEXT_PACKAGE "pamac")
set (RELEASE_NAME "pamac")
set (CMAKE_C_FLAGS "")
set (PREFIX ${CMAKE_INSTALL_PREFIX})
set (VERSION "6.3.3")
set (TESTSRCDIR "${CMAKE_SOURCE_DIR}")
set (DOLLAR "$")

configure_file (${CMAKE_SOURCE_DIR}/src/pamac-clean-cache/Config.vala.base ${CMAKE_BINARY_DIR}/src/pamac-clean-cache/Config.vala)
add_definitions(-DGETTEXT_PACKAGE=\"${GETTEXT_PACKAGE}\")
set (VERSION "6.3.3")
add_definitions (${DEPS_CFLAGS})
include_directories ( ${CMAKE_BINARY_DIR}/src )
link_libraries ( ${DEPS_LIBRARIES} -lPamac )
link_directories ( ${DEPS_LIBRARY_DIRS} ${CMAKE_BINARY_DIR}/src )
find_package (Vala REQUIRED)
include (ValaVersion)
ensure_vala_version ("0.38" MINIMUM)
include (ValaPrecompile)

set (VALA_PACKAGES ${VALA_PACKAGES} posix)
set (VALA_PACKAGES ${VALA_PACKAGES} gtk+-3.0)
set (VALA_PACKAGES ${VALA_PACKAGES} gio-2.0)
set (VALA_PACKAGES ${VALA_PACKAGES} glib-2.0)
set (VALA_PACKAGES ${VALA_PACKAGES} gobject-2.0)

set (APP_SOURCES ${APP_SOURCES} ${CMAKE_CURRENT_BINARY_DIR}/Config.vala)
set (APP_SOURCES ${APP_SOURCES} clean_cache.vala)
set (APP_SOURCES ${APP_SOURCES} pamac_config.vala)

set (CUSTOM_VAPIS_LIST ${CUSTOM_VAPIS_LIST} ${CMAKE_BINARY_DIR}/src/Pamac.vapi)

if (DISABLE_AUR)
	set (COMPILE_OPTIONS ${COMPILE_OPTIONS} -D DISABLE_AUR)
	set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DDISABLE_AUR " )
	set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DDISABLE_AUR " )
endif ()
if (KDE_TRAY)
	set (COMPILE_OPTIONS ${COMPILE_OPTIONS} -D KDE_TRAY)
	set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DKDE_TRAY " )
	set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DKDE_TRAY " )
endif ()
set (COMPILE_OPTIONS ${COMPILE_OPTIONS} --vapidir=${CMAKE_SOURCE_DIR}/src/vapis )

if ((${CMAKE_BUILD_TYPE} STREQUAL "Debug") OR (${CMAKE_BUILD_TYPE} STREQUAL "RelWithDebInfo"))
	set(COMPILE_OPTIONS ${COMPILE_OPTIONS} "-g")
endif()


vala_precompile(VALA_C pamac-clean-cache
	${APP_SOURCES}
PACKAGES
	${VALA_PACKAGES}
CUSTOM_VAPIS
	${CUSTOM_VAPIS_LIST}
OPTIONS
	${COMPILE_OPTIONS}
)

add_executable(pamac-clean-cache ${VALA_C})
set ( pamac-clean-cache_DEPENDENCIES ${pamac-clean-cache_DEPENDENCIES} Pamac )
add_dependencies( pamac-clean-cache ${pamac-clean-cache_DEPENDENCIES} )


install(TARGETS
	pamac-clean-cache
RUNTIME DESTINATION
	${CMAKE_INSTALL_BINDIR}
)
if(HAVE_VALADOC)
	valadoc(pamac-clean-cache
		${CMAKE_BINARY_DIR}/valadoc/pamac-clean-cache
		${APP_SOURCES}
	PACKAGES
		${VALA_PACKAGES}
	CUSTOM_VAPIS
		${CUSTOM_VAPIS_LIST}
	)
	install(DIRECTORY
		${CMAKE_BINARY_DIR}/valadoc
	DESTINATION
		${CMAKE_INSTALL_DATAROOTDIR}/doc/pamac
	)
endif()