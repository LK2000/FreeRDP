# FreeRDP: A Remote Desktop Protocol Implementation
# FreeRDP cmake build script
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

set(MODULE_NAME "smartcard")
set(MODULE_PREFIX "CHANNEL_RDPDR_SMARTCARD_CLIENT")

set(${MODULE_PREFIX}_SRCS
	scard_main.c
	scard_operations.c)

include_directories(${PCSC_INCLUDE_DIRS})

add_channel_client_library(${MODULE_PREFIX} ${MODULE_NAME} TRUE "DVCPluginEntry")

set_target_properties(${MODULE_NAME} PROPERTIES PREFIX "")

set_complex_link_libraries(VARIABLE ${MODULE_PREFIX}_LIBS
	MONOLITHIC ${MONOLITHIC_BUILD}
	MODULE freerdp
	MODULES freerdp-utils)
	
set_complex_link_libraries(VARIABLE ${MODULE_PREFIX}_LIBS
	MONOLITHIC ${MONOLITHIC_BUILD}
	MODULE winpr
	MODULES winpr-crt winpr-synch winpr-thread winpr-interlocked)

set(${MODULE_PREFIX}_LIBS ${${MODULE_PREFIX}_LIBS} ${PCSC_LIBRARIES})

target_link_libraries(${MODULE_NAME} ${${MODULE_PREFIX}_LIBS})

if(NOT STATIC_CHANNELS)
	install(TARGETS ${MODULE_NAME} DESTINATION ${FREERDP_PLUGIN_PATH})
endif()

set_property(TARGET ${MODULE_NAME} PROPERTY FOLDER "Channels/${MODULE_NAME}/Client")