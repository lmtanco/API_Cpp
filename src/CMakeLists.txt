add_library(libsofa 
    SOFAAPI.cpp
    SOFAAttributes.cpp
    SOFACoordinates.cpp
    SOFADate.cpp
    SOFAEmitter.cpp
    SOFAExceptions.cpp
    SOFAFile.cpp
    SOFAGeneralFIR.cpp
    SOFAGeneralFIRE.cpp
    SOFAGeneralTF.cpp
    SOFAHelper.cpp
    SOFAListener.cpp
    SOFAMultiSpeakerBRIR.cpp
    SOFANcFile.cpp
    SOFAPoint3.cpp
    SOFAPosition.cpp
    SOFAReceiver.cpp
    SOFASimpleFreeFieldHRIR.cpp
    SOFASimpleFreeFieldSOS.cpp
    SOFASimpleHeadphoneIR.cpp
    SOFASingleRoomDRIR.cpp
    SOFASource.cpp
    SOFAString.cpp
    SOFAUnits.cpp
    )

# Look for curl and z libraries
# In MacOS brew install curl leaves it in /usr/lcoal/opt/curl/lib
find_package(CURL REQUIRED)

# In MacOS removed SZIP depedndency (see below) 
# find_library(SZIP_LIB NAMES libsz.a)

#In MacOS brew install zlib 
find_library(Z_LIB 
    NAMES libz.a
    HINTS /usr/local/opt/zlib/lib
    )

# Look for hdf5 libraries
# In MacOS compiled without SZLIB from develop branch
# of https://github.com/HDFGroup/hdf5.git
# with cmake .. -DHDF5_ENABLE_SZIP_SUPPORT:BOOL=OFF
find_library(HDF5_LIB NAMES libhdf5.a)
find_library(HDF5_HL_LIB NAMES libhdf5_hl.a)

# Look for netcdf headers
# In MacOS compiled from master branch 
# of https://github.com/Unidata/netcdf-c.git 
find_path(NETCDF_HEADER NAMES netcdf.h)

# Look for netcdf library
# In MacOS compiled from master branch 
# of https://github.com/Unidata/netcdf-c.git
find_library(NETCDF_LIB NAMES libnetcdf.a)

# Look for netcdf-cxx4 headers
# In MacOS compiled from master branch
# of https://github.com/Unidata/netcdf-cxx4.git
find_path(NETCDF_CXX_HEADER NAMES ncFile.h)

# Look for netcdf-cxx libraries
# In MacOS compiled from master branch
# of https://github.com/Unidata/netcdf-cxx4.git
# changed in CMakeLists.txt BUILD_SHARED_LIBS to OFF
find_library(NETCDF_CXX_LIB NAMES libnetcdf-cxx4.a)

target_include_directories(libsofa SYSTEM # not sure SYSTEM is needed. 
    PUBLIC ${NETCDF_HEADER}
    PUBLIC ${NETCDF_CXX_HEADER})

target_include_directories(libsofa 
    PUBLIC $(CMAKE_CURRENT_SOURCE_DIR)
 )                            

 target_link_libraries(libsofa
    PUBLIC ${HDF5_LIB}
    PUBLIC ${HDF5_HL_LIB}
    PUBLIC ${NETCDF_LIB}
    PUBLIC ${NETCDF_CXX_LIB}
    PUBLIC ${CURL_LIBRARIES}
  #  PUBLIC ${SZIP_LIB} # In MacOS not required. 
    PUBLIC ${Z_LIB}
 )