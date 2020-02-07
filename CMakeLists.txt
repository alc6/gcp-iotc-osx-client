cmake_minimum_required(VERSION 3.00)

# set the project name and version
project(OSX_GCP_IOT_CLIENT VERSION 1.0)

set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED True)

configure_file(config.h.in config.h)

# etup the paths variables
set(GOOGLE_IOT_SDK ${CMAKE_CURRENT_LIST_DIR}/iot-device-sdk-embedded-c)
set(MBEDTLS_DIR ${GOOGLE_IOT_SDK}/third_party/tls/mbedtls)

# Following mbedtls example on how to add mbedtls as a subdirectory
## Begin
# Link against all the Mbed TLS libraries.
set(libs
    mbedtls
    mbedcrypto
    mbedx509
)

#include_directories("${GOOGLE_IOT_SDK}/third_party/tls/mbedtls/")
#link_directories("${GOOGLE_IOT_SDK}/third_party/tls/mbedtls/")
add_subdirectory(${MBEDTLS_DIR} build)
## End

# Build the Google IoT Core SDK
# 1.a Set variables (include dirs)
set (COMPONENT_ADD_INCLUDEDIRS
        "${GOOGLE_IOT_SDK}/src/bsp"
        "${GOOGLE_IOT_SDK}/include/bsp"
        "${GOOGLE_IOT_SDK}/include"
        "${GOOGLE_IOT_SDK}/src/libiotc/control_topic"
        "${GOOGLE_IOT_SDK}/src/libiotc/datastructures"
        "${GOOGLE_IOT_SDK}/src/libiotc/debug_extensions/memory_limiter"
        "${GOOGLE_IOT_SDK}/src/libiotc/event_dispatcher"
        "${GOOGLE_IOT_SDK}/src/libiotc/event_loop"
        "${GOOGLE_IOT_SDK}/src/libiotc/io/fs/memory"
        "${GOOGLE_IOT_SDK}/src/libiotc/io/fs"
        "${GOOGLE_IOT_SDK}/src/libiotc/io/net"
        "${GOOGLE_IOT_SDK}/src/libiotc/mqtt/logic"
        "${GOOGLE_IOT_SDK}/src/libiotc/mqtt/codec"
        "${GOOGLE_IOT_SDK}/src/libiotc/platform/iotc_thread"
        "${GOOGLE_IOT_SDK}/src/libiotc/platform/posix/iotc_thread"
        "${GOOGLE_IOT_SDK}/src/libiotc/tls"
        "${GOOGLE_IOT_SDK}/src/libiotc/tls/certs"
        "${GOOGLE_IOT_SDK}/src/libiotc/memory"
        "${GOOGLE_IOT_SDK}/src/libiotc"
        "${GOOGLE_IOT_SDK}/third_party/mqtt-protocol-c"
        "${GOOGLE_IOT_SDK}/third_party/tls/mbedtls/include"
        "${GOOGLE_IOT_SDK}/examples/"
    )
