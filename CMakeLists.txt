cmake_minimum_required(VERSION 2.8)
project(wormhole)
include_directories(${PROJECT_SOURCE_DIR}/fflib ${PROJECT_SOURCE_DIR}/lib3party ${PROJECT_SOURCE_DIR}/src  
            ${PROJECT_SOURCE_DIR}/lib3party/php ${PROJECT_SOURCE_DIR}/lib3party/php/embed 
            ${PROJECT_SOURCE_DIR}/lib3party/php/main ${PROJECT_SOURCE_DIR}/lib3party/php/Zend
            ${PROJECT_SOURCE_DIR}/lib3party/php/TSRM
            /usr/include/mysql/ /usr/include/python2.7/
)
link_directories(/usr/local/lib /usr/lib64/mysql ${PROJECT_SOURCE_DIR})

set(ENGINE_LIB_SRC 
    ${PROJECT_SOURCE_DIR}/fflib/db/sqlite3.c
    ${PROJECT_SOURCE_DIR}/fflib/db/sqlite_ops.cpp
    ${PROJECT_SOURCE_DIR}/fflib/db/ffdb.cpp
    ${PROJECT_SOURCE_DIR}/fflib/db/mysql_ops.cpp
    ${PROJECT_SOURCE_DIR}/fflib/net/socketimpl/socket_win.cpp
    ${PROJECT_SOURCE_DIR}/fflib/net/socketimpl/socket_linux.cpp
    ${PROJECT_SOURCE_DIR}/fflib/net/eventloopimpl/epoll.cpp
    ${PROJECT_SOURCE_DIR}/fflib/net/eventloopimpl/select.cpp
    ${PROJECT_SOURCE_DIR}/fflib/net/ctrlimpl/socket_ctrl_gate.cpp
    ${PROJECT_SOURCE_DIR}/fflib/net/ctrlimpl/socket_ctrl_common.cpp
    ${PROJECT_SOURCE_DIR}/fflib/net/acceptorimpl/acceptor_linux_gate.cpp
    ${PROJECT_SOURCE_DIR}/fflib/net/acceptorimpl/acceptor_linux.cpp
    ${PROJECT_SOURCE_DIR}/fflib/net/codec.cpp
    ${PROJECT_SOURCE_DIR}/fflib/net/net_stat.cpp
    ${PROJECT_SOURCE_DIR}/fflib/base/lock.cpp
    ${PROJECT_SOURCE_DIR}/fflib/base/event_bus.cpp
    ${PROJECT_SOURCE_DIR}/fflib/base/thread.cpp
    ${PROJECT_SOURCE_DIR}/fflib/base/log.cpp
    ${PROJECT_SOURCE_DIR}/fflib/base/performance_daemon.cpp
    ${PROJECT_SOURCE_DIR}/fflib/rpc/ffrpc.cpp
    ${PROJECT_SOURCE_DIR}/fflib/rpc/ffbroker.cpp
    ${PROJECT_SOURCE_DIR}/fflib/rpc/msg_def/ffrpc_msg_types.cpp
    ${PROJECT_SOURCE_DIR}/fflib/rpc/msg_def/ffrpc_msg_constants.cpp
    ${PROJECT_SOURCE_DIR}/fflib/server/gen-cpp/msg_constants.cpp
    ${PROJECT_SOURCE_DIR}/fflib/server/gen-cpp/msg_types.cpp
    ${PROJECT_SOURCE_DIR}/fflib/server/ffgate.cpp
    ${PROJECT_SOURCE_DIR}/fflib/server/http_mgr.cpp
    ${PROJECT_SOURCE_DIR}/fflib/server/db_mgr.cpp
    ${PROJECT_SOURCE_DIR}/fflib/server/ffworker.cpp
    ${PROJECT_SOURCE_DIR}/fflib/server/shared_mem.cpp
    ${PROJECT_SOURCE_DIR}/fflib/xml/tinystr.cpp
    ${PROJECT_SOURCE_DIR}/fflib/xml/tinyxml.cpp
    ${PROJECT_SOURCE_DIR}/fflib/xml/tinyxmlerror.cpp
    ${PROJECT_SOURCE_DIR}/fflib/xml/ffxml.cpp
    ${PROJECT_SOURCE_DIR}/fflib/xml/tinyxmlparser.cpp
)
set(GAME_EXAMPLE_SRC
    ${PROJECT_SOURCE_DIR}/src/common/entity.cpp
    ${PROJECT_SOURCE_DIR}/src/player/cmd_player.cpp
    ${PROJECT_SOURCE_DIR}/src/player/db_player.cpp
    ${PROJECT_SOURCE_DIR}/src/map/map.cpp
    ${PROJECT_SOURCE_DIR}/src/task/task.cpp
    ${PROJECT_SOURCE_DIR}/src/item/item.cpp
    ${PROJECT_SOURCE_DIR}/src/npc/npc.cpp
)
set(ENGINE_SRC_LIST 
    ${ENGINE_LIB_SRC}
    ${PROJECT_SOURCE_DIR}/engine/main.cpp
)
set(WORKER_PY_SRC_LIST 
    ${ENGINE_LIB_SRC} ${GAME_EXAMPLE_SRC}
    ${PROJECT_SOURCE_DIR}/workerpy/ffworker_python.cpp
    ${PROJECT_SOURCE_DIR}/workerpy/main.cpp
)
set(WORKER_LUA_SRC_LIST 
    ${ENGINE_LIB_SRC} ${GAME_EXAMPLE_SRC}
    ${PROJECT_SOURCE_DIR}/workerlua/ffworker_lua.cpp
    ${PROJECT_SOURCE_DIR}/workerlua/main.cpp
)
set(WORKER_JS_SRC_LIST 
    ${ENGINE_LIB_SRC} ${GAME_EXAMPLE_SRC}
    ${PROJECT_SOURCE_DIR}/workerjs/ffworker_js.cpp
    ${PROJECT_SOURCE_DIR}/workerjs/main.cpp
)
set(WORKER_PHP_SRC_LIST 
    ${ENGINE_LIB_SRC} ${GAME_EXAMPLE_SRC}
    ${PROJECT_SOURCE_DIR}/workerphp/ffworker_php.cpp
    ${PROJECT_SOURCE_DIR}/workerphp/main.cpp
)
add_executable(worm ${ENGINE_SRC_LIST})
target_link_libraries(worm pthread dl mysqlclient crypto ssl z curl)

add_executable(pyhole ${WORKER_PY_SRC_LIST})
target_link_libraries(pyhole pthread dl mysqlclient crypto ssl z curl python2.7)

add_executable(luahole ${WORKER_LUA_SRC_LIST})
target_link_libraries(luahole pthread dl mysqlclient crypto ssl z curl lua)

add_executable(jshole ${WORKER_JS_SRC_LIST})
target_link_libraries(jshole pthread dl mysqlclient crypto ssl z curl v8)

add_executable(phphole ${WORKER_PHP_SRC_LIST})
target_link_libraries(phphole pthread dl mysqlclient crypto ssl z curl php5)


