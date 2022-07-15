# Drumstick RT (realtime MIDI In/Out)
# Copyright (C) 2009-2022 Pedro Lopez-Cabanillas <plcl@users.sourceforge.net>
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 3 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program. If not, see <http://www.gnu.org/licenses/>.

cmake_minimum_required(VERSION 3.14)
project( drumstick-rt-outoftree
    VERSION 2.7
    LANGUAGES CXX
)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include(GNUInstallDirs)

option(STATIC_DRUMSTICK "Build static plugins instead of shared ones" OFF)

find_package(QT NAMES Qt5 Qt6 REQUIRED)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Core Widgets)
find_package(Drumstick 2.7 REQUIRED COMPONENTS RT)

set(DRUMSTICK_PLUGINS_DIR "drumstick2")
message(STATUS "DRUMSTICK PLUGINS destination directory: ${CMAKE_INSTALL_FULL_LIBDIR}/${DRUMSTICK_PLUGINS_DIR}")
get_target_property(DRUMSTICK_INTERFACES Drumstick::RT INTERFACE_INCLUDE_DIRECTORIES)
message(STATUS "DRUMSTICK INCLUDE INTERFACES: ${DRUMSTICK_INTERFACES}")

set(DUMMY_IN_SOURCES
    dummyinput.cpp
    dummyinput.h
)

set(DUMMY_OUT_SOURCES 
    dummyoutput.cpp
    dummyoutput.h
)

if(STATIC_DRUMSTICK)
    add_library(drumstick-rt-dummy-in STATIC ${DUMMY_IN_SOURCES})
    target_compile_definitions(drumstick-rt-dummy-in PRIVATE QT_STATICPLUGIN VERSION=${PROJECT_VERSION})
    
    add_library(drumstick-rt-dummy-out STATIC ${DUMMY_OUT_SOURCES})
    target_compile_definitions(drumstick-rt-dummy-out PRIVATE QT_STATICPLUGIN VERSION=${PROJECT_VERSION})
else()
    add_library(drumstick-rt-dummy-in MODULE ${DUMMY_IN_SOURCES})
    target_compile_definitions(drumstick-rt-dummy-in PRIVATE QT_PLUGIN VERSION=${PROJECT_VERSION})
    
    add_library(drumstick-rt-dummy-out MODULE ${DUMMY_OUT_SOURCES})
    target_compile_definitions(drumstick-rt-dummy-out PRIVATE QT_PLUGIN VERSION=${PROJECT_VERSION})
endif()

target_link_libraries(drumstick-rt-dummy-in PRIVATE
    Qt${QT_VERSION_MAJOR}::Core
    Qt${QT_VERSION_MAJOR}::Widgets
    Drumstick::RT
)

target_link_libraries(drumstick-rt-dummy-out PRIVATE
    Qt${QT_VERSION_MAJOR}::Core
    Qt${QT_VERSION_MAJOR}::Widgets
    Drumstick::RT
)

install(TARGETS drumstick-rt-dummy-in drumstick-rt-dummy-out
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}/${DRUMSTICK_PLUGINS_DIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}/${DRUMSTICK_PLUGINS_DIR}
)
