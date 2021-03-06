# Copyright © 2017-2020 Canonical Ltd.
#
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License version 3 as
# published by the Free Software Foundation.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <http://www.gnu.org/licenses/>.

set (CMAKE_AUTOMOC ON)

add_library(qemu_backend STATIC
  dnsmasq_process_spec.cpp
  dnsmasq_server.cpp
  iptables_config.cpp
  qemu_base_process_spec.cpp
  qemu_vm_process_spec.cpp
  qemu_vmstate_process_spec.cpp
  qemu_virtual_machine_factory.cpp
  qemu_virtual_machine.cpp
  ${CMAKE_SOURCE_DIR}/include/multipass/process/basic_process.h
  ${CMAKE_SOURCE_DIR}/include/multipass/process/process.h)

target_link_libraries(qemu_backend
  daemon
  fmt
  logger
  Qt5::Core
  Qt5::Network
  ip_address
  process
  shared_linux
  ssh
  utils
  yaml)

add_custom_target(copy-check-kvm-support ALL DEPENDS
  ${CMAKE_BINARY_DIR}/bin/check_kvm_support)
add_custom_command(OUTPUT ${CMAKE_BINARY_DIR}/bin/check_kvm_support
  COMMAND ${CMAKE_COMMAND} -E copy
      ${CMAKE_CURRENT_SOURCE_DIR}/check_kvm_support
      ${CMAKE_BINARY_DIR}/bin/check_kvm_support
  DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/check_kvm_support)

install(PROGRAMS check_kvm_support
  DESTINATION bin)
