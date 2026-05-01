# SPDX-License-Identifier: Apache-2.0

cmake_minimum_required(VERSION 3.20.0)
find_package(Zephyr REQUIRED HINTS $ENV{ZEPHYR_BASE})
project(espnow_sample)

target_sources(app PRIVATE src/main.c)

# libespnow.a is in hal_espressif blobs but not linked by default
zephyr_link_libraries(espnow)
