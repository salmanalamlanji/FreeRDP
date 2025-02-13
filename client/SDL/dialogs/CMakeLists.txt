set(SRCS
    sdl_button.hpp
    sdl_button.cpp
    sdl_buttons.hpp
    sdl_buttons.cpp
    sdl_dialogs.cpp
    sdl_dialogs.hpp
    sdl_widget.hpp
    sdl_widget.cpp
    sdl_input.hpp
    sdl_input.cpp
    sdl_input_widgets.cpp
    sdl_input_widgets.hpp
    sdl_select.hpp
    sdl_select.cpp
    sdl_selectlist.hpp
    sdl_selectlist.cpp
    sdl_connection_dialog.cpp
    sdl_connection_dialog.hpp
)

list(APPEND LIBS
    ${SDL2_LIBRARIES}
    sdl_client_res
    winpr
)

macro(find_sdl_component name)
	find_package(${name})
	if (NOT ${name}_FOUND)
		find_package(PkgConfig REQUIRED)
		pkg_check_modules(${name} REQUIRED ${name})

		if (BUILD_SHARED_LIBS)
			list(APPEND LIBS ${${name}_LIBRARIES})
			link_directories(${${name}_LIBRARY_DIRS})
			include_directories(${${name}_INCLUDE_DIRS})
		else()
			list(APPEND LIBS ${${name}_STATIC_LIBRARIES})
			link_directories(${${name}_STATIC_LIBRARY_DIRS})
			include_directories(${${name}_STATIC_INCLUDE_DIRS})
		endif()
	else()
		if (BUILD_SHARED_LIBS)
			list(APPEND LIBS ${name}::${name})
		else()
			list(APPEND LIBS ${name}::${name}-static)
		endif()
	endif()
endmacro()

find_sdl_component(SDL2_ttf)
find_sdl_component(SDL2_image)

add_subdirectory(res)

add_library(dialogs STATIC
    ${SRCS}
)

target_link_libraries(dialogs PRIVATE ${LIBS})

if(BUILD_TESTING)
#	add_subdirectory(test)
endif()
