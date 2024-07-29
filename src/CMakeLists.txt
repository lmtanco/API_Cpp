# This CMakeLists.txt has been tested in: 
# - MacOS Monterey
# - Windows 10
#
# This version of libsofa relies on a series of libraries installed in your system. 
#
# For MacOS you can install some libraries with "brew install <library>" 
# for others you will have to "git clone" the library, then create build files with "cmake", 
# build with "make" and install with "make install". 
# 
# For Windows you can install all libraries with vcpkg.
# start with netcdf-cxx4 because this might install all the other dependencies.
# then run cmake like this cmake .. -DCMAKE_TOOLCHAIN_FILE=C:\vcpkg\scripts\buildsystems\vcpkg.cmake -DVCPKG_TARGET_TRIPLET=x64-windows-static 

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
# In Windows you can ttry to insttall curl with vcpkg install curl:x64-windows-static
# To use with cmake: 
# cmake .. -DCMAKE_TOOLCHAIN_FILE=C:\vcpkg\scripts\buildsystems\vcpkg.cmake -DVCPKG_TARGET_TRIPLET=x64-windows-static 
# Alteratively git clone, cmake with BUILD_SHARED_LIBS=OFF, CMAKE_INSTALL_PREFIX=C:\extlibs (for example), then install
# in Visual Studio by compiling the INSTALL visual studio project.
# Then when configuring this library use the same path but with set(CMAKE_PREFIX_PATH C:\extlibs)
find_package(CURL)
if (NOT CURL_FOUND) 
    find_library(CURL_LIBRARIES
        NAMES libcurl.a libcurl.lib
        HINTS ${CMAKE_PREFIX_PATH}/lib /usr/local/opt/curl/lib
        )
endif()



# In MacOS removed SZIP depedndency (see below) 
# find_library(SZIP_LIB NAMES libsz.a)

#In MacOS brew install zlib 
find_library(Z_LIB 
    NAMES libz.a zlib.lib
    HINTS /usr/local/opt/zlib/lib
    )

# Look for hdf5 libraries
# In MacOS compiled without SZLIB from develop branch
# of https://github.com/HDFGroup/hdf5.git
# with cmake .. -DHDF5_ENABLE_SZIP_SUPPORT:BOOL=OFF
# In Windows you can try to install with vcpkg install hdf5[core,cpp,zlib]:x64-windows-static
# Alteratively git clone, git switch  cmake with BUILD_SHARED_LIBS=OFF,HDF5_ENABLE_SZIP_SUPPORT=ON, 
# CMAKE_INSTALL_PREFIX=C:\extlibs (for example), then install
# in Visual Studio by compiling the INSTALL visual studio project.
find_library(HDF5_LIB NAMES libhdf5.a libhdf5.lib)
find_library(HDF5_HL_LIB NAMES libhdf5_hl.a libhdf5_hl.lib)

# Look for netcdf headers
# In MacOS compiled from master branch 
# of https://github.com/Unidata/netcdf-c.git 
# In Windows it's very hard to compile netcdf from source.
# You need to have m4 installed and in your path. Download from https://gnuwin32.sourceforge.net/packages/m4.htm
# git clone, cmake with BUILD_SHARED_LIBS=OFF, ENABLE_DAP=OFF, ENABLE_DAP2=OFF, ENABLE_DAP4=OFF, NETCDF_ENABLE_BYTERANGE=OFF
# CMAKE_INSTALL_PREFIX=C:\extlibs (for example), then install
# in Visual Studio by compiling the INSTALL visual studio project.
find_path(NETCDF_HEADER NAMES netcdf.h)

# Look for netcdf library
# In MacOS compiled from master branch 
# of https://github.com/Unidata/netcdf-c.git
find_library(NETCDF_LIB NAMES libnetcdf.a netcdf.lib)

# Look for netcdf-cxx4 headers
# In MacOS compiled from master branch
# of https://github.com/Unidata/netcdf-cxx4.git
find_path(NETCDF_CXX_HEADER NAMES ncFile.h)

# Look for netcdf-cxx libraries
# In MacOS compiled from master branch
# of https://github.com/Unidata/netcdf-cxx4.git
# changed in CMakeLists.txt BUILD_SHARED_LIBS to OFF. 
# In Windows had to use https://github.com/lakinwecker/netcdf-cxx4.git 
# which worked out of the box with cmake. 
find_library(NETCDF_CXX_LIB NAMES libnetcdf-cxx4.a netcdf-cxx4.lib)

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