# 📂 Cross-Platform Inline Assembly Library Project

> **C언어 인라인 어셈블리(Inline Assembly)를 활용한 연산 로직 구현 및 OS별(Linux/Windows) 라이브러리 교차 컴파일 실습**

## 🛠️ Tech Stack & Environment
* **Language**: C, Assembly (AT&T Syntax)
* **OS**: WSL (Ubuntu 20.04), Windows 10/11
* **Compiler**: GCC (GNU Compiler Collection), MinGW-w64
* **Build Targets**: Static Library (`.a`), Shared Object (`.so`), Dynamic Link Library (`.dll`)
---

## 0. 실행 환경 준비 (Prerequisites)
프로젝트 빌드에 필요한 GCC 컴파일러와 필수 라이브러리를 설치하는 과정입니다. WSL(Ubuntu) 환경에서 진행했습니다.

* **Command**:
```bash
sudo apt update
sudo apt install build-essential gcc-multilib
```
* **Details**:
    * `build-essential`: gcc, make 등 개발에 필요한 기본 도구 모음
    * `gcc-multilib`: 32비트 및 64비트 아키텍처를 모두 지원하기 위한 라이브러리


---

## 1. 핵심 코드 구현 (Core Implementation)
C언어 변수가 아닌 CPU 레지스터(`%eax`)를 직접 제어하여 덧셈 연산을 수행하는 함수를 작성하고, 이를 헤더와 소스로 분리하여 모듈화했습니다.

### 1-1. 헤더 파일 선언 (mylib.h)
함수의 원형을 선언하여 외부에서 호출 가능하도록 인터페이스를 정의했습니다.

<img width="287" height="273" alt="596d7830-36cc-479f-b29a-cd0e0eeb9c7a" src="https://github.com/user-attachments/assets/61a3a52b-368a-4297-9eaa-95eb22b5b8e3" />


### 1-2. 인라인 어셈블리 (`mylib.c`)
GCC 기반의 **AT&T 문법**을 사용하여 하드웨어 레벨의 연산 과정을 C 코드 내에 통합했습니다.
![Assembly Code]<img width="497" height="436" alt="KakaoTalk_20251208_203955630_07" src="https://github.com/user-attachments/assets/cf3d32c4-bf64-4f97-8e2b-cb7f5001cbbb" />


### 1-3. 모듈화된 메인 함수 (`main.c`)
라이브러리 헤더(`mylib.h`)를 포함하여, 어셈블리 로직을 외부 함수처럼 호출하는 구조입니다.
![Main Code]<img width="775" height="410" alt="KakaoTalk_20251208_203955630_08" src="https://github.com/user-attachments/assets/668f48c7-6131-43d3-b058-baf89f45ef57" />



---

## 2. 기본 로직 검증 (Basic Verification)
라이브러리 빌드 전, 단일 파일 상태에서 컴파일하여 인라인 어셈블리 로직이 정상 작동하는지 우선 확인했습니다.

![KakaoTalk_20251208_203955630_06](https://github.com/user-attachments/assets/0e7caecf-b535-4b52-a72b-7663a7de5ff3)

> **Test:** `gcc main.c -o main` 명령어로 컴파일 후 실행하여 결과값 `30`이 정상적으로 출력됨을 확인했습니다.

---

## 3. Linux (WSL) 라이브러리 빌드
리눅스 환경에서 **정적(Static)** 및 **동적(Dynamic)** 라이브러리를 생성했습니다.

* **Static Link (`.a`)**: `ar rcs` 명령어로 아카이브 생성.
* **Dynamic Link (`.so`)**: `gcc -shared` 및 `-fPIC` 옵션 사용.
* **Execution**: `LD_LIBRARY_PATH`를 지정하여 런타임에 라이브러리를 동적으로 로드.

![Linux Build Process]<img width="1009" height="287" alt="image" src="https://github.com/user-attachments/assets/ca3af627-cbdd-41b2-a8fe-037b0c656ff8" />

> **Success:** WSL 터미널에서 라이브러리 생성부터 링킹, 실행까지 완벽하게 수행되는 과정입니다.

---

## 4. 시행착오 및 문제 해결 (Troubleshooting)
개발 과정에서 발생한 주요 오류와 해결 과정입니다.

### 4-1. 링킹(Linking) 오류
라이브러리 파일이 존재함에도 불구하고 컴파일러가 함수를 찾지 못하는 문제가 발생했습니다.
![Linking Error]<img width="1012" height="495" alt="image" src="https://github.com/user-attachments/assets/89878018-533d-48d1-ba7e-8d3467a91f74" />

> **Cause & Fix:** 라이브러리 경로(`-L.`)와 라이브러리명(`-lmylib`) 옵션을 명시하지 않아 발생한 문제로, 올바른 플래그를 추가하여 해결했습니다.

### 4-2. 크로스 플랫폼 호환성 이슈
WSL에서 `.exe` 확장자로 컴파일했으나 실행되지 않는 현상을 겪었습니다.
![Exe Error]<img width="923" height="136" alt="image" src="https://github.com/user-attachments/assets/b0bb54ae-d8fe-4e6c-9fd0-debb2415c3f5" />

> **Insight:** 리눅스용 ELF 포맷은 윈도우에서 실행될 수 없으며, 단순 확장자 변경이 아닌 **MinGW** 같은 크로스 컴파일러가 필요함을 깨달았습니다.

### 4-3. 문제 해결 과정 로그
다양한 옵션을 시도하며 빌드 환경을 잡아가는 전체 과정입니다.
![History Log]<img width="736" height="604" alt="image" src="https://github.com/user-attachments/assets/1eca87c7-0e44-4d78-a659-a3eb33ffecff" />


---

## 5. Windows (MinGW) DLL 제작 성공
Windows CMD 환경으로 이동하여 **MinGW-w64**를 이용해 실제 윈도우에서 작동하는 **DLL**과 실행 파일을 빌드했습니다.

* **Encoding Fix**: `chcp 65001`을 사용하여 터미널 한글 깨짐 문제 해결.
* **DLL Build**: `gcc -shared` 옵션으로 `mylib.dll` 생성.
![Windows Success]![KakaoTalk_20251208_203955630_01](https://github.com/user-attachments/assets/857e81f8-9406-41a0-a45d-97fe0bcc34ec)

> **Success:** Windows 환경에서 정상적으로 DLL이 로드되어 결과값 `30`을 출력하는 모습입니다.

---

## 6. 최종 결과물 (Build Artifacts)
하나의 소스 코드로 다양한 플랫폼에 맞는 바이너리를 생성해냈습니다.
![Final Files]<img width="642" height="466" alt="KakaoTalk_20251208_203955630" src="https://github.com/user-attachments/assets/8d078f36-272a-454e-8c47-4b8f6c19ee90" />

> **Structure:**
> * **Linux**: `libmylib.a`, `libmylib.so`, `main`
> * **Windows**: `mylib.dll`, `main.exe`

---

### 📝 결론 (Conclusion)
이 프로젝트를 통해 **"Write Once, Compile Anywhere"**의 개념을 직접 실습했습니다. 동일한 어셈블리 로직이라도 운영체제에 따라 **컴파일 옵션, 라이브러리 포맷, 링킹 방식**이 다르다는 것을 시스템 레벨에서 깊이 있게 이해하는 계기가 되었습니다.
