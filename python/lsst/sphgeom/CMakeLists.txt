find_package(pybind11 REQUIRED)

FUNCTION(SPHGEOM_PYBIND_MODULES)
    FOREACH(WRAPPER IN ITEMS ${ARGV})
        get_filename_component(NAME ${WRAPPER} NAME_WE)
        set(MODULE ${NAME}_sphgeom_pybind)
        pybind11_add_module(${MODULE} ${WRAPPER})
        target_link_libraries(${MODULE} PUBLIC sphgeom)
        set_target_properties(${MODULE} PROPERTIES OUTPUT_NAME ${NAME})
        install(TARGETS ${MODULE} DESTINATION ${CMAKE_INSTALL_PREFIX}/python/lsst/sphgeom)
    ENDFOREACH()
ENDFUNCTION()

sphgeom_pybind_modules(
    _angle.cc
    _angleInterval.cc
    _box3d.cc
    _box.cc
    _chunker.cc
    _circle.cc
    _convexPolygon.cc
    _curve.cc
    _ellipse.cc
    _htmPixelization.cc
    _interval1d.cc
    _lonLat.cc
    _matrix3d.cc
    _mq3cPixelization.cc
    _normalizedAngle.cc
    _normalizedAngleInterval.cc
    _orientation.cc
    _pixelization.cc
    _q3cPixelization.cc
    _rangeSet.cc
    _region.cc
    _relationship.cc
    _unitVector3d.cc
    _utils.cc
    _vector3d.cc
)

install(
    DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ DESTINATION ${CMAKE_INSTALL_PREFIX}/python/lsst/sphgeom
    PATTERN "CMakeLists.txt" EXCLUDE
    PATTERN "SConscript" EXCLUDE
    PATTERN "*.cc" EXCLUDE
)

file(GENERATE OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/version.py CONTENT "\
__version__ = 'unknown'
__all__ = ('__version__',)
")

install(FILES ${CMAKE_CURRENT_BINARY_DIR}/version.py DESTINATION ${CMAKE_INSTALL_PREFIX}/python/lsst/sphgeom)
