project( folly )

cmake_minimum_required( VERSION 2.8 )
include( CheckCXXCompilerFlag )
check_cxx_compiler_flag( "-std=c++11"   COMPILER_SUPPORTS_CXX11 )
check_cxx_compiler_flag( "-std=c++0x"   COMPILER_SUPPORTS_CXX0X )
if( COMPILER_SUPPORTS_CXX11 )
    if( CMAKE_COMPILER_IS_GNUCXX )
        set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11" )
    else()
        set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14" )
    endif()
elseif( COMPILER_SUPPORTS_CXX0X)
    set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x" )
else()
    # MSVC, On by default (if available)
endif()
include( cmake/Modules/FindGlog.cmake )
find_package( Boost REQUIRED  thread Regex Filesystem system)
include_directories( ${Boost_INCLUDE_DIR} )
include_directories( ${CMAKE_SOURCE_DIR} ) 
link_directories( /usr/local/Cellar/double-conversion/1.1.5/lib  )
set(SOURCE
   ${SOURCE}
   folly/CheckSum.cpp 
folly/futures/Barrier.cpp
folly/futures/Future.cpp
folly/Demangle.cpp 
folly/io/IOBuf.cpp  
 folly/detail/MemoryIdler.cpp
 folly/detail/MallocImpl.cpp 
 folly/Conv.cpp
 folly/portability/SysMman.cpp
 folly/detail/ThreadLocalDetail.cpp
 folly/SharedMutex.cpp
 folly/ScopeGuard.cpp
 folly/detail/CacheLocality.cpp
 folly/detail/StaticSingletonManager.cpp
 folly/detail/Futex.cpp
 folly/Format.cpp
 folly/portability/BitsFunctexcept.cpp
 folly/portability/SysUio.cpp
 folly/experimental/JemallocNodumpAllocator.cpp
 folly/FormatTables.cpp
 folly/Random.cpp
 folly/Uri.cpp
 folly/Subprocess.cpp
 folly/File.cpp
 folly/FileUtil.cpp
 folly/SpookyHashV2.cpp
 folly/io/IOBufQueue.cpp
 folly/Assume.cpp
  folly/experimental/TestUtil.cpp
#  folly/fibers/Baton.cpp
  folly/MallctlHelper.cpp
# folly/json.cpp
  folly/Unicode.cpp
   folly/futures/ThreadWheelTimekeeper.cpp
#   folly/io/async/EventBase.cpp
)


set(IOSOURCE
   ${IOSOURCE}
   folly/io/Compression.cpp 

   )
find_package(GTest REQUIRED)
find_package(gflags REQUIRED)
include_directories(${GTEST_INCLUDE_DIRS})

add_library( LibFolly STATIC  ${SOURCE}  )
add_library( LibFollyio STATIC ${IOSOURCE} )
add_executable( ForeachTest folly/test/ForeachTest.cpp  )
add_executable( AtomicHashArrayTest folly/test/AtomicHashArrayTest.cpp folly/test/DeterministicSchedule.cpp  )
add_executable( FormatTest folly/test/FormatTest.cpp   )
add_executable( ArrayTest folly/test/ArrayTest.cpp   )
add_executable( UriTest folly/test/UriTest.cpp   )
add_executable( SubprocessTest folly/test/SubprocessTest.cpp   )
add_executable( BatonTest folly/test/BatonTest.cpp  folly/test/DeterministicSchedule.cpp  )
add_executable( CallOnceTest folly/test/CallOnceTest.cpp   )
add_executable( CpuIdTest folly/test/CpuIdTest.cpp   )
add_executable( JsonTest folly/test/JsonTest.cpp   )
add_executable( TimeseriesTest folly/test/TimeseriesTest.cpp   )

add_executable( CollectTest folly/futures/test/CollectTest.cpp   )

add_executable( ApplyTupleTest folly/test/ApplyTupleTest.cpp   )
add_executable( ArenaSmartPtrTest  folly/test/ArenaSmartPtrTest.cpp )

add_executable( CompressionTest  folly/io/test/CompressionTest.cpp )

target_link_libraries( ForeachTest  ${GTEST_BOTH_LIBRARIES} LibFolly     ${Boost_LIBRARIES}  ${GLOG_LIBRARY} )
target_link_libraries( AtomicHashArrayTest   ${GTEST_BOTH_LIBRARIES} LibFolly     ${Boost_LIBRARIES}  ${GLOG_LIBRARY}  double-conversion )

target_link_libraries( FormatTest   ${GTEST_BOTH_LIBRARIES} LibFolly     ${Boost_LIBRARIES}  ${GLOG_LIBRARY}  double-conversion )
target_link_libraries( ArrayTest   ${GTEST_BOTH_LIBRARIES} LibFolly     ${Boost_LIBRARIES}  ${GLOG_LIBRARY}  double-conversion )
target_link_libraries( UriTest   ${GTEST_BOTH_LIBRARIES} LibFolly     ${Boost_LIBRARIES}  ${GLOG_LIBRARY}  double-conversion )
target_link_libraries( SubprocessTest   ${GTEST_BOTH_LIBRARIES} LibFolly     ${Boost_LIBRARIES}  ${GLOG_LIBRARY}  double-conversion )
target_link_libraries( BatonTest   ${GTEST_BOTH_LIBRARIES} LibFolly     ${Boost_LIBRARIES}  ${GLOG_LIBRARY}  double-conversion )
target_link_libraries( CallOnceTest    ${GTEST_BOTH_LIBRARIES} LibFolly     ${Boost_LIBRARIES}  ${GLOG_LIBRARY}  double-conversion gflags)
target_link_libraries( CpuIdTest    ${GTEST_BOTH_LIBRARIES} LibFolly     ${Boost_LIBRARIES}  ${GLOG_LIBRARY}  double-conversion gflags)
target_link_libraries( JsonTest    ${GTEST_BOTH_LIBRARIES} LibFolly     ${Boost_LIBRARIES}  ${GLOG_LIBRARY}  double-conversion gflags)
target_link_libraries( TimeseriesTest    ${GTEST_BOTH_LIBRARIES} LibFolly     ${Boost_LIBRARIES}  ${GLOG_LIBRARY}  double-conversion gflags)
target_link_libraries( CollectTest    ${GTEST_BOTH_LIBRARIES} LibFolly     ${Boost_LIBRARIES}  ${GLOG_LIBRARY}  double-conversion gflags)
target_link_libraries( ArenaSmartPtrTest    ${GTEST_BOTH_LIBRARIES} LibFolly     ${Boost_LIBRARIES}  ${GLOG_LIBRARY}  double-conversion gflags)
target_link_libraries( CompressionTest     ${GTEST_BOTH_LIBRARIES} LibFolly   LibFollyio   ${Boost_LIBRARIES}  ${GLOG_LIBRARY}  double-conversion gflags)
