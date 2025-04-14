## Steps to set up and build the project using Bazel.
Preparation of the environment

```
apt install bazel python3 python3-pip
pip3 install conan
```

Building a project
```
$ git clone git@github.com:Szymonjac/bazel.git
$ conan install .
$ bazel --bazelrc=./conan/conan_bzl.rc build --config=conan-config //test:Mox_test
$ bazel --bazelrc=./conan/conan_bzl.rc build --config=conan-config //src:MoxPP
```

## Instructions on how Conan is integrated within the Bazel build system.
- In the WORKSPACR file put the variables
    ```
    load("@//conan:dependencies.bzl", "load_conan_dependencies")
    load_conan_dependencies()
    ```
- In the root directory put the conanfile.txt file with dependent libraries for example
    ```
    [requires]
    fmt/10.1.1

    [generators]
    BazelDeps
    BazelToolchain

    [layout]
    bazel_layout
    ```
- Calling the library in the BUILD file (example):
    ```
    cc_binary(
        name = "MoxPP",
        local_defines = ['MOXPP_VERSION=0.1'],
        srcs = ["main.cpp"],
        deps = [
            "@spdlog//:spdlog",
        ],
    )
    ```
## Any considerations or potential issues encountered during the migration.

- Environment customization (Debian-derived systems have older application packages)

- Lack of libraries in conan repository (May occur when migrating from another system)
