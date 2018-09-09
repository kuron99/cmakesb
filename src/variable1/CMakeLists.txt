# cmake if() handling is complicated
# normally variables should be referred as ${varname}, but if() allows specifying var name directly in the condition like if(varname)
# if(varname) is recommended because ${varname} will be empty when varname is not defined, and then the condition becomes false even with NOT
# Use of ${varname} brings a little bit unexpected result
cmake_minimum_required(VERSION 3.10)

set(var1 true)
set(vartrue true)
set(var2 off)
set(var3 0)
message("var1 : ${var1}")
message("var2 : ${var2}")
message("var3 : ${var3}")

if (var1 STREQUAL vartrue)   # you can specify naked variable name in if() condition
else()
    message(SEND_ERROR "assert failed")
endif()

if (undef1)
    message(SEND_ERROR "assert failed")
else()
endif()

if (var2)
    message(SEND_ERROR "assert failed")
else()
endif()

if (DEFINED var2)
else()
    message(SEND_ERROR "assert failed")
endif()

if (var1)
else()
    message(SEND_ERROR "assert failed")
endif()
if (NOT var1)
    message(SEND_ERROR "assert failed")
else()
endif()

if (${var1})
else()
    message(SEND_ERROR "assert failed")
endif()

if (NOT ${var1})
    message(SEND_ERROR "assert failed")
else()
endif()


if (${undef1})
    message(SEND_ERROR "assert failed")
else()
endif()

if (NOT ${undef1})
    message(SEND_ERROR "assert failed")
else()
endif()

if (NOT DEFINED ${undef1})
else()
    message(SEND_ERROR "assert failed")
endif()

# empty condition is always false
if()
    message(SEND_ERROR "assert failed")
else()
endif()
if(NOT)
    message(SEND_ERROR "assert failed")
else()
endif()

# but for DEFINED/NOT DEFINED are different
if(DEFINED)
    message(SEND_ERROR "assert failed")
else()
endif()
if(NOT DEFINED)
else()
    message(SEND_ERROR "assert failed")
endif()

# falsy values
set(falsy 0;faLse;oFf;nO;N;ignoRe;notfound;aaa-notfound)
foreach(f IN LISTS falsy)
    message("${f}")
    if(${f})
        message(SEND_ERROR "assert failed")
    else()
    endif()
endforeach()

set(empty "")
if(empty)
    message(SEND_ERROR "assert failed")
else()
endif()

# truethy values
set(truthy 1;2;tRue;oN;yes;y)
foreach(t IN LISTS truthy)
    message("${t}")
    if(${t})
    else()
        message(SEND_ERROR "assert failed")
    endif()
endforeach()

# char string is truthy
set(name "arbitrary_char_string")
if(name)
else()
    message(SEND_ERROR "assert failed")
endif()

# undefined name is assumed to be a string
if(undef3 STREQUAL "undef3")
else()
    message(SEND_ERROR "assert failed")
endif()

if(name STREQUAL "name")
    message(SEND_ERROR "assert failed")
else()
endif()

if(arbitrary_char_string STREQUAL ${name})
else()
    message(SEND_ERROR "assert failed")
endif()

if(name STREQUAL arbitrary_char_string)
else()
    message(SEND_ERROR "assert failed")
endif()

# accident happens when a variable has variable name
set(nameval "name")
if(nameval STREQUAL name)
    message(SEND_ERROR "assert failed")
else()
endif()

set(nameval2 "name2")
if(nameval2 STREQUAL name2)
else()
    message(SEND_ERROR "assert failed")
endif()

if(${nameval} STREQUAL name)
else()
    message(SEND_ERROR "assert failed")
endif()
if(${nameval} STREQUAL "name")
    message(SEND_ERROR "assert failed")
else()
endif()
