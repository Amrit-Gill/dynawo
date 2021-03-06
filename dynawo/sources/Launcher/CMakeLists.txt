# Copyright (c) 2015-2019, RTE (http://www.rte-france.com)
# See AUTHORS.txt
# All rights reserved.
# This Source Code Form is subject to the terms of the Mozilla Public
# License, v. 2.0. If a copy of the MPL was not distributed with this
# file, you can obtain one at http://mozilla.org/MPL/2.0/.
# SPDX-License-Identifier: MPL-2.0
#
# This file is part of Dynawo, an hybrid C++/Modelica open source time domain simulation tool for power systems.

# Dynawo
set(DYNAWO_SOURCES
    main.cpp
    )

install(FILES ${DYNAWO_HEADERS} DESTINATION ${INCLUDEDIR_NAME})

add_executable(dynawo ${DYNAWO_SOURCES})

target_link_libraries(dynawo
    dynawo_Simulation
    )

if(APPLE)
  # Additional step to make shared library work on macOS; see https://gist.github.com/robertmaynard/5750737
  set_target_properties(dynawo PROPERTIES INSTALL_RPATH "@loader_path/../lib")
endif()

set_target_properties(dynawo PROPERTIES VERSION ${DYNAWO_VERSION_STRING})

install(TARGETS dynawo DESTINATION ${BINDIR_NAME})

if(NOT MSVC)
  # Launcher
  configure_file(launcher.in launcher @ONLY)

  install(PROGRAMS ${CMAKE_CURRENT_BINARY_DIR}/launcher DESTINATION ${BINDIR_NAME})

  # execution script for distribution
  set(EXEC_DYNAWO
      execDynawo.sh
      )

  install(PROGRAMS ${EXEC_DYNAWO} DESTINATION ${BINDIR_NAME})
else()
  # Launcher
  configure_file(dynawo.cmd.in dynawo.cmd @ONLY)
  install(PROGRAMS ${CMAKE_CURRENT_BINARY_DIR}/dynawo.cmd DESTINATION /)
endif()
