# Tech Stack & Environment: SafeOTA-ECU

본 문서는 **SafeOTA-ECU** 프로젝트 개발에 사용되는 기술 스택과 개발 환경을 상세히 기술합니다. 현대모비스 임베디드 SW 직무에서 요구하는 핵심 역량을 효과적으로 드러내기 위해 업계 표준 및 실무 친화적인 도구들을 선정했습니다.

## 1. Programming Languages

| Category | Language | Usage & Rationale |
| :--- | :--- | :--- |
| **Main** | **C (C99)** | - 임베디드 시스템 개발의 표준 언어.<br>- **MISRA-C 2012** 가이드라인을 일부 준수하여 코드의 안전성과 신뢰성 확보.<br>- 포인터, 메모리 관리, 비트 연산 등 Low-level 제어 역량 강조. |
| **Low-Level** | **Assembly (Thumb-2)** | - **Startup Code** 분석 및 수정 (Reset Handler, Stack pointer 초기화).<br>- Bootloader에서 Application으로의 **Jump Logic** 구현 시 사용. |
| **Tooling** | **Python 3.10+** | - PC 기반의 **UDS 진단 시뮬레이터** 및 펌웨어 전송 툴 제작.<br>- `pyserial` 라이브러리를 이용한 UART 통신 자동화 스크립트. |

## 2. Hardware & Target Architecture

| Component | Specification | Description |
| :--- | :--- | :--- |
| **MCU** | **STM32F103C8T6** | - **Core**: ARM Cortex-M3 (32-bit RISC).<br>- **Clock**: 72MHz.<br>- **Memory**: 64KB Flash / 20KB SRAM.<br>- 선정 이유: 전장 업계에서 가장 널리 쓰이는 ARM Cortex-M 아키텍처 이해도 증명 및 풍부한 레퍼런스. |
| **Peripherals** | **UART, Timer, GPIO** | - **UART**: PC와의 통신 및 FW 다운로드 채널.<br>- **Timer**: OS Tick 시뮬레이션 및 시간 측정.<br>- **Internal Flash**: 펌웨어 저장 및 섹션 분할 (Boot/App/Backup). |

## 3. Software Architecture & Standards

### 3.1. AUTOSAR (Automotive Open System Architecture) - *Lite Implementation*
실제 AUTOSAR 툴체인(Davinci 등)을 사용할 수 없는 환경이므로, **AUTOSAR의 계층형 아키텍처 철학**을 적용하여 수동으로 구현합니다.
*   **Layered Architecture**: `ASW` - `RTE` - `BSW` - `MCAL` 계층 분리.
*   **MCAL (Microcontroller Abstraction Layer)**: 하드웨어 종속적인 코드를 추상화하여 상위 계층에 표준 API 제공.
*   **BSW (Basic Software)**: 하드웨어 독립적인 서비스 제공 (메모리 인터페이스, 통신 매니저 등).

### 3.2. Communication Protocol
*   **ISO 14229 (UDS - Unified Diagnostic Services)**: 차량 진단 표준 프로토콜.
    *   **Service $10**: Diagnostic Session Control (부기로더 모드 진입).
    *   **Service $27**: Security Access (Seed & Key 인증).
    *   **Service $34, $36, $37**: Request Download, Transfer Data, Transfer Exit (펌웨어 바이너리 전송).
    *   **Service $11**: ECU Reset.

## 4. Development Tools (IDE & Toolchain)

*   **IDE**: **STM32CubeIDE 1.12+**
    *   Eclipse 기반의 통합 개발 환경.
    *   디버깅(Breakpoints, Register View, Memory View) 기능 활용.
*   **Compiler**: **GCC ARM Embedded Toolchain**
    *   오픈소스 크로스 컴파일러.
    *   Linker Script (`.ld`) 수정을 통한 메모리 섹션 배치 (Bootloader/App 분리).
*   **Debugger**: **ST-Link/V2**
    *   SWD (Serial Wire Debug) 인터페이스를 통한 실시간 디버깅 및 바이너리 다운로드.
*   **Version Control**: **Git & GitHub**
    *   이슈 관리 및 커밋 히스토리 관리.
    *   전략적인 Branch 운용 (Feature/Develop/Master).

## 5. Directory Structure Strategy
프로젝트는 AUTOSAR 계층 구조를 반영하여 다음과 같이 구성될 예정입니다.

```text
Project_Root/
├── Core/
│   ├── Inc/
│   └── Src/               # Main loop, Interrupt Handlers
├── App/                   # ASW (Application Software)
├── RTE/                   # RTE (Runtime Environment) Interfaces
├── BSW/                   # Basic Software
│   ├── Dcm/               # Diagnostic Communication Manager (UDS)
│   ├── MemIf/             # Memory Abstraction Interface
│   └── Comm/              # Communication Manager
├── MCAL/                  # Microcontroller Abstraction Layer
│   ├── Mcu/
│   ├── Port/
│   ├── Dio/
│   ├── Uart/
│   └── Fls/               # Flash Driver
└── Tools/                 # Python Scripts (UDS Tester)
```
