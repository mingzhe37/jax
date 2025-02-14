# Copyright 2023 The JAX Authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     https://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

load(
    "//jaxlib:jax.bzl",
    "jax_test",
    "py_deps",
)
load("@rules_python//python:defs.bzl", "py_test")

licenses(["notice"])

package(
    default_applicable_licenses = [],
    default_visibility = ["//visibility:private"],
)

jax_test(
    name = "pallas_test",
    srcs = [
        "pallas_test.py",
    ],
    backend_tags = {
        "gpu": ["noasan"],  # https://github.com/openai/triton/issues/2918
    },
    config_tags_overrides = {
        "gpu_x32": {
            "ondemand": False,  # Include in presubmit.
        },
    },
    disable_backends = [
        "cpu",
        "tpu",
    ],
    disable_configs = [
        "gpu",
        "gpu_a100",
        "gpu_p100",
    ],
    enable_configs = [
        "gpu_x32",
        "gpu_a100_x32",
    ],
    shard_count = 4,
    deps = [
        "//third_party/py/jax:pallas_gpu",
    ] + py_deps("absl/testing") + py_deps("numpy"),
)

py_test(
    name = "indexing_test",
    srcs = [
        "indexing_test.py",
    ],
    deps = [
        "//third_party/py/jax:pallas",
    ] + py_deps("absl/testing") + py_deps("hypothesis") + py_deps("numpy"),
)

jax_test(
    name = "all_gather_test",
    srcs = [
        "all_gather_test.py",
    ],
    disable_backends = [
        "cpu",
        "gpu",
    ],
    deps = [
        "//third_party/py/jax:pallas_tpu_ops",
    ] + py_deps("absl/testing") + py_deps("numpy") + py_deps("hypothesis"),
)

jax_test(
    name = "splash_attention_kernel_test",
    srcs = [
        "splash_attention_kernel_test.py",
    ],
    disable_backends = [
        "cpu",
        "gpu",
    ],
    shard_count = 18,
    tags = [
        "noasan",  # Times out.
        "nomsan",  # Times out.
        "notsan",  # Times out.
    ],
    deps = [
        "//third_party/py/jax:pallas_tpu_ops",
    ] + py_deps("absl/testing") + py_deps("numpy") + py_deps("hypothesis"),
)

jax_test(
    name = "splash_attention_mask_test",
    srcs = [
        "splash_attention_mask_test.py",
    ],
    disable_backends = [
        "cpu",
        "gpu",
    ],
    deps = [
        "//third_party/py/jax:pallas_tpu_ops",
    ] + py_deps("absl/testing") + py_deps("numpy") + py_deps("hypothesis"),
)

jax_test(
    name = "pallas_call_tpu_test",
    srcs = ["pallas_call_tpu_test.py"],
    disable_backends = [
        "gpu",
    ],
    main = "pallas_call_tpu_test.py",
    deps = [
        "//third_party/py/jax:extend",
        "//third_party/py/jax:pallas_tpu",
        "//third_party/py/jax:pallas_tpu_ops",
    ],
)
