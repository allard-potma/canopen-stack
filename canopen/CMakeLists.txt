set(CANOPEN_PROJ_DIR "${CMAKE_CURRENT_SOURCE_DIR}")

# The following five lines of boilerplate have to be in your project's
# CMakeLists in this exact order for cmake to work correctly
set(srcs
    "${CANOPEN_PROJ_DIR}/source/co_core.c"
	"${CANOPEN_PROJ_DIR}/source/co_csdo.c"
	"${CANOPEN_PROJ_DIR}/source/co_dict.c"
	"${CANOPEN_PROJ_DIR}/source/co_emcy.c"
	"${CANOPEN_PROJ_DIR}/source/co_if_can.c"
	"${CANOPEN_PROJ_DIR}/source/co_if_nvm.c"
	"${CANOPEN_PROJ_DIR}/source/co_if_timer.c"
	"${CANOPEN_PROJ_DIR}/source/co_if.c"
	"${CANOPEN_PROJ_DIR}/source/co_lss.c"
	"${CANOPEN_PROJ_DIR}/source/co_nmt.c"
	"${CANOPEN_PROJ_DIR}/source/co_para.c"
	"${CANOPEN_PROJ_DIR}/source/co_pdo.c"
	"${CANOPEN_PROJ_DIR}/source/co_ssdo.c"
	"${CANOPEN_PROJ_DIR}/source/co_csdo.c"
	"${CANOPEN_PROJ_DIR}/source/co_sync.c"
	"${CANOPEN_PROJ_DIR}/source/co_tmr.c"
	"${CANOPEN_PROJ_DIR}/source/co_ver.c"
)

set(include_dirs "${CANOPEN_PROJ_DIR}/config"
				 "${CANOPEN_PROJ_DIR}/include")

set(priv_include_dirs "${CANOPEN_PROJ_DIR}/source")

#check if the user wants to override co_obj.c
if(EXISTS "${PROJECT_DIR}/main/co_obj.c")
    list(APPEND srcs "${PROJECT_DIR}/main/co_obj.c")
    message(STATUS "Override co_obj from ${PROJECT_DIR} folder.")
else()
    list(APPEND srcs "${CANOPEN_PROJ_DIR}/source/co_obj.c")
    message(STATUS "No co_obj override")
endif()

message(STATUS "Sources: ${srcs}.")
message(STATUS "Includes: ${priv_include_dirs}.")

idf_component_register(SRCS "${srcs}"
                    INCLUDE_DIRS "${include_dirs}"
                    PRIV_INCLUDE_DIRS "${priv_include_dirs}"
                    PRIV_REQUIRES freemodbus)
 
#the following will tell the linker that COLssStrore is an external function. Other callback will work also after this,.                   
target_link_libraries(${COMPONENT_LIB} INTERFACE "-u COLssStore")
