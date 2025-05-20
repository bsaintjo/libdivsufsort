# A list of available rules and their signatures can be found here: <https://buck2.build/docs/prelude/globals/>

genrule(
    name = "libdivsufsort/cmake",
    srcs = [
        "examples", "include", "lib",
        "CMakeLists.txt", "VERSION.cmake",
        "CMakeModules", "pkgconfig",
        "LICENSE", "README.md",
    ],
    cmd =
"""
OUT=$PWD/$OUT
cp -rL $SRCS $TMP

cd $TMP
mkdir build
cd build

cmake -DCMAKE_INSTALL_PREFIX=$OUT \
    -DCMAKE_POLICY_VERSION_MINIMUM=3.5 \
    -DBUILD_SHARED_LIBS=OFF \
    -DCMAKE_BUILD_EXAMPLES=OFF \
    -DCMAKE_BUILD_TYPE=Release \
    ..
make && make install
cp -L include/lfs.h $OUT/include/lfs.h
cp -L include/config.h $OUT/include/config.h
""",
    outs = {
        "divsufsort.h": ["include/divsufsort.h"],
        "lfs.h": ["include/lfs.h"],
        "config.h": ["include/config.h"],
        "lib": ["lib/libdivsufsort.a"],
    }
)

export_file(
    name = "libdivsufsort/header/divsufsort.h",
    src = ":libdivsufsort/cmake[divsufsort.h]",
    out = "divsufsort.h",
)

export_file(
    name = "libdivsufsort/header/lfs.h",
    src = ":libdivsufsort/cmake[lfs.h]",
    out = "lfs.h",
)

export_file(
    name = "libdivsufsort/header/config.h",
    src = ":libdivsufsort/cmake[config.h]",
    out = "config.h",
)

cxx_library(
    name = "libdivsufsort",
    exported_headers = [
        ":libdivsufsort/header/divsufsort.h",
        ":libdivsufsort/header/lfs.h",
        ":libdivsufsort/header/config.h",
    ],
    public_include_directories = [
        ":libdivsufsort/header/divsufsort.h",
        ":libdivsufsort/header/lfs.h",
        ":libdivsufsort/header/config.h",
    ],
    exported_linker_flags = ["$(location :libdivsufsort/cmake[lib])"],
)

cxx_binary(
    name = "example",
    # srcs = ["example.cpp"],
    srcs = ["examples/suftest.c"],
    deps = [":libdivsufsort"],
)
