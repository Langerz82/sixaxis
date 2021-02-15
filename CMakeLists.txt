cmake_minimum_required(VERSION 3.6)
project(sixaxis LANGUAGES C)

add_executable(sixaxis-timeout sixaxis-timeout.c)

# Point the service helper to the install location set by CMAKE_INSTALL_PREFIX
configure_file(${CMAKE_SOURCE_DIR}/sixaxis@.service.in ${CMAKE_BINARY_DIR}/sixaxis@.service @ONLY)

include(GNUInstallDirs)
install(TARGETS sixaxis-timeout RUNTIME DESTINATION ${CMAKE_INSTALL_PREFIX}/sbin)
install(FILES sixaxis-helper.sh DESTINATION ${CMAKE_INSTALL_PREFIX}/bin)
install(FILES 99-sixaxis.rules DESTINATION /etc/udev/rules.d)
install(FILES ${CMAKE_BINARY_DIR}/sixaxis@.service DESTINATION /lib/systemd/system)
install(FILES README.md COPYING DESTINATION ${CMAKE_INSTALL_FULL_DOCDIR})

# Debian packaging
#  - try to maintain the same version number as the one produced by 'checkinstall'
execute_process(COMMAND date +%Y%m%d-1 OUTPUT_VARIABLE PACKAGE_VER OUTPUT_STRIP_TRAILING_WHITESPACE)

# - the rest of the DEB generator options
SET(CPACK_PACKAGING_INSTALL_PREFIX ${CMAKE_INSTALL_PREFIX})
SET(CPACK_GENERATOR "DEB")
SET(CPACK_DEBIAN_PACKAGE_VERSION ${PACKAGE_VER})
SET(CPACK_DEBIAN_FILE_NAME DEB-DEFAULT)
SET(CPACK_DEBIAN_PACKAGE_NAME "sixaxis")
SET(CPACK_DEBIAN_PACKAGE_SECTION "utils")
SET(CPACK_DEBIAN_PACKAGE_DESCRIPTION "Helper service for official and third-party Sony DualShock controllers")
SET(CPACK_DEBIAN_PACKAGE_HOMEPAGE "https://github.com/RetroPie/sixaxis.git")
SET(CPACK_DEBIAN_PACKAGE_DEPENDS "systemd, libevdev-tools, bluez")
SET(CPACK_DEBIAN_PACKAGE_MAINTAINER "RetroPie Project <retropieproject@gmail.com>")
SET(CPACK_DEBIAN_PACKAGE_CONTROL_EXTRA "${CMAKE_SOURCE_DIR}/debian/postinst;${CMAKE_SOURCE_DIR}/debian/postrm")
INCLUDE(CPack)
