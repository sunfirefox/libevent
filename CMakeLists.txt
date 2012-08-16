cmake_minimum_required (VERSION 2.8)
PROJECT(libevent)

set (VERSION_MAJOR 2)
set (VERSION_MINOR 0)

find_package(OpenSSL REQUIRED)

add_definitions(-DHAVE_CONFIG_H -DSNAPPY)
if(WIN32)
add_definitions(-DUNICODE -D_UNICODE -DHAVE_CONFIG_H)
include_directories(${PROJECT_SOURCE_DIR}/WIN32-Code)
include_directories(${PROJECT_SOURCE_DIR}/include)
include_directories(${PROJECT_SOURCE_DIR}/compat)
include_directories(${PROJECT_SOURCE_DIR})
endif()

include_directories(OPENSSL_INCLUDE_DIR)

SET(CORE_SRCS event.c buffer.c bufferevent.c bufferevent_sock.c  	bufferevent_pair.c listener.c evmap.c log.c evutil.c  	strlcpy.c signal.c bufferevent_filter.c evthread.c  	bufferevent_ratelim.c evutil_rand.c evutil_time.c)
SET(WIN_SRCS win32select.c evthread_win32.c buffer_iocp.c  	event_iocp.c bufferevent_async.c)
SET(EXTRA_SRCS event_tagging.c http.c evdns.c evrpc.c)
SET(SSL_SRCS bufferevent_openssl.c)

ADD_LIBRARY(event_core STATIC ${CORE_SRCS} ${WIN_SRCS})
ADD_LIBRARY(event_extras STATIC ${EXTRA_SRCS})
ADD_LIBRARY(event STATIC ${CORE_SRCS} ${EXTRA_SRCS} ${WIN_SRCS})
ADD_LIBRARY(event_openssl STATIC ${SSL_SRCS})

ADD_SUBDIRECTORY(test)
