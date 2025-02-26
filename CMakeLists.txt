cmake_minimum_required(VERSION 3.24)

if (DEFINED ENV{METAMODULE_SDK_DIR})
    set(METAMODULE_SDK_DIR "$ENV{METAMODULE_SDK_DIR}")
elseif("${METAMODULE_SDK_DIR}" STREQUAL "")
    set(METAMODULE_SDK_DIR "${CMAKE_CURRENT_LIST_DIR}/../metamodule-plugin-sdk")
endif()
include(${METAMODULE_SDK_DIR}/plugin.cmake)

project(SeasideModular VERSION 1.0.0 DESCRIPTION "Seaside Modular Pack" LANGUAGES C CXX)

# Set C++17 standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Define source directories
set(SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/SeasideModularVCV)
set(DSP_DIR ${SOURCE_DIR}/dsp)

# Main library target
add_library(SeasideModular STATIC)

# Compiler definitions
target_compile_definitions(SeasideModular PRIVATE 
    __V1x 
    __PLUGIN
)

# Compiler options
target_compile_options(SeasideModular PRIVATE
    -fdata-sections
    -ffunction-sections
    -fno-exceptions
    -fno-threadsafe-statics
    -fno-use-cxa-atexit
    $<$<CXX_COMPILER_ID:GNU>:-fvisibility=hidden>
    $<$<CXX_COMPILER_ID:GNU>:-fvisibility-inlines-hidden>
)

# Linker options
target_link_options(SeasideModular PRIVATE
    -Wl,--gc-sections
    -static-libgcc
    -static-libstdc++
)

# Source files
target_sources(SeasideModular PRIVATE
    ${SOURCE_DIR}/src/plugin.cpp
    ${SOURCE_DIR}/src/Jawari.cpp
    ${SOURCE_DIR}/src/Tala.cpp
    ${SOURCE_DIR}/src/Raga.cpp
    ${SOURCE_DIR}/src/Proteus.cpp
    ${SOURCE_DIR}/src/ProteusX.cpp
    ${SOURCE_DIR}/inc/Gate.cpp
    ${SOURCE_DIR}/inc/Glide.cpp
    ${SOURCE_DIR}/inc/poisson.cpp
    # DaisySP source files
    ${SOURCE_DIR}/inc/DaisySP/Source/Control/adenv.cpp
    ${SOURCE_DIR}/inc/DaisySP/Source/Control/adsr.cpp
    ${SOURCE_DIR}/inc/DaisySP/Source/Control/line.cpp
    ${SOURCE_DIR}/inc/DaisySP/Source/Control/phasor.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Drums/analogbassdrum.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Drums/analogsnaredrum.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Drums/hihat.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Drums/synthbassdrum.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Drums/synthsnaredrum.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Dynamics/balance.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Dynamics/compressor.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Dynamics/crossfade.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Dynamics/limiter.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Effects/autowah.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Effects/bitcrush.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Effects/chorus.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Effects/decimator.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Effects/flanger.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Effects/fold.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Effects/overdrive.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Effects/reverbsc.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Effects/phaser.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Effects/sampleratereducer.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Effects/tremolo.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Filters/allpass.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Filters/atone.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Filters/biquad.cpp
    ${SOURCE_DIR}/inc/DaisySP/Source/Filters/comb.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Filters/mode.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Filters/moogladder.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Filters/nlfilt.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Filters/svf.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Filters/tone.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Noise/clockednoise.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Noise/grainlet.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Noise/particle.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/PhysicalModeling/drip.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/PhysicalModeling/modalvoice.cpp
    ${SOURCE_DIR}/inc/DaisySP/Source/PhysicalModeling/pluck.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/PhysicalModeling/resonator.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/PhysicalModeling/KarplusString.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/PhysicalModeling/stringvoice.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Synthesis/blosc.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Synthesis/fm2.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Synthesis/formantosc.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Synthesis/oscillator.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Synthesis/oscillatorbank.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Synthesis/variablesawosc.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Synthesis/variableshapeosc.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Synthesis/vosim.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Synthesis/zoscillator.cpp
    ${SOURCE_DIR}/inc/DaisySP/Source/Utility/dcblock.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Utility/jitter.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Utility/metro.cpp
    # ${SOURCE_DIR}/inc/DaisySP/Source/Utility/port.cpp
    # Add your module source files here
    ${SOURCE_DIR}/inc/libresample/src/resample.c
    ${SOURCE_DIR}/inc/libresample/src/filterkit.c  
    ${SOURCE_DIR}/inc/libresample/src/resamplesubs.c
)

# Include directories
target_include_directories(SeasideModular PRIVATE 
    ${SOURCE_DIR}
    ${SOURCE_DIR}/src
    ${SOURCE_DIR}/inc
    ${SOURCE_DIR}/inc/DaisySP/Source
    ${SOURCE_DIR}/inc/libresample/include
    ${DSP_DIR}
)

# Create the plugin
create_plugin(
    SOURCE_LIB      SeasideModular
    PLUGIN_NAME     SeasideModular
    PLUGIN_JSON     ${SOURCE_DIR}/plugin.json
    SOURCE_ASSETS   ${CMAKE_CURRENT_LIST_DIR}/assets
    DESTINATION     ${CMAKE_CURRENT_LIST_DIR}/metamodule-plugins
) 