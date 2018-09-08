# basic file operations - try create file, directory and delete them

cmake_minimum_required(VERSION 3.10)

file(MAKE_DIRECTORY dir1 dir2 dir1/dir11)
file(WRITE dir1/main.cpp "int main() { return 0 }")
file(WRITE dir2/main.cpp "int main() { return 0 }")
file(WRITE dir1/dir11/main.cpp "int main() { return 0 }")

file(GLOB DIRS LIST_DIRECTORIES true "dir*")
message("Directories found : ${DIRS}")
foreach(dir IN LISTS DIRS)
    message("$dir: ${dir}")
    if(IS_DIRECTORY ${dir})
        get_filename_component(DIRNAME ${dir} NAME_WE)
        message("dirname : ${DIRNAME}")
        file(REMOVE_RECURSE ${dir})
    endif()
endforeach()


