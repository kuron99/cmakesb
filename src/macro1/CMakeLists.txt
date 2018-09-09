# prefer function() to MACRO() since macro is simple character replacement
# also, make sure to use ${varname} format when you call macros and functions - f(p) passes string expression "p"
cmake_minimum_required(VERSION 3.10)

function(ASSERT_STREQ var val)
    if (var STREQUAL val)
    else ()
        message(SEND_ERROR "assert failed")
    endif ()
endfunction()

function(ASSERT_STRNEQ var val)
    if (NOT var STREQUAL val)
    else ()
        message(SEND_ERROR "assert failed")
    endif ()
endfunction()

function(ASSERT_TRUE var)
    if (var)
    else ()
        message(SEND_ERROR "assert failed")
    endif ()
endfunction()

function(ASSERT_FALSE var)
    if (var)
        message(SEND_ERROR "assert failed")
    endif ()
endfunction()

set(var1 true)
set(var2 off)
set(var3 0)
message("var1 : ${var1}")
message("var2 : ${var2}")
message("var3 : ${var3}")
ASSERT_TRUE(${var1})
ASSERT_FALSE(${var2})
ASSERT_STREQ(${var1} true)
ASSERT_STRNEQ(${var1} TRUE) # STREQUAL is case sensitive
ASSERT_STREQ(${var2} off)
ASSERT_STRNEQ(${var2} on)
ASSERT_STREQ(${var3} 0)
ASSERT_STRNEQ(${var3} 1)

# experiment passing variable name as parameter values
macro(MYMACRO var)
    message("var : ${var}")             # this shows "var : var1"
    if (NOT ${var} STREQUAL true)       # but here ${var} is evaluated as true through var1
        message(SEND_ERROR "assert failed")
    endif()
    if (NOT "${var}" STREQUAL "var1")   # to check simply ${var} in this macro/function, quote it and compare as string
        message(SEND_ERROR "assert failed")
    endif()
endmacro()

# same as above
function(MYFUNC var)
    message("var : ${var}")             # this shows "var : var1"
    if (NOT ${var} STREQUAL true)       # but here ${var} is evaluated as true through var1
        message(SEND_ERROR "assert failed")
    endif()
    if (NOT "${var}" STREQUAL "var1")   # to check simply ${var} in this macro/function, quote it and compare as string
        message(SEND_ERROR "assert failed")
    endif()
endfunction()

# following passes "var1" variable name as string - sometimes works, but not always good
MYMACRO(var1)
MYFUNC(var1)
