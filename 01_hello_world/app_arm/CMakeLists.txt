# SPDX-License-Identifier: Apache-2.0

cmake_minimum_required(VERSION 3.20.0)

# Find external Zephyr project, and load its settings
find_package(Zephyr REQUIRED HINTS $ENV{ZEPHYR_BASE})

# Set project name
project(hello_world)

# Add sources
target_sources(app PRIVATE src/main.c)
