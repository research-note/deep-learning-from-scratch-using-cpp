# BUILD

In this stage we step it up and showcase how to integrate multiple ```cc_library``` targets from different packages.

BUILD file is in a subdirectory called lib. In Bazel, subdirectories containing BUILD files are known as packages. The new property ```visibility``` will tell Bazel which package(s) can reference this target, in this case the ```//main``` package can use ```lib``` library. 

```
cc_library(
    name = "hello-time",
    srcs = ["hello-time.cc"],
    hdrs = ["hello-time.h"],
    visibility = ["//main:__pkg__"],
)
```

To use our ```lib``` libary, an extra dependency is added in the form of //path/to/package:target_name, in this case, it's ```//lib:lib```

```
cc_binary(
    name = "hello-world",
    srcs = ["hello-world.cc"],
    deps = [
        ":hello-greet",
        "//lib:hello-time",
    ],
)
```

To build this example you use (notice that 3 slashes are required in windows)
```
bazel build --config=clang_config //main:main

# In Windows, note the three slashes

bazel build --config=clang_config ///main:main
```
