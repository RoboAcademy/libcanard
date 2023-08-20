# Libcanard

C로 DroneCAN protocol statck 최소 구현(임베디드 장치같이 리소스가 작은 장치에 적합)

**[DroneCAN Forum](https://dronecan.org/discord)** 에서 도움얻기.

## Usage
* Git을 사용안하는 경우 복사해서 원하는 프로젝트에 넣기
* 추천하는 방식은 submodule로 project에 추가하는 방식
아래와 같이:

```bash
git submodule add https://github.com/DroneCAN/libcanard
```

전체 library는 3개 파일로 구성 :

- `canard.c` - 유일한 translation unit; 별도의 static library로 빌드나 컴파일에 추가;
- `canard.h` - API header; 여러분의 project 내에서 이것을  include;
- `canard_internals.h` - library의 내부 정의;
`canard.c`와 같은 디렉토리내에 이 파일을 위치시기키

Add `canard.c` to your application build, add `libcanard` directory to the include paths,
and you're ready to roll.

Also you may want to use one of the available drivers for various CAN backends
that are distributed with Libcanard - check out the `drivers/` directory to find out more.

If you wish to override some of the default options, e.g., assert macros' definition,
define the macro `CANARD_ENABLE_CUSTOM_BUILD_CONFIG` as a non-zero value
(e.g. for GCC or Clang: `-DCANARD_ENABLE_CUSTOM_BUILD_CONFIG=1`)
and provide your implementation in a file named `canard_build_config.h`.

Example for Make:

```make
# Adding the library.
INCLUDE += libcanard
CSRC += libcanard/canard.c

# Adding drivers, unless you want to use your own.
# In this example we're using Linux SocketCAN drivers.
INCLUDE += libcanard/drivers/socketcan
CSRC += libcanard/drivers/socketcan/socketcan.c
```

Example for CMake, first installing dependencies. 

```bash
sudo apt-get update && sudo apt-get install gcc-multilib g++-multilib
cmake -S . -B build -DBUILD_TESTING=ON
cmake --build build
ctest .
```

There is no dedicated documentation for the library API, because it is simple enough to be self-documenting.
Please check out the explanations provided in the comments in the header file to learn the basics.
Most importantly, check out the [examples](examples) directory for fully worked examples

For generation of de-serialisation and serialisation source code, please refer https://github.com/dronecan/dronecan_dsdlc .

## C++ Interface

canard/ directory 내부에 C++ interface가 있다. C++ API로 완전히 동작하는 예제는 [examples/ESCNode_C++](examples/ESCNode_C++) 를 참고하자.

## Library 개발

이 섹션은 library 개발자와 contributors 만을 위한 부분.

이 library 설계 문서는 [DESIGN.md](DESIGN.md) 내에서 찾을 수 있다.

### 빌드하기 와 Tests 실행하기

```bash
mkdir build && cd build
cmake ../libcanard/tests    # 필요하면 경로를 조정하기
make
./run_tests
```
