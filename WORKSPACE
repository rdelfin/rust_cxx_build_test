WORKSPACE_NAME = "rust_cxx_build_test"

workspace(name = WORKSPACE_NAME)

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

http_archive(
    name = "rules_rust",
    sha256 = "dc8d79fe9a5beb79d93e482eb807266a0e066e97a7b8c48d43ecf91f32a3a8f3",
    urls = ["https://github.com/bazelbuild/rules_rust/releases/download/0.19.0/rules_rust-v0.19.0.tar.gz"],
)

# Rust

load("@rules_rust//rust:repositories.bzl", "rust_repositories")

rust_repositories(
    edition = "2021",
    versions = ["1.68.0"],
)

load("@rules_rust//tools/rust_analyzer:deps.bzl", "rust_analyzer_deps")

rust_analyzer_deps()

# Loads all 3rdparty crates
load("@rules_rust//crate_universe:repositories.bzl", "crate_universe_dependencies")
crate_universe_dependencies()

load("@rules_rust//crate_universe:defs.bzl", "crate", "crates_repository")
crates_repository(
    name = "crate_index",
    lockfile = "//:Cargo.Bazel.lock",
    cargo_lockfile = "//:Cargo.lock",
    packages = {
        "cxxbridge-cmd": crate.spec(version = "1.0.92"),
    },
)

load("@crate_index//:defs.bzl", "crate_repositories")
crate_repositories()
