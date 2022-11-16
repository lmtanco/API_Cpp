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

# Look for curl, sz and z libraries
# TODO: Consider FindCURL functionality of CMake. 
# In MacOS brew install curl leaves it in /usr/lcoal/opt/curl/lib
# find_library(CURL_LIB NAMES libcurl.a HINTS )
find_package(CURL REQUIRED)
#no package for szip. 
#find_package(SZIP REQUIRED)
find_library(SZIP_LIB NAMES libsz.a)
find_library(Z_LIB 
    NAMES libz.a
    HINTS /usr/local/opt/zlib/lib
    )

# Look for hdf5 headers - not required I think 
# find_path(HDF5_HEADER NAMES hdf5.h)

# Look for hdf5 libraries
find_library(HDF5_LIB NAMES libhdf5.a)
find_library(HDF5_HL_LIB NAMES libhdf5_hl.a)


# Look for netcdf headers
find_path(NETCDF_HEADER NAMES netcdf.h)

# Look for netcdf library
find_library(NETCDF_LIB NAMES libnetcdf.a)

# Look for netcdf-cxx4 headers
find_path(NETCDF_CXX_HEADER NAMES ncFile.h)

# Look for netcdf-cxx libraries
find_library(NETCDF_CXX_LIB NAMES libnetcdf-cxx4.a)

target_include_directories(libsofa SYSTEM
  #  PUBLIC ${HDF5_HEADER}
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
    PUBLIC ${SZIP_LIB}
    PUBLIC ${Z_LIB}
 )