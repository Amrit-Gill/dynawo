# Copyright (c) 2015-2019, RTE (http://www.rte-france.com)
# See AUTHORS.txt
# All rights reserved.
# This Source Code Form is subject to the terms of the Mozilla Public
# License, v. 2.0. If a copy of the MPL was not distributed with this
# file, you can obtain one at http://mozilla.org/MPL/2.0/.
# SPDX-License-Identifier: MPL-2.0
#
# This file is part of Dynawo, an hybrid C++/Modelica open source time domain simulation tool for power systems.

# API version
set(API_PAR_VERSION_STRING ${DYNAWO_VERSION_STRING})
set(API_PAR_VERSION_MAJOR ${DYNAWO_VERSION_MAJOR})

set(API_PAR_SHARED
    xsd/parameters.xsd
    )

install(FILES ${API_PAR_SHARED} DESTINATION ${SHAREDIR_NAME}/xsd/)

set(API_PAR_SOURCES
    PARParameterImpl.cpp
    PARParameterFactory.cpp
    PARReferenceImpl.cpp
    PARReferenceFactory.cpp
    PARParametersSet.cpp
    PARParametersSetImpl.cpp
    PARParametersSetFactory.cpp
    PARParametersSetCollection.cpp
    PARParametersSetCollectionImpl.cpp
    PARParametersSetCollectionFactory.cpp
    PARXmlExporter.cpp
    PARXmlHandler.cpp
    PARXmlImporter.cpp
    )

set(API_PAR_INCLUDE_HEADERS
    PARParameter.h
    PARParameterFactory.h
    PARReference.h
    PARReferenceFactory.h
    PARParametersSet.h
    PARParametersSetFactory.h
    PARParametersSetCollection.h
    PARParametersSetCollectionFactory.h
    PARXmlExporter.h
    PARXmlImporter.h
    PARExporter.h
    PARImporter.h
    )

add_library(dynawo_API_PAR ${LIBRARY_TYPE} ${API_PAR_SOURCES})

target_include_directories(dynawo_API_PAR
    INTERFACE
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
        $<INSTALL_INTERFACE:${INCLUDEDIR_NAME}>
    )

target_link_libraries(dynawo_API_PAR
    XMLSAXFormatter${LibXML_LINK_SUFFIX}
    XMLSAXParser${LibXML_LINK_SUFFIX}
    dynawo_Common
    )

set_target_properties(dynawo_API_PAR PROPERTIES VERSION ${API_PAR_VERSION_STRING}
                                              SOVERSION ${API_PAR_VERSION_MAJOR})

install(TARGETS dynawo_API_PAR EXPORT dynawo-targets DESTINATION ${LIBDIR_NAME})
install(FILES ${API_PAR_INCLUDE_HEADERS} DESTINATION ${INCLUDEDIR_NAME})
