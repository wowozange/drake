set(drakeCollision_SRC_FILES DrakeCollision.cpp Element.cpp Model.cpp)
find_package(Bullet)

if(BULLET_FOUND)
  set(drakeCollision_SRC_FILES ${drakeCollision_SRC_FILES} bullet_model.cc)
  set(bullet "yes" CACHE STRING "yes")
endif()
set(drakeCollision_SRC_FILES ${drakeCollision_SRC_FILES} unusable_model.cc)

add_library_with_exports(LIB_NAME drakeCollision SOURCE_FILES ${drakeCollision_SRC_FILES})

if(BULLET_FOUND)
  target_compile_definitions(drakeCollision PUBLIC BULLET_COLLISION)
  target_include_directories(drakeCollision PUBLIC ${BULLET_INCLUDE_DIRS})
  target_link_libraries(drakeCollision ${BULLET_LIBRARIES})
endif()
target_link_libraries(drakeCollision drakeShapes drakeCommon)

pods_install_libraries(drakeCollision)
drake_install_headers(DrakeCollision.h point_pair.h Element.h Model.h)
pods_install_pkg_config_file(drake-collision
  LIBS -ldrakeCollision
  REQUIRES
  VERSION 0.0.1)

# ==== Below this line is MATLAB-specific code ====

# ==== Below this line is C++ and MATLAB shared code ====

add_subdirectory(test)
