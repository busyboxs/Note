# CMake FindBoost

## find_package

```cmake
find_package(Boost
  [version] [EXACT]      # Minimum or EXACT version e.g. 1.67.0
  [REQUIRED]             # Fail with error if Boost is not found
  [COMPONENTS <libs>...] # Boost libraries by their canonical name
                         # e.g. "date_time" for "libboost_date_time"
  [OPTIONAL_COMPONENTS <libs>...]
                         # Optional Boost libraries by their canonical name)
  )                      # e.g. "date_time" for "libboost_date_time"
```

## Result Variables

|Variable|Description|
|:-------|:----------|
|`Boost_FOUND`|True if headers and requested libraries were found.|
|`Boost_INCLUDE_DIRS` |Boost include directories.|
|`Boost_LIBRARY_DIRS` |Link directories for Boost libraries. |
|`Boost_LIBRARIES` |Boost component libraries to be linked. |
|`Boost_<COMPONENT>_FOUND` |True if component `<COMPONENT>` was found (`<COMPONENT>` name is upper-case). |
|`Boost_<COMPONENT>_LIBRARY` |Libraries to link for component `<COMPONENT>` (may include `target_link_libraries()` debug/optimized keywords). |
|`Boost_VERSION_MACRO` |BOOST_VERSION value from boost/version.hpp. |
|`Boost_VERSION_STRING` |Boost version number in X.Y.Z format. |
|`Boost_VERSION` |Boost version number in X.Y.Z format (same as `Boost_VERSION_STRING`). |
|`Boost_LIB_VERSION` |Version string appended to library filenames. |
|`Boost_VERSION_MAJOR`, `Boost_MAJOR_VERSION` |Boost major version number (X in X.Y.Z). |
|`Boost_VERSION_MINOR`, `Boost_MINOR_VERSION` |Boost minor version number (Y in X.Y.Z). |
|`Boost_VERSION_PATCH`, `Boost_SUBMINOR_VERSION` |Boost subminor version number (Z in X.Y.Z). |
|`Boost_VERSION_COUNT` |Amount of version components (3). |
|`Boost_LIB_DIAGNOSTIC_DEFINITIONS` (Windows-specific) |Pass to add_definitions() to have diagnostic information about Boost's automatic linking displayed during compilation |

## Cache variables

|Variable|Description|
|:-------|:----------|
|`Boost_INCLUDE_DIR` |Directory containing Boost headers. |
|`Boost_LIBRARY_DIR_RELEASE` |Directory containing release Boost libraries. |
|`Boost_LIBRARY_DIR_DEBUG` |Directory containing debug Boost libraries. |
|`Boost_<COMPONENT>_LIBRARY_DEBUG` |Component `<COMPONENT>` library debug variant. |
|`Boost_<COMPONENT>_LIBRARY_RELEASE` |Component `<COMPONENT>` library release variant. |

## Hints

|Variable|Description|
|:-------|:----------|
|`BOOST_ROOT`, `BOOSTROOT` |Preferred installation prefix. |
|`BOOST_INCLUDEDIR` |Preferred include directory e.g. `<prefix>/include`. |
|`BOOST_LIBRARYDIR` |Preferred library directory e.g. `<prefix>/lib`. |
|`Boost_NO_SYSTEM_PATHS` |Set to `ON` to disable searching in locations not specified by these hint variables. Default is `OFF`. |
|`Boost_ADDITIONAL_VERSIONS` |List of Boost versions not known to this module. (Boost install locations may contain the version). |

## Imported Targets

|Variable|Description|
|:-------|:----------|
|`Boost::boost` |Target for header-only dependencies. (Boost include directory). |
|`Boost::headers` |New in version 3.15: Alias for Boost::boost. |
|`Boost::<component>` |Target for specific component dependency (shared or static library); `<component>` name is lower-case. |
|`Boost::diagnostic_definitions` |Interface target to enable diagnostic information about Boost's automatic linking during compilation (adds `-DBOOST_LIB_DIAGNOSTIC`). |
|`Boost::disable_autolinking` |Interface target to disable automatic linking with MSVC (adds `-DBOOST_ALL_NO_LIB`). |
|`Boost::dynamic_linking` |Interface target to enable dynamic linking linking with MSVC (adds `-DBOOST_ALL_DYN_LINK`). |

