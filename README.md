# Rust Bazel Universe Test

This repo is a minimal reproducible example demonstrating a crates universe bug where
binaries from a crate are not exposed.

## Reproduce

Start by doing a:

```bash
$ bazel build @crate_index//:cxxbridge-cmd
```

This should build an `.rlib` file that you can link against. Note that the crate built
is not a library but a binary. You can confirm this by looking at the contents of
`bazel-bin/external/crate_index__cxxbridge-cmd-1.0.79`.

However, if you look at the contents of
`bazel-rust-bazel-universe-test/external/crate_index/BUILD.cxxbridge-cmd-1.0.79.bazel`,
you'll notice that there's a `rust_binary` target called `cxxbridge__bin` which seems
to point at the appropriate binary. Howver, the
`bazel-rust-bazel-universe-test/external/crate_index/defs.bzl` file contains this
segment:

```python
_NORMAL_DEPENDENCIES = {
    "": {
        _COMMON_CONDITION: {
            "cxxbridge-cmd": "@crate_index__cxxbridge-cmd-1.0.79//:cxxbridge_cmd",
        },
    },
}

```

Showing that only the library, `:cxxbridge_cmd` is being exposed.
