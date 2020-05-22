set (SOURCES
  callback_manager.cpp
  connection.cpp
  connection_impl.cpp
  default_http_client.cpp
  default_websocket_client.cpp
  handshake_protocol.cpp
  hub_connection.cpp
  hub_connection_builder.cpp
  hub_connection_impl.cpp
  json_helpers.cpp
  json_hub_protocol.cpp
  logger.cpp
  negotiate.cpp
  signalr_client_config.cpp
  signalr_value.cpp
  stdafx.cpp
  trace_log_writer.cpp
  transport.cpp
  transport_factory.cpp
  url_builder.cpp
  websocket_transport.cpp
  ../../third_party_code/cpprestsdk/uri.cpp
  ../../third_party_code/cpprestsdk/uri_builder.cpp
  ../../third_party_code/jsoncpp/jsoncpp.cpp
)

include_directories(
    ../../third_party_code/cpprestsdk
    ../../third_party_code/jsoncpp)

add_library (signalrclient SHARED ${SOURCES})

if(NOT USE_CPPRESTSDK)
  target_link_libraries(signalrclient pthread)
else()
  if(OPENSSL_FOUND)
    message("--   OpenSSL")
    target_link_libraries(signalrclient pthread ${CPPREST_LIB} boost_system boost_chrono boost_thread ${OPENSSL_LIBRARIES})
  else()
    target_link_libraries(signalrclient pthread ${CPPREST_LIB} boost_system boost_chrono boost_thread)
  endif()
endif() # USE_CPPRESTSDK

include(GNUInstallDirs)

install(TARGETS signalrclient
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
  LIBRARY DESTINATION /usr/lib
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
)
