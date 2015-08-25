
cmake_minimum_required(VERSION 3.1.0)

project(pyusb NONE)

find_package(MBCMakeTools REQUIRED NO_POLICY_SCOPE)

build_wheel(
    pyusb
    SETUP_PY ${PROJECT_SOURCE_DIR}/setup.py
    SOURCES
        usb/__init__.py
        usb/_debug.py
        usb/_interop.py
        usb/_lookup.py
        usb/_objfinalizer.py
        usb/control.py
        usb/core.py
        usb/legacy.py
        usb/libloader.py
        usb/util.py
        usb/backend/__init__.py
        usb/backend/libusb0.py
        usb/backend/libusb1.py
        usb/backend/openusb.py
    WHEEL wheel
    WHEEL_NAME wheel_name)

# Also create a .pth file so that we can import the egg without
# manually adding it to the path (see the docs for site.py).
# TODO(chris):  Resolve python3 issues and use mustache here
set(pyusb_pth "${PROJECT_BINARY_DIR}/pyusb.pth")
configure_file("${PROJECT_SOURCE_DIR}/pyusb.pth.in" "${pyusb_pth}")

install(
    FILES ${wheel} "${pyusb_pth}"
    DESTINATION ${PY34_MODULE_DIR})

generate_and_install_config(
    NAME PyUsb
    ADDITIONAL_PATHS
        pyusb ${PY34_MODULE_DIR}/${wheel_name})