# 1.a Set variables (sources)
set (SOURCES
        iot-device-sdk-embedded-c/src/bsp/crypto/mbedtls/iotc_bsp_crypto_mbedtls.c
        iot-device-sdk-embedded-c/src/libiotc/io/fs/posix/iotc_fs_posix.c
        iot-device-sdk-embedded-c/src/libiotc/debug_extensions/memory_limiter/iotc_memory_limiter.c
        iot-device-sdk-embedded-c/src/bsp/platform/posix/iotc_bsp_io_fs_posix.c
        iot-device-sdk-embedded-c/src/bsp/platform/posix/iotc_bsp_io_net_posix.c
        iot-device-sdk-embedded-c/src/bsp/platform/posix/iotc_bsp_mem_posix.c
        iot-device-sdk-embedded-c/src/bsp/platform/posix/iotc_bsp_rng_posix.c
        iot-device-sdk-embedded-c/src/bsp/platform/posix/iotc_bsp_time_posix.c
        iot-device-sdk-embedded-c/src/libiotc/io/net/iotc_io_net_layer.c
        iot-device-sdk-embedded-c/src/libiotc/memory/iotc_allocator.c
        iot-device-sdk-embedded-c/src/libiotc/event_loop/iotc_event_loop.c
        iot-device-sdk-embedded-c/src/libiotc/tls/certs/iotc_RootCA_list.c
        iot-device-sdk-embedded-c/src/libiotc/tls/iotc_tls_layer.c
        iot-device-sdk-embedded-c/src/bsp/tls/mbedtls/iotc_bsp_tls_mbedtls.c
        iot-device-sdk-embedded-c/src/libiotc/iotc.c
        iot-device-sdk-embedded-c/src/libiotc/iotc_backoff_status_api.c
        iot-device-sdk-embedded-c/src/libiotc/iotc_connection_data.c
        iot-device-sdk-embedded-c/src/libiotc/iotc_data_desc.c
        iot-device-sdk-embedded-c/src/libiotc/iotc_error.c
        iot-device-sdk-embedded-c/src/libiotc/iotc_globals.c
        iot-device-sdk-embedded-c/src/libiotc/iotc_handle.c
        iot-device-sdk-embedded-c/src/libiotc/iotc_helpers.c
        iot-device-sdk-embedded-c/src/libiotc/iotc_internals.c
        iot-device-sdk-embedded-c/src/libiotc/iotc_io_timeouts.c
        iot-device-sdk-embedded-c/src/libiotc/iotc_jwt.c
        iot-device-sdk-embedded-c/src/libiotc/iotc_layer_api.c
        iot-device-sdk-embedded-c/src/libiotc/iotc_layer_default_allocators.c
        iot-device-sdk-embedded-c/src/libiotc/iotc_layer_default_functions.c
        iot-device-sdk-embedded-c/src/libiotc/iotc_timed_task.c
        iot-device-sdk-embedded-c/src/libiotc/iotc_tuples.c
        iot-device-sdk-embedded-c/src/libiotc/iotc_user_sub_call_wrapper.c
        iot-device-sdk-embedded-c/src/libiotc/io/fs/iotc_fs_bsp_to_iotc_mapping.c
        iot-device-sdk-embedded-c/src/libiotc/io/fs/iotc_fs_filenames.c
        iot-device-sdk-embedded-c/src/libiotc/io/fs/iotc_resource_manager.c
        iot-device-sdk-embedded-c/src/libiotc/event_dispatcher/iotc_event_dispatcher.c
        iot-device-sdk-embedded-c/src/libiotc/event_dispatcher/iotc_event_handle.c
        iot-device-sdk-embedded-c/src/libiotc/event_dispatcher/iotc_event_thread_dispatcher.c
        iot-device-sdk-embedded-c/src/libiotc/event_dispatcher/iotc_time_event.c
        iot-device-sdk-embedded-c/src/libiotc/datastructures/iotc_vector.c
        iot-device-sdk-embedded-c/src/libiotc/mqtt/codec/iotc_mqtt_codec_layer.c
        iot-device-sdk-embedded-c/src/libiotc/mqtt/codec/iotc_mqtt_codec_layer_data.c
        iot-device-sdk-embedded-c/src/libiotc/mqtt/logic/iotc_mqtt_logic_layer.c
        iot-device-sdk-embedded-c/src/libiotc/mqtt/logic/iotc_mqtt_logic_layer_data.c
        iot-device-sdk-embedded-c/src/libiotc/mqtt/logic/iotc_mqtt_logic_layer_keepalive_handler.c
        iot-device-sdk-embedded-c/src/libiotc/mqtt/logic/iotc_mqtt_logic_layer_task_helpers.c
        iot-device-sdk-embedded-c/src/libiotc/control_topic/iotc_control_topic_layer.c
        iot-device-sdk-embedded-c/third_party/mqtt-protocol-c/iotc_mqtt_message.c
        iot-device-sdk-embedded-c/third_party/mqtt-protocol-c/iotc_mqtt_parser.c
        iot-device-sdk-embedded-c/third_party/mqtt-protocol-c/iotc_mqtt_serialiser.c
        iot-device-sdk-embedded-c/examples/common/src/commandline.c
        iot-device-sdk-embedded-c/examples/common/src/example_utils.c
    )

#2. Include directories
include_directories(${COMPONENT_ADD_INCLUDEDIRS})

#3. Add compile definitions for the Google Core IoT SDK
add_definitions(-D IOTC_FS_POSIX
                    -DIOTC_BSP_TLS=mbedtls
                    -DIOTC_TLS_LIB_MBEDTLS
                    -DIOTC_FS_MEMORY
                    -DIOTC_MEMORY_LIMITER_APPLICATION_MEMORY_LIMIT=524288
                    -DIOTC_MEMORY_LIMITER_SYSTEM_MEMORY_LIMIT=2024
                    -DIOTC_MEMORY_LIMITER_ENABLED
                )

        

add_executable(osx_gcp_iot_client ${SOURCES} main.c)

target_link_libraries(osx_gcp_iot_client ${libs})

configure_file(build/osx_gcp_iot_client bin/osx_gcp_iot_client COPYONLY)

#Move the executable from build to bin for a clearer file organization
add_custom_command(
        TARGET osx_gcp_iot_client POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy
                ${CMAKE_SOURCE_DIR}/build/osx_gcp_iot_client
                ${CMAKE_SOURCE_DIR}/bin/osx_gcp_iot_client)