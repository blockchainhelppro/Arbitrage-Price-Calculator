# Copyright 2019 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      https://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

load(
    "//bindings/python:build_defs.oss.bzl",
    "NUMPY_DEPS",
    "PYBIND_COPTS",
    "PYBIND_EXTENSION_COPTS",
    "PYBIND_FEATURES",
    "iree_py_extension",
    "iree_py_library",
    "iree_py_test",
    "pybind_cc_library",
)

package(
    default_visibility = ["//visibility:public"],
    licenses = ["notice"],  # Apache 2.0
)

COMPILER_DEPS = [
    # Transforms.
    "//iree/compiler/Dialect/Flow/Transforms",
    "//iree/compiler/Dialect/HAL/Transforms",
    "//iree/compiler/Dialect/HAL/Target:ExecutableTarget",
    "//iree/compiler/Dialect/VM/Transforms",

    # Targets.
    "//iree/compiler/Dialect/HAL/Target/LegacyInterpreter",
    "//iree/compiler/Dialect/HAL/Target/VMLA",
    "//iree/compiler/Dialect/HAL/Target/VulkanSPIRV",
    "//iree/compiler/Dialect/VM/Target/Bytecode",
]

iree_py_library(
    name = "compiler",
    srcs = [
        "__init__.py",
    ],
    srcs_version = "PY3",
    tags = ["nokokoro"],
    deps = [
        ":binding",
        "//bindings/python:pathsetup",  # build_cleaner: keep
    ],
)

iree_py_extension(
    name = "binding",
    srcs = [
        "initialize_module.cc",
    ],
    copts = PYBIND_COPTS + PYBIND_EXTENSION_COPTS,
    features = PYBIND_FEATURES,
    linkstatic = 1,
    tags = ["nokokoro"],
    win_def_file = "export.def",
    deps = [
        ":compiler_library",
        "//bindings/python/pyiree/common",
    ],
)

pybind_cc_library(
    name = "compiler_library",
    srcs = [
        "compiler.cc",
    ],
    hdrs = [
        "compiler.h",
    ],
    tags = ["nokokoro"],
    deps = COMPILER_DEPS + [
        "//bindings/python/pyiree/common",
        "@llvm-project//llvm:support",
        "@llvm-project//mlir:IR",
        "@llvm-project//mlir:Parser",
        "@llvm-project//mlir:Pass",
    ],
)

iree_py_test(
    name = "compiler_test",
    srcs = ["compiler_test.py"],
    python_version = "PY3",
    tags = ["nokokoro"],
    deps = NUMPY_DEPS + [
        "//bindings/python:pathsetup",  # build_cleaner: keep
        "@absl_py//absl/testing:absltest",
        "//bindings/python/pyiree/compiler",
    ],
)
