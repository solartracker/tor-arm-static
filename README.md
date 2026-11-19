# tor-arm-static

Statically linked build of **Tor 0.4.9.17** for ARMv7 Linux systems using musl libc.

The primary goal of this project is to provide a self-contained Tor executable for embedded Linux devices, particularly ARM-based routers, where installing and maintaining the usual collection of shared libraries may be impractical.

## Overview

This project cross-compiles Tor and its dependencies using an ARM Linux musl toolchain.

The resulting executables are statically linked, minimizing external runtime library dependencies and making deployment on compatible embedded Linux systems easier.

### Target platform

| Component | Configuration |
|---|---|
| Architecture | ARMv7 |
| ABI | musl |
| Cross-compiler | arm-linux-musleabi |
| Tor version | 0.4.9.17 |
| Linking | Static |

## Dependencies

The following libraries are built as static libraries and linked into Tor:

| Library | Version |
|---|---|
| zlib | 1.3.1 |
| LZ4 | 1.10.0 |
| XZ | 5.8.2 |
| Zstandard | 1.5.7 |
| OpenSSL | 3.6.0 |
| libevent | 2.1.12 |

Source archives are downloaded and verified using SHA-256 checksums before compilation.

The build process also performs final binary stripping and dependency checks.

## Build

The project includes scripts to automate the cross-compilation process and build the required dependencies.

The build process:

1. Downloads the required source archives.
2. Verifies source archives using SHA-256 checksums.
3. Cross-compiles the dependencies for ARMv7 using musl.
4. Builds Tor against the statically compiled libraries.
5. Strips the resulting executables.
6. Checks the resulting binaries for external library dependencies.
7. Stages the resulting executables and supporting data files into the package directory.

## Package contents

The resulting package has the following directory structure:

```text
usr/
├── bin/
│   ├── tor
│   ├── tor-gencert
│   ├── tor-resolve
│   ├── tor-print-ed-signing-cert
│   └── torify
└── share/
    └── tor/
        ├── geoip
        └── geoip6
```

### Executables

| Executable | Description |
|---|---|
| `tor` | Main Tor daemon |
| `tor-gencert` | Generates Tor directory authority certificates |
| `tor-resolve` | Performs hostname resolution through Tor |
| `tor-print-ed-signing-cert` | Displays Tor Ed25519 signing certificates |
| `torify` | Wrapper for running applications through Tor |

The `geoip` and `geoip6` files provide IP address geolocation data used by Tor.

## Installation and configuration

**Installation is currently a manual process.**

An automated installation script has not yet been implemented.

The package contains the executables and GeoIP data files, but does not automatically configure Tor, create system users, establish directory permissions, or configure a service.

### Required files

At a minimum, you will need:

```text
tor
geoip
geoip6
torrc
```

The `torrc` configuration file must be created manually.

You must also create a dedicated `tor` user, or configure Tor to run under another appropriate unprivileged user account.

### Example torrc

```text
GeoIPFile /usr/share/tor/geoip
GeoIPv6File /usr/share/tor/geoip6
```

Adjust the paths to match the actual installation locations on your system.

Additional Tor configuration options will depend on how you intend to use Tor.

Refer to the official Tor documentation for configuration directives:

https://support.torproject.org/

## Important notes

- This project builds Tor for ARMv7 Linux using musl libc.
- The executables are statically linked to minimize runtime library dependencies.
- The build process does not install Tor onto a target device.
- User creation, configuration, permissions, service management, and runtime integration must currently be handled manually.
- The package is intended for compatible ARMv7 Linux systems, not as a universal binary for all ARM architectures.

## Upstream project

Tor Project:

https://www.torproject.org/

Tor source code:

https://gitlab.torproject.org/tpo/core/tor

Tor source releases:

https://dist.torproject.org/
