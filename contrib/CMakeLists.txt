# ================================================================================================
# Download
# ================================================================================================
function(cat IN_FILE OUT_FILE)
    file(READ ${IN_FILE} CONTENTS)
    file(APPEND ${OUT_FILE} "${CONTENTS}")
endfunction(cat IN_FILE OUT_FILE)

set(CONTRIB_TMP_FILE "${CMAKE_CURRENT_SOURCE_DIR}/../cmake/deps_tmp_CMakeLists.txt.in")
configure_file("${CMAKE_CURRENT_SOURCE_DIR}/../cmake/deps_CMakeLists.txt.in" ${CONTRIB_TMP_FILE})

cat("${CMAKE_CURRENT_SOURCE_DIR}/../cmake/jansson_CMakeLists.txt.in" ${CONTRIB_TMP_FILE})

message(${CONTRIB_TMP_FILE})
# download dependencies
configure_file(${CONTRIB_TMP_FILE} "${CMAKE_CURRENT_SOURCE_DIR}/../contrib/deps-download/CMakeLists.txt")
execute_process(COMMAND "${CMAKE_COMMAND}" -G "${CMAKE_GENERATOR}" .
        WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/../contrib/deps-download")
execute_process(COMMAND "${CMAKE_COMMAND}" --build .
        WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/../contrib/deps-download")