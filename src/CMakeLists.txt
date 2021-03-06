# Add all your dependencies to the list below
pkg_check_modules (DEPS REQUIRED glib-2.0 switchboard-2.0 granite>=0.5 gtk+-3.0>=3.14 gexiv2 gnome-desktop-3.0 plank>=0.7.1.1137)

pkg_check_modules(PLANK011 QUIET plank>=0.10.9)
if (PLANK011_FOUND)
  set (PLANK_OPTIONS --define=HAVE_PLANK_0_11)
endif ()

add_definitions (${DEPS_CFLAGS})
link_libraries (${DEPS_LIBRARIES})
link_directories (${DEPS_LIBRARY_DIRS})

find_package (Vala REQUIRED)
include (ValaVersion)
ensure_vala_version ("0.22.0" MINIMUM)

include (ValaPrecompile)
# Add all your vala files and requires packages to the List below to include them in the build
vala_precompile (VALA_C ${PLUGNAME}
    ThumbnailGenerator.vala
    desktop-plug.vala
    GalaSettings.vala
    Views/Dock.vala
    Views/HotCorners.vala
    Views/Wallpaper.vala
    Widgets/SolidColorContainer.vala
    Widgets/WallpaperContainer.vala
    ${CMAKE_CURRENT_BINARY_DIR}/config.vala
PACKAGES
    gnome-desktop-3.0
    switchboard-2.0
    granite
    gexiv2
    posix
    plank
OPTIONS
    --thread
    --vapidir=${CMAKE_SOURCE_DIR}/vapi
    ${PLANK_OPTIONS}
)

add_library (${PLUGNAME} MODULE ${VALA_C})
target_link_libraries(${PLUGNAME} m)

# Installation
install (TARGETS ${PLUGNAME} DESTINATION ${PKGDATADIR})
