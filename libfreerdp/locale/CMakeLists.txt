# FreeRDP: A Remote Desktop Protocol Implementation
# libfreerdp-locale cmake build script
#
# Copyright 2012 Marc-Andre Moreau <marcandre.moreau@gmail.com>
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

set(MODULE_NAME "freerdp-locale")
set(MODULE_PREFIX "FREERDP_LOCALE")

set(${MODULE_PREFIX}_SRCS
	virtual_key_codes.c
	keyboard_layout.c
	keyboard.c
	locale.c
	timezone.c
	liblocale.h)

set(${MODULE_PREFIX}_X11_SRCS
	xkb_layout_ids.c
	xkb_layout_ids.h)

set(${MODULE_PREFIX}_X11_KEYMAP_SRCS
	keyboard_keymap.c
	keyboard_keymap.h
	keyboard_x11.c
	keyboard_x11.h)

set(${MODULE_PREFIX}_XKBFILE_SRCS
	keyboard_xkbfile.c
	keyboard_xkbfile.h)

set(${MODULE_PREFIX}_SUN_SRCS
	keyboard_sun.c
	keyboard_sun.h)

if(CMAKE_SYSTEM_NAME MATCHES Solaris)
	set(WITH_SUN true)
endif()

if(NOT WIN32)
	find_suggested_package(X11)
	if(WITH_X11)
		add_definitions(-DWITH_X11)
		include_directories(${X11_INCLUDE_DIRS})
		set(${MODULE_PREFIX}_SRCS ${${MODULE_PREFIX}_SRCS} ${${MODULE_PREFIX}_X11_SRCS})
		set(${MODULE_PREFIX}_LIBS ${${MODULE_PREFIX}_LIBS} ${X11_LIBRARIES})
		if(WITH_SUN)
			add_definitions(-DWITH_SUN)
			set(${MODULE_PREFIX}_SRCS ${${MODULE_PREFIX}_SRCS} ${${MODULE_PREFIX}_SUN_SRCS})
		elseif(NOT APPLE)
			find_suggested_package(XKBFile)
		endif()
		if(WITH_XKBFILE)
			add_definitions(-DWITH_XKBFILE)
			include_directories(${XKBFILE_INCLUDE_DIRS})
			set(${MODULE_PREFIX}_SRCS ${${MODULE_PREFIX}_SRCS} ${${MODULE_PREFIX}_XKBFILE_SRCS})
			set(${MODULE_PREFIX}_LIBS ${${MODULE_PREFIX}_LIBS} ${XKBFILE_LIBRARIES})
		else()
			set(${MODULE_PREFIX}_SRCS ${${MODULE_PREFIX}_SRCS} ${${MODULE_PREFIX}_X11_KEYMAP_SRCS})
		endif()
	endif()
endif()

if(MONOLITHIC_BUILD)
	add_library(${MODULE_NAME} OBJECT ${${MODULE_PREFIX}_SRCS})
else()
	add_library(${MODULE_NAME} ${${MODULE_PREFIX}_SRCS})
endif()

set_target_properties(${MODULE_NAME} PROPERTIES VERSION ${FREERDP_VERSION_FULL} SOVERSION ${FREERDP_VERSION} PREFIX "lib")

if(MONOLITHIC_BUILD)
	set(${MODULE_PREFIX}_LIBS ${${MODULE_PREFIX}_LIBS} winpr)
	set(FREERDP_LIBS ${FREERDP_LIBS} ${${MODULE_PREFIX}_LIBS} PARENT_SCOPE)
else()
	set(${MODULE_PREFIX}_LIBS ${${MODULE_PREFIX}_LIBS} winpr-crt)
	set(${MODULE_PREFIX}_LIBS ${${MODULE_PREFIX}_LIBS} freerdp-utils)

	target_link_libraries(${MODULE_NAME} ${${MODULE_PREFIX}_LIBS})
	install(TARGETS ${MODULE_NAME} DESTINATION ${CMAKE_INSTALL_LIBDIR})
endif()

set_property(TARGET ${MODULE_NAME} PROPERTY FOLDER "FreeRDP/libfreerdp")
