# Copyright (c) 2018-2019, NVIDIA CORPORATION. All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions
# are met:
#  * Redistributions of source code must retain the above copyright
#    notice, this list of conditions and the following disclaimer.
#  * Redistributions in binary form must reproduce the above copyright
#    notice, this list of conditions and the following disclaimer in the
#    documentation and/or other materials provided with the distribution.
#  * Neither the name of NVIDIA CORPORATION nor the names of its
#    contributors may be used to endorse or promote products derived
#    from this software without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS ``AS IS'' AND ANY
# EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
# IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR
# PURPOSE ARE DISCLAIMED.  IN NO EVENT SHALL THE COPYRIGHT OWNER OR
# CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
# EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
# PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR
# PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY
# OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
# (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
# OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

package(
    default_visibility = ["//visibility:public"],
)

load("@protobuf_archive//:protobuf.bzl", "cc_proto_library")

cc_proto_library(
    name = "plan_backend_proto",
    srcs = ["plan_backend.proto"],
)

cc_library(
    name = "autofill_header",
    hdrs = ["autofill.h"],
    deps = [
    ],
)

cc_library(
    name = "autofill",
    srcs = ["autofill.cc"],
    deps = [
        ":loader",
        ":plan_utils",
        "//src/core:model_config_proto",
        "//src/core:server_header",
        "@org_tensorflow//tensorflow/core:lib",
    ],
)

cc_library(
    name = "loader",
    srcs = ["loader.cc"],
    hdrs = ["loader.h"],
    deps = [
        ":logging",
        "//src/core:server_header",
    ],
)

cc_library(
    name = "logging",
    srcs = ["logging.cc"],
    hdrs = ["logging.h"],
    deps = [
        "//src/core:server_header",
    ],
)

cc_library(
    name = "plan_utils",
    srcs = ["plan_utils.cc"],
    hdrs = ["plan_utils.h"],
    deps = [
        "//src/core:model_config",
    ],
)

cc_library(
    name = "plan_backend",
    srcs = ["plan_backend.cc"],
    hdrs = ["plan_backend.h"],
    deps = [
        ":loader",
        ":plan_utils",
        "//src/core:constants",
        "//src/core:model_config_cuda",
        "//src/core:model_config_proto",
        "//src/core:server_header",
    ],
)

cc_library(
    name = "plan_backend_factory",
    srcs = ["plan_backend_factory.cc"],
    hdrs = ["plan_backend_factory.h"],
    deps = [
        ":plan_backend",
        ":plan_backend_proto",
        "//src/core:constants",
        "//src/core:model_config",
        "//src/core:model_config_proto",
        "//src/core:server_header",
        "@org_tensorflow//tensorflow/core:lib",
    ],
    alwayslink = 1,
)

filegroup(
    name = "tensorrt_testdata",
    srcs = glob(
        ["testdata/**/*"]),
)

cc_test(
    name = "plan_backend_test",
    srcs = ["plan_backend_test.cc"],
    linkopts = [
        "-L/opt/tensorrtserver/lib",
        "-lcaffe2_gpu",
        "-lcaffe2",
        "-lnvinfer",
        "-L/usr/local/cuda/lib64/stubs",
        "-lnvidia-ml",
        "-lnvonnxparser_runtime"
    ],
    data = [
        ":tensorrt_testdata"
    ],
    deps = [
        "//src/test:model_config_test_base",
        "//src/core:server",
        "//src/test:testmain",
        "@local_config_cuda//cuda:cudart",
    ],
    tags = ["exclusive"],
)
