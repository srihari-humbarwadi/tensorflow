# Experimental gcs filesystem plugin.
load("//tensorflow:tensorflow.bzl", "get_win_copts", "tf_cc_shared_object", "tf_cc_test")

package(
    licenses = ["notice"],  # Apache 2.0
)

# Filesystem implementation for GCS environments
tf_cc_shared_object(
    name = "gcs_filesystem",
    framework_so = [],
    linkstatic = False,
    per_os_targets = 1,
    visibility = ["//visibility:public"],
    deps = [":gcs_filesystem_impl"],
)

# The real implementation of the filesystem.
cc_library(
    name = "gcs_filesystem_impl",
    srcs = ["gcs_filesystem.cc"],
    hdrs = ["gcs_filesystem.h"],
    copts = select({
        "//conditions:default": [],
        "//tensorflow:windows": get_win_copts(),
    }),
    deps = [
        ":gcs_helper",
        "//tensorflow/c:env",
        "//tensorflow/c:tf_status",
        "//tensorflow/c/experimental/filesystem:filesystem_interface",
        "@com_github_googlecloudplatform_google_cloud_cpp//:storage_client",
        "@com_google_absl//absl/strings",
    ],
)

cc_library(
    name = "gcs_helper",
    srcs = ["gcs_helper.cc"],
    hdrs = ["gcs_helper.h"],
    linkstatic = 1,
    deps = [
        "//tensorflow/c:env",
    ],
)

tf_cc_test(
    name = "gcs_filesystem_test",
    srcs = [
        "gcs_filesystem.cc",
        "gcs_filesystem_test.cc",
    ],
    local_defines = ["TF_GCS_FILESYSTEM_TEST"],
    tags = [
        "manual",
        "notap",
    ],
    deps = [
        ":gcs_filesystem_impl",
        "//tensorflow/c:tf_status_helper",
        "//tensorflow/core/platform:stacktrace_handler",
        "//tensorflow/core/platform:test",
    ],
)
