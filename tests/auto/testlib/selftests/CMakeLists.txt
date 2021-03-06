# The whole file is written manually.
# special case skip regeneration

include(../../../../src/testlib/selfcover.cmake)

# ------------- Test runner -------------

qt_add_test(tst_selftests
    OUTPUT_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/"
    EXCEPTIONS
    SOURCES
        ../../../shared/emulationdetector.h
        tst_selftests.cpp
        catch.cpp
    INCLUDE_DIRECTORIES
        ../../../shared
    PUBLIC_LIBRARIES
        Qt::TestPrivate
    # special case begin
    # These lines need to be commented out as they need to be enabled
    # COMPILE_OPTIONS
    #   conditionally
    #   --cs-exclude-file-abs-wildcard=${QT_SOURCE_TREE}/*
    #   --cs-include-file-abs-wildcard=*/src/testlib/*
    #   --cs-mcc
    #   --cs-mcdc
    # LINK_OPTIONS
    #   "$$COVERAGE_OPTIONS"
    # special case end
)

file(GLOB resource_glob_0 RELATIVE "${CMAKE_CURRENT_SOURCE_DIR}" "expected_*")
foreach(file IN LISTS resource_glob_0)
    set_source_files_properties("${CMAKE_CURRENT_SOURCE_DIR}/${file}" PROPERTIES QT_RESOURCE_ALIAS "${file}")
endforeach()

# Resources:
set(qmake_expected_files_resource_files
    ${resource_glob_0}
)

qt_add_resource(tst_selftests "qmake_expected_files"
    PREFIX
        "/"
    BASE
        "${CMAKE_CURRENT_SOURCE_DIR}" # special case
    FILES
        ${qmake_expected_files_resource_files}
)


## Scopes:
#####################################################################

#### Keys ignored in scope 5:.:.:test.pro:NOT ANDROID AND NOT WINRT AND for(file,SUBPROGRAMS):
# TEST_HELPER_INSTALLS = "../$${file}/$${file}"

#### Keys ignored in scope 7:.:..:../selftests.pri:TARGET Qt::Gui:
# SUBPROGRAMS = "keyboard" "mouse"

qt_apply_testlib_coverage_options(tst_selftests)

# ------------- Sub tests -------------

# special case begin
if(NOT WINRT)
    set(subprograms
        assert
        badxml
        benchlibcallgrind
        benchlibcounting
        benchlibeventcounter
        benchliboptions
        benchlibtickcounter
        benchlibwalltime
        blacklisted
        cmptest
        commandlinedata
        counting
        crashes
        datatable
        datetime
        deleteLater
        deleteLater_noApp
        differentexec
        exceptionthrow
        expectfail
        failcleanup
        faildatatype
        failfetchtype
        failinit
        failinitdata
        fetchbogus
        findtestdata
        float
        globaldata
        longstring
        maxwarnings
        multiexec
        pass
        pairdiagnostics
        printdatatags
        printdatatagswithglobaltags
        qexecstringlist
        silent
        signaldumper
        singleskip
        skip
        skipcleanup
        skipinit
        skipinitdata
        sleep
        strcmp
        subtest
        testlib
        tuplediagnostics
        verbose1
        verbose2
        verifyexceptionthrown
        warnings
        watchdog
        xunit
    )

    if(TARGET Qt::Gui)
        list(APPEND subprograms
            keyboard
            mouse
        )
    endif()
endif()

foreach(subprogram IN LISTS subprograms)
    add_subdirectory(${subprogram})
endforeach()

list(JOIN subprograms " " subprograms)
qt_extend_target(tst_selftests
    DEFINES
        SUBPROGRAMS=${subprograms}
)
# special case end

#### Keys ignored in scope 1:.:.:test.pro:<TRUE>:
# CONFIG = "testcase"
# expected_files.base = "$$PWD/.."
# expected_files.files = "$$files$$PWD/../expected_*"

## Scopes:
#####################################################################

#### Keys ignored in scope 5:.:.:test.pro:NOT ANDROID AND NOT WINRT AND for(file,SUBPROGRAMS):
# TEST_HELPER_INSTALLS = "../$${file}/$${file}"
# special case end
