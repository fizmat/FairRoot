 ################################################################################
 #    Copyright (C) 2014 GSI Helmholtzzentrum fuer Schwerionenforschung GmbH    #
 #                                                                              #
 #              This software is distributed under the terms of the             #
 #              GNU Lesser General Public Licence (LGPL) version 3,             #
 #                  copied verbatim in the file "LICENSE"                       #
 ################################################################################

configure_file(${CMAKE_SOURCE_DIR}/parmq/parameter-server.json ${CMAKE_BINARY_DIR}/bin/config/parameter-server.json)

Set(INCLUDE_DIRECTORIES
  ${BASE_INCLUDE_DIRECTORIES}
  ${CMAKE_SOURCE_DIR}/parmq
  ${CMAKE_SOURCE_DIR}/base/MQ/policies/Serialization
  ${CMAKE_CURRENT_BINARY_DIR}
)

Set(SYSTEM_INCLUDE_DIRECTORIES
  ${SYSTEM_INCLUDE_DIRECTORIES}
  ${Boost_INCLUDE_DIR}
)

Include_Directories(${INCLUDE_DIRECTORIES})
Include_Directories(SYSTEM ${SYSTEM_INCLUDE_DIRECTORIES})

Set(LINK_DIRECTORIES
  ${Boost_LIBRARY_DIRS}
  ${ROOT_LIBRARY_DIR}
)

Link_Directories(${LINK_DIRECTORIES})

Set(SRCS
  ParameterMQServer.cxx
)

Set(DEPENDENCIES
  ${DEPENDENCIES}
  Base
  ParBase
  FairMQ::FairMQ
  ${Boost_THREAD_LIBRARY}
  ${Boost_PROGRAM_OPTIONS_LIBRARY}
)

Set(LIBRARY_NAME ParMQ)

GENERATE_LIBRARY()

Set(Exe_Names
  parmq-server
)

Set(Exe_Source
  runParameterMQServer.cxx
)

list(LENGTH Exe_Names _length)
math(EXPR _length ${_length}-1)

ForEach(_file RANGE 0 ${_length})
  list(GET Exe_Names ${_file} _name)
  list(GET Exe_Source ${_file} _src)
  Set(EXE_NAME ${_name})
  Set(SRCS ${_src})
  Set(DEPENDENCIES ParMQ)
  GENERATE_EXECUTABLE()
EndForEach(_file RANGE 0 ${_length})