## Other Variables

|Variable|Description|
|:-------|:----------|
|`Boost_USE_DEBUG_LIBS` |Set to `ON` or `OFF` to specify whether to search and use the debug libraries. Default is `ON`. |
|`Boost_USE_RELEASE_LIBS` |Set to `ON` or `OFF` to specify whether to search and use the release libraries. Default is `ON`. |
|`Boost_USE_MULTITHREADED` |Set to `OFF` to use the non-multithreaded libraries (`"mt"` tag). Default is `ON`. |
|`Boost_USE_STATIC_LIBS` |Set to `ON` to force the use of the static libraries. Default is `OFF`. |
|`Boost_USE_STATIC_RUNTIME` |Set to `ON` or `OFF` to specify whether to use libraries linked statically to the C++ runtime (`"s"` tag). Default is platform dependent. |
|`Boost_USE_DEBUG_RUNTIME` |Set to `ON` or `OFF` to specify whether to use libraries linked to the MS debug C++ runtime (`"g"` tag). Default is ON. |
|`Boost_USE_DEBUG_PYTHON` |Set to `ON` to use libraries compiled with a debug Python build (`"y"` tag). Default is `OFF`. |
|`Boost_USE_STLPORT` |Set to `ON` to use libraries compiled with STLPort (`"p"` tag). Default is `OFF`. |
|`Boost_USE_STLPORT_DEPRECATED_NATIVE_IOSTREAMS` |Set to `ON` to use libraries compiled with STLPort deprecated "native iostreams" (`"n"` tag). Default is `OFF`. |
|`Boost_COMPILER` |Set to the compiler-specific library suffix (e.g. `-gcc43`). Default is auto-computed for the C++ compiler in use. |
|`Boost_LIB_PREFIX` |Set to the platform-specific library name prefix (e.g. `lib`) used by Boost static libs. This is needed only on platforms where CMake does not know the prefix by default. |
|`Boost_ARCHITECTURE` |Set to the architecture-specific library suffix (e.g. `-x64`). Default is auto-computed for the C++ compiler in use. |
|`Boost_THREADAPI` |Suffix for thread component library name, such as `pthread` or `win32`. Names with and without this suffix will both be tried. |
|`Boost_NAMESPACE` |Alternate namespace used to build boost with e.g. if set to `myboost`, will search for `myboost_thread` instead of `boost_thread`. |


## Example

**1. find boost header only**

```cmake
find_package(Boost 1.36.0)
if(Boost_FOUND)
  include_directories(${Boost_INCLUDE_DIRS})
  add_executable(foo foo.cc)
endif()
```

**2. find Boost libraries and use imported targets**

```cmake
find_package(Boost 1.56 REQUIRED COMPONENTS
             date_time filesystem iostreams)
add_executable(foo foo.cc)
target_link_libraries(foo Boost::date_time Boost::filesystem
                          Boost::iostreams)
```

**3. Find Boost Python 3.6 libraries and use imported targets**

```cmake
find_package(Boost 1.67 REQUIRED COMPONENTS
             python36 numpy36)
add_executable(foo foo.cc)
target_link_libraries(foo Boost::python36 Boost::numpy36)
```

**4. Find Boost headers and some static (release only) libraries**

```cmake
set(Boost_USE_STATIC_LIBS        ON)  # only find static libs
set(Boost_USE_DEBUG_LIBS        OFF)  # ignore debug libs and
set(Boost_USE_RELEASE_LIBS       ON)  # only find release libs
set(Boost_USE_MULTITHREADED      ON)
set(Boost_USE_STATIC_RUNTIME    OFF)
find_package(Boost 1.66.0 COMPONENTS date_time filesystem system ...)
if(Boost_FOUND)
  include_directories(${Boost_INCLUDE_DIRS})
  add_executable(foo foo.cc)
  target_link_libraries(foo ${Boost_LIBRARIES})
endif()
```
