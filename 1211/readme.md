📂 Cross-Platform Inline Assembly Library Project
C언어 인라인 어셈블리(Inline Assembly)를 활용한 연산 로직 구현 및 OS별(Linux/Windows) 라이브러리 교차 컴파일 실습

🛠️ Tech Stack & Environment
Language: C, Assembly (AT&T Syntax)

OS: WSL (Ubuntu 20.04), Windows 10/11

Compiler: GCC (GNU Compiler Collection), MinGW-w64

Build Targets: Static Library (.a), Shared Object (.so), Dynamic Link Library (.dll)

1. 핵심 코드 구현 (Core Implementation)
C언어 변수가 아닌 CPU 레지스터(%eax)를 직접 제어하여 덧셈 연산을 수행하는 함수를 작성했습니다. GCC 기반의 AT&T 문법을 사용하여 하드웨어 레벨의 연산 과정을 C 코드 내에 통합했습니다.

Code: __asm__ volatile 키워드를 사용하여 어셈블리 명령어를 C 코드 내부에 직접 삽입한 모습입니다.

2. 기본 로직 검증 (Basic Verification)
라이브러리로 모듈화하기 전, 단일 파일(main.c) 상태에서 컴파일하여 인라인 어셈블리 로직이 정상 작동하는지 확인했습니다.

Test: gcc main.c -o main 명령어로 컴파일 후 실행하여 결과값 30이 정상적으로 출력됨을 확인했습니다.

3. Linux (WSL) 라이브러리 빌드
코드를 재사용 가능하도록 mylib.c(구현부)와 main.c(실행부)로 분리하고, 리눅스 환경에서 정적(Static) 및 동적(Dynamic) 라이브러리를 생성했습니다.

Static Link (.a): ar rcs 명령어로 아카이브 생성.

Dynamic Link (.so): gcc -shared 및 -fPIC 옵션 사용.

Execution: LD_LIBRARY_PATH를 지정하여 런타임에 라이브러리를 동적으로 로드.

Process: WSL 터미널에서 라이브러리 생성부터 링킹, 실행까지 완벽하게 수행되는 과정입니다.

4. 시행착오 및 문제 해결 (Troubleshooting)
리눅스(WSL) 환경에서 컴파일한 실행 파일을 윈도우 스타일(.exe)로 실행하려다 발생한 크로스 플랫폼 호환성 이슈입니다.

Issue: WSL에서 .exe 확장자를 붙여 컴파일했으나, 실제 포맷은 리눅스용 ELF 파일이므로 윈도우 CMD 창이나 WSL 내부의 윈도우 방식 호출로는 실행되지 않았습니다.

Insight: 단순한 확장자 변경이 아닌, 대상 OS에 맞는 **바이너리 포맷(PE vs ELF)**으로 컴파일해야 함을 확인했습니다.

Solution: Windows 네이티브 실행을 위해 MinGW 컴파일러 도입을 결정했습니다.

Error Log: "무늬만 exe"인 파일을 실행하려다 실패한 모습. 이 과정을 통해 운영체제 간 실행 파일 구조의 차이를 명확히 이해했습니다.

5. Windows (MinGW) DLL 제작 성공
Windows CMD 환경으로 이동하여 MinGW-w64를 이용해 실제 윈도우에서 작동하는 DLL과 실행 파일을 빌드했습니다.

Encoding Fix: chcp 65001을 사용하여 터미널 한글 깨짐 문제 해결.

DLL Build: gcc -shared 옵션으로 mylib.dll 생성.

Execution: 별도의 환경 변수 설정 없이, 같은 폴더 내의 DLL을 자동으로 인식하여 실행 성공.

Success: Windows 환경에서 정상적으로 DLL이 로드되어 결과값 30을 출력하는 모습입니다.

6. 최종 결과물 (Build Artifacts)
하나의 소스 코드(mylib.c)를 사용하여 다양한 플랫폼에 맞는 바이너리를 생성해냈습니다.

Linux Artifacts: libmylib.a, libmylib.so, main

Windows Artifacts: mylib.dll, main.exe

Structure: 프로젝트 완료 후 생성된 전체 파일 목록입니다. 소스 코드는 하나지만 결과물은 OS별로 다양하게 생성되었습니다.

📝 결론 (Conclusion)
이 프로젝트를 통해 **"Write Once, Compile Anywhere"**의 개념을 직접 실습했습니다. 동일한 어셈블리 로직이라도 운영체제에 따라 컴파일 옵션, 라이브러리 포맷, 링킹 방식이 다르다는 것을 시스템 레벨에서 깊이 있게 이해하는 계기가 되었습니다.
