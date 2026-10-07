# Unified Anland display producer and DRM-like userspace device layer.
# This is the single public producer-side library used by WM backends.
include("${CMAKE_CURRENT_LIST_DIR}/presentation.cmake")
set(DISPLAY_PRODUCER_SOURCES
    display_producer.c
    anland_audio.c
    anland_camera.c
    anland_device.c
    anland_buffer_registry.c)

add_library(display_producer SHARED ${DISPLAY_PRODUCER_SOURCES}
    $<TARGET_OBJECTS:anland_presentation>)
set_target_properties(display_producer PROPERTIES
    VERSION ${PROJECT_VERSION}
    SOVERSION 5)
target_include_directories(display_producer PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/../common>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/anland>)
target_link_libraries(display_producer PRIVATE socket_utils pthread PkgConfig::PIPEWIRE)

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/../display-producer.pc.in display-producer.pc @ONLY)
install(TARGETS display_producer
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR})
install(FILES
    display_producer.h
    anland_audio.h
    anland_camera.h
    anland_buffer_registry.h
    anland_de_backend.h
    anland_device.h
    anland_present.h
    anland_scene.h
    anland_scene_legacy.h
    anland_window_map.h
    ../common/protocol.h
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/anland)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/display-producer.pc
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig)
