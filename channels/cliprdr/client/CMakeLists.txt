# FreeRDP: A Remote Desktop Protocol Client
# FreeRDP cmake build script
#
# Copyright 2011 O.S. Systems Software Ltda.
# Copyright 2011 Otavio Salvador <otavio@ossystems.com.br>
# Copyright 2011 Marc-Andre Moreau <marcandre.moreau@gmail.com>
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

set(CLIPRDR_SRCS
	cliprdr_constants.h
	cliprdr_format.c
	cliprdr_format.h
	cliprdr_main.c
	cliprdr_main.h)

add_library(cliprdr ${CLIPRDR_SRCS})
set_target_properties(cliprdr PROPERTIES PREFIX "")

if(WITH_MONOLITHIC_BUILD)
	target_link_libraries(cliprdr freerdp)
else()
	target_link_libraries(cliprdr freerdp-utils)
endif()

install(TARGETS cliprdr DESTINATION ${FREERDP_PLUGIN_PATH})