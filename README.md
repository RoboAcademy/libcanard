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

`canard.c`를 application build에 추가, `libcanard` 디렉토리를 include path에 추가하면 준비가 된 것이다.

Libcanard와 함께 배포되는 여러 CAN 백엔드를 위한 다양한 드라이버 중에 하나를 사용하고 싶을 수도 있다. - `drivers/` 디렉토리를 확인해보자.

기본 옵션의 일부를 덮어쓰기를 하려면 에제로 assert macros의 정의 `CANARD_ENABLE_CUSTOM_BUILD_CONFIG` 를 0이 아닌 값으로 설정한다.(GCC나 Clang: `-DCANARD_ENABLE_CUSTOM_BUILD_CONFIG=1`)와 `canard_build_config.h` 파일 내부에서 구현부를 제공한다.

Make 예제:

```make
# library 추가하기
INCLUDE += libcanard
CSRC += libcanard/canard.c

# drivers 추가하기 (여러분의 것을 사용하지 않는 경우)
# 이 예제에서 Linux SocketCAN drivers를 사용
INCLUDE += libcanard/drivers/socketcan
CSRC += libcanard/drivers/socketcan/socketcan.c
```

CMake 예제, 먼저 dependencies를 설치

```bash
sudo apt-get update && sudo apt-get install gcc-multilib g++-multilib
cmake -S . -B build -DBUILD_TESTING=ON
cmake --build build
ctest .
```

library API에 대한 문서는 없다. 왜냐하면 코드 자체로 충분하기 때문이다.
기본을 익히고자 한다면 헤더 파일내에 코멘트에서 제공하는 설명을 확인해보자.
가장 중요한 것은 완전히 동작하는 [examples](examples) 디렉토리를 확인하는 것이다.

역직렬화와 직렬화 소스 코드의 생성을 위해서 https://github.com/dronecan/dronecan_dsdlc 를 참조하자.

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
