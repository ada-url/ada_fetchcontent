cmake_minimum_required(VERSION 3.15)

project(FetchContentDemo VERSION 0.1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include(FetchContent)

FetchContent_Declare(
  ada
  GIT_REPOSITORY git@github.com:ada-url/ada.git
  GIT_SHALLOW TRUE
  GIT_TAG tags/v2.7.2
  )

FetchContent_MakeAvailable(ada)



file(WRITE main.cpp "
 #include \"ada.h\"
 #include <iostream>

 int main(int, char *[]) {
   auto url = ada::parse<ada::url>(\"https://www.google.com\");
   if (!url) {
     std::cout << \"failure\" << std::endl;
     return EXIT_FAILURE;
   }
   url->set_protocol(\"http\");
   std::cout << url->get_protocol() << std::endl;
   std::cout << url->get_host() << std::endl;
   return EXIT_SUCCESS;
 }")



add_executable(repro main.cpp)
target_link_libraries(repro PUBLIC ada)