# Description: Build rules for building androidx.test from source

load("@build_bazel_rules_android//android:rules.bzl", "android_library")
load("//build_extensions:api_checks.bzl", "api_checks")
load("//build_extensions:dackka_test.bzl", "dackka_test")
load("//build_extensions/maven:axt_android_aar.bzl", "axt_android_aar")
load("//build_extensions/maven:maven_artifact.bzl", "maven_artifact")

# all users should reference the equivalent targets in //runner/android_junit_runner
package(
    default_applicable_licenses = ["//:license"],
    default_testonly = 1,
    default_visibility = [
        ":allowlist",
    ],
)

package_group(
    name = "allowlist",
    packages = [
        "//...",
    ],
)

licenses(["notice"])

android_library(
    name = "runner",
    srcs = glob(
        ["**/*.java"],
    ),
    custom_package = "androidx.test.runner",
    idl_srcs = glob(["**/*.aidl"]),
    manifest = "AndroidManifest.xml",
    proguard_specs = [
        "proguard_library.cfg",
    ],
    exports = [
        "//runner/monitor",
    ],
    deps = [
        "//opensource/androidx:annotation",
        "//runner/monitor",
        "//services/events/java/androidx/test/services/events",
        "//services/storage",
        "@androidsdk//:legacy_test-35",
        "@maven//:androidx_tracing_tracing",
        "@maven//:junit_junit",
    ],
)

# Generate rules for the release artifacts.
axt_android_aar(
    name = "runner_aar",
    expected_class_prefixes = [
        "androidx.test.services.events",
        "androidx.test.runner",
        "androidx.test.orchestrator",
        "androidx.test.internal",
        "androidx.test.filters",
    ],
    included_dep = ":runner",
)

maven_artifact(
    name = "runner_maven_artifact",
    last_updated = "20170622000000",
    target = ":runner_aar",
)

dackka_test(
    name = "runner_doc",
    runtime_dep = ":runner",
    src_jar = ":runner_aar-src.jar",
)

api_checks(
    name = "runner_api",
    runtime_dep = ":runner",
    src_jar = ":runner_aar-src.jar",
)
