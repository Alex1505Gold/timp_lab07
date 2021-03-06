<h1>Отчет по лобораторной 07</h1>
</br>gmail почта - sgolenkov2002@gmail.com </br>
telegram - @Xacker_ducker

<h2>Ход выполнения лабораторной работы:</h2>

Были выплнены следующие команды
```shell
export GITHUB_USERNAME=Alex1505Gold
alias gsed=sed
cd ${GITHUB_USERNAME}/workspace
pushd .
source scripts/activate
git clone https://github.com/${GITHUB_USERNAME}/lab06 projects/lab07
cd projects/lab07
git remote remove origin
git remote add origin https://github.com/${GITHUB_USERNAME}/lab07
mkdir -p cmake
wget https://raw.githubusercontent.com/cpp-pm/gate/master/cmake/HunterGate.cmake -O cmake/HunterGate.cmake
```
sed команды были изменены в соответствии с синтаксисом
```shell
gsed -i '/cmake_minimum_required(VERSION 3.4)/a\
\
include("cmake/HunterGate.cmake")\
HunterGate(\
    URL "https://github.com/cpp-pm/hunter/archive/v0.23.251.tar.gz"\
    SHA1 "5659b15dc0884d4b03dbd95710e6a1fa0fc3258d"\
)\
' CMakeLists.txt
git rm -rf third-party/gtest
gsed -i '/set(PRINT_VERSION_STRING "v\${PRINT_VERSION}")/a\
\
hunter_add_package(GTest)\
find_package(GTest CONFIG REQUIRED)\
' CMakeLists.txt
gsed -i 's|add_subdirectory(third-party/gtest)||' CMakeLists.txt
gsed -i 's/gtest_main/GTest::main/' CMakeLists.txt
cmake -H. -B_builds -DBUILD_TESTS=ON
cmake --build _builds
cmake --build _builds --target test
ls -la $HOME/.hunter
git clone https://github.com/cpp-pm/hunter $HOME/projects/hunter
export HUNTER_ROOT=$HOME/projects/hunter
rm -rf _builds
cmake -H. -B_builds -DBUILD_TESTS=ON
cmake --build _builds
cmake --build _builds --target test
cat $HUNTER_ROOT/cmake/configs/default.cmake | grep GTest
cat $HUNTER_ROOT/cmake/projects/GTest/hunter.cmake
mkdir cmake/Hunter
cat > cmake/Hunter/config.cmake <<EOF
hunter_config(GTest VERSION 1.7.0-hunter-9)
EOF
mkdir demo
cat > demo/main.cpp <<EOF
#include <print.hpp>

#include <cstdlib>

int main(int argc, char* argv[])
{
  const char* log_path = std::getenv("LOG_PATH");
  if (log_path == nullptr)
  {
    std::cerr << "undefined environment variable: LOG_PATH" << std::endl;
    return 1;
  }
  std::string text;
  while (std::cin >> text)
  {
    std::ofstream out{log_path, std::ios_base::app};
    print(text, out);
    out << std::endl;
  }
}
EOF

gsed -i '/endif()/a\
\
add_executable(demo ${CMAKE_CURRENT_SOURCE_DIR}/demo/main.cpp)\
target_link_libraries(demo print)\
install(TARGETS demo RUNTIME DESTINATION bin)\
' CMakeLists.txt
mkdir tools
git submodule add https://github.com/ruslo/polly tools/polly
tools/polly/bin/polly.py --test
tools/polly/bin/polly.py --install
tools/polly/bin/polly.py --toolchain clang-cxx14
```
Результаты выполнения команд
![screen01](./screens/screen01.png)</br>
![screen02](./screens/screen02.png)</br>
![screen06](./screens/screen03.png)</br>
![screen01](./screens/screen04.png)</br>
![screen02](./screens/screen05.png)</br>
![screen06](./screens/screen06.png)</br>
![screen01](./screens/screen07.png)</br>
![screen02](./screens/screen08.png)</br>
