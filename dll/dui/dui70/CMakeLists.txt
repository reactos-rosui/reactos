add_definitions(
    -DDUI70
    -D__ROS_LONG64__
    -D_DUI70_)

include_directories("include/")

remove_definitions(-D_WIN32_WINNT=0x502 -DWINVER=0x502)
add_definitions(-D_WIN32_WINNT=0x600 -DWINVER=0x600)

spec2def(dui70.dll dui70.spec ADD_IMPORTLIB)


list(APPEND PCH_SKIP_SOURCE
    DirectUI.cpp)

add_library(dui70 MODULE
   # ${SOURCE}
    ${PCH_SKIP_SOURCE}
    dui70.rc
    ${CMAKE_CURRENT_BINARY_DIR}/dui70.def)

set_module_type(dui70 win32dll UNICODE)
target_link_libraries(dui70 uuid wine ${PSEH_LIB} oldnames)
add_importlibs(dui70 WindowsCodecs oleacc uxtheme gdiplus oleaut32 msvcrt user32 gdi32 advapi32 kernel32 ntdll)
add_pch(dui70 DirectUI.h "${PCH_SKIP_SOURCE}")
add_cd_file(TARGET dui70 DESTINATION reactos/system32 FOR all)
