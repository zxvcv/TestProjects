docker run -ti -u $(id -u ${USER}):$(id -g ${USER}) -w=/home/conan/pckg -v ${HOME}/.conan:/.conan -v $(pwd):/home/conan/pckg zxvcv/gcc-arm-none-eabi:latest
mkdir bld && cd bld
conan install .. --profile=../profiles/default.cfg
cmake -DCMAKE_TOOLCHAIN_FILE=../toolchains/toolchain_stm32f3.cmake -DCMAKE_BUILD_TYPE=Debug ..
cmake --build .
