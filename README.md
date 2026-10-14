libmobile
=========

Library that implements the [Mobile Adapter GB](https://bulbapedia.bulbagarden.net/wiki/Mobile_Game_Boy_Adapter) protocol, in a way that should be easy to integrate into a plethora of different emulators/hardware projects/etc.

This implementation prides itself in being the most complete recreation of the adapter, for which a lot of the research published on [Dan Docs](https://shonumi.github.io/dandocs.html#magb) has been made.


Fork status
-----------

This is the zenaror fork's core library, used by mGBA, libmobile-bgb and PicoAdapterGB with REON. Fork development is on `feature/full_server`; `master` tracks REONTeam/libmobile. The core implements the adapter protocol and delegates platform I/O to frontend callbacks; it is maintained separately from its consumers and the REON server.


Build and validation
--------------------

For a standalone build, use a C11 compiler, CMake 3.25 or newer, and a build tool supported by CMake. Keep the build directory outside this repository:

```sh
cmake -S . -B ../libmobile-build -DCMAKE_C_FLAGS=-Werror
cmake --build ../libmobile-build
ctest --test-dir ../libmobile-build --output-on-failure
```

The CMake test suite is enabled by default for standalone builds and disabled by default when the library is included as a subproject. Tests in `tests/` cover core behavior with fixtures and simulated callbacks; passing them does not establish a natural game flow, production compatibility or physical hardware behavior.

Both static and shared libraries are enabled by default where supported. See [CMakeOptions.txt](CMakeOptions.txt) for weak implementation callbacks, allocation-free use and disabling 32-bit serial mode. [CMakeLists.txt](CMakeLists.txt) also exposes `LIBMOBILE_BUILD_STATIC`, `LIBMOBILE_BUILD_SHARED` and `LIBMOBILE_BUILD_TESTS`. Meson and Autotools definitions remain available in the root; the standalone regression workflow above uses CMake/CTest.


Documentation
-------------

This library is currently documented through its primary header file, [mobile.h](mobile.h). For an example project using it, you can refer to [libmobile-bgb](https://github.com/REONTeam/libmobile-bgb).

The integration contract covers initialization, callback registration, `mobile_start()`, `mobile_loop()`, serial transfers and `mobile_stop()`. Socket and device-auth callbacks must follow the non-blocking contracts documented there. Other headers describe internal implementation and are not frontend APIs.

The existing root layout is retained:

| Responsibility | Location |
| --- | --- |
| Agent instructions and artifact retention | [AGENTS.md](AGENTS.md) |
| Public API and address types | [mobile.h](mobile.h), [mobile_inet.h](mobile_inet.h) |
| Lifecycle, serial protocol and command dispatch | [mobile.c](mobile.c), [serial.c](serial.c), [commands.c](commands.c) |
| Platform callbacks, DNS and relay | [callback.c](callback.c), [dns.c](dns.c), [relay.c](relay.c) |
| Configuration layout and persistence | [config.c](config.c) and its format comments |
| Fork authentication implementation and decisions | [device_auth.h](device_auth.h), [device_auth.c](device_auth.c), [pop3_auth.h](pop3_auth.h), [pop3_auth.c](pop3_auth.c) |
| Regression fixtures and executables | [tests/](tests/), registered by [CMakeLists.txt](CMakeLists.txt) |
| Build definitions and generated configuration templates | [CMakeLists.txt](CMakeLists.txt), [Makefile.am](Makefile.am), [configure.ac](configure.ac), [meson.build](meson.build) and `mobile_config.*.h.in` |

Planning, open questions and handoff are maintained in OMM scope `libmobile`; implementation contracts and format decisions remain in the source files listed above. Consumer updates are coordinated separately: libmobile-bgb and PicoAdapterGB pin a submodule commit, while mGBA vendors a source copy. A core commit must be available on the public mirror before consumers can adopt it.


AI assistance
-------------

Parts of this fork's modifications were developed with assistance from AI tools, including code generation. This statement applies to our modifications, not to the upstream project.


Licensing
---------

As this is a community-ran project that will keep changing, I ask that this library remain user-modifiable and changes to this library are released, such that we can keep improving it. To this end, the code is licensed under the LGPLv3.

If, however, for whatever reason (e.g. platform requirements) you cannot comply with the license, please open an issue on github, or send me an [email](mailto:esteve.varela@gmail.com).

Exemptions:
- Nintendo and its subsidiaries (e.g. NERD) may use and modify this code under the terms of the zero-clause BSD license.
