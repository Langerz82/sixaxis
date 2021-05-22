## sixaxis

The sixaxis helper service is designed as a simple replacement for the sixad wrapper. It will
* install udev rules to suppress the motion sensors node (as it causes issues with player slot assignment in emulators)
* configure a sensible fuzz for analog axes (so that the analog sticks will not produce events when in a resting state)
* implement a simple 10 minute idle timeout mechanism (as the bluez plugin does not support the standard IdleTimeout timer).

The service is recommend for use with kernel 4.15 and Bluez 5.48 to ensure full compatibility with third-party controllers.

## installation

`cmake` / `make` is required to build the helper. `systemd`, `udev` and `libevdev-tools`(Debian/Ubuntu) or `libevdev-utils`(Fedora/Centos/SuSE) are required for running the helper.

### build/install with `make`

To build and install, run from the source folder:

```
make install
```

## build/install with `cmake`

To build and install, run from the source folder
```
mkdir build && cd build
cmake -DCMAKE_INSTALL_PREFIX:PATH=/usr ..
[sudo] make [install]
# reload systemctl unit files
[sudo] systemctl daemon-reload
```

Replace `CMAKE_INSTALL_PREFIX` with a different value of you wish to install to another prefix (i.e. `/opt`)

## packaging with `cmake`

To create a Debian `.deb` package, run from the source folder
```
mkdir build && cd build
cmake -DCMAKE_INSTALL_PREFIX:PATH=/usr ..
make package
# or
cpack -GDEB .
```

This will create a `sixaxis` `.deb` package in the current build folder. Installing the package will add the necessary `udev` rules and reload `systemctl` automatically.
