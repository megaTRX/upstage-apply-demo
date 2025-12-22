# Portfolio Project PRD: SafeOTA-ECU (현대모비스 MCU 임베디드 SW 지원용)

## 1. 프로젝트 개요
*   **프로젝트명**: SafeOTA-ECU (Reliable OTA Update System based on AUTOSAR-like Layered Architecture)
*   **목표**: 현대모비스 'MCU 임베디드 SW' 직무의 핵심 요구사항인 **AUTOSAR 기반 BSW 개발**, **MCU 제어**, **OTA 업데이트**, **기능 안전(무결성 검증)** 역량을 증명하기 위한 포트폴리오 프로젝트.
*   **개발 기간**: 4주 (예상)

## 2. 타겟 직무 분석 및 매핑
| 채용 공고 요구사항 | 프로젝트 구현 요소 | 기대 효과 |
| :--- | :--- | :--- |
| **AUTOSAR 기반 BSW 개발** | 계층형 아키텍처 (App-RTE-BSW-MCAL) 설계 및 구현 | 표준 스택 이해도 및 모듈화 설계 역량 어필 |
| **MCU 기반 MCAL/CDD 개발** | STM32 레지스터 직접 제어를 통한 UART/Flash/GPIO 드라이버 구현 | 하드웨어 제어 능력 및 C언어 심화 역량 증명 |
| **OTA 적용 다운로드 SW** | 듀얼 뱅크(Dual Bank) 또는 A/B 파티션 기반 펌웨어 업데이트 구현 | Bootloader 및 메모리 맵 구조 설계 능력 증명 |
| **차량 보안/기능 안전** | 펌웨어 무결성 검증 (CRC-32), 롤백(Roll-back) 메커니즘 | 임베디드 시스템의 신뢰성 및 안전성 고려 경험 강조 |

## 3. 상세 기능 명세

### 3.1. Layered Architecture (소프트웨어 아키텍처)
*   **ASW (Application Software)**:
    *   단순 LED 점멸 및 상태 리포팅 로직 (OTA 동작 확인용).
    *   RTE API를 통해서만 하위 계층에 접근하도록 격리.
*   **RTE (Runtime Environment) Lite**:
    *   ASW와 BSW 간의 인터페이스 정의.
    *   가벼운 형태의 스케줄러 포함 (Cyclic Task 관리).
*   **BSW (Basic Software) - Service Layer**:
    *   **DCM (Diagnostic Communication Manager) Lite**: UDS(iso14229) 일부 서비스($10, $11, $27, $34, $36, $37)를 모사하여 펌웨어 다운로드 프로토콜 구현.
    *   **MEMIF (Memory Abstraction Interface)**: Flash 메모리 쓰기/지우기 추상화.
*   **MCAL (Microcontroller Abstraction Layer)**:
    *   **GPT**: 타이머 제어.
    *   **UART**: PC(진단기 시뮬레이터)와의 통신.
    *   **Fls**: 내장 Flash 메모리 제어 드라이버 (Program/Erase).

### 3.2. 핵심 기능: OTA (Over-The-Air) Simulation
*   **Bootloader**:
    *   시스템 부팅 시 특정 GPIO 또는 플래그 확인으로 진입.
    *   APP 영역의 유효성(CRC, Magic Number) 검사 후 점프.
    *   업데이트 실패 시 백업 파티션으로 롤백 기능(선택 사항).
*   **Firmware Update Sequence**:
    1.  Diagnostic Session Control ($10) 진입.
    2.  Security Access ($27) - Seed & Key 알고리즘 적용 (간소화).
    3.  Routine Control ($31) - Flash Erase.
    4.  Request Download ($34) & Transfer Data ($36) - 펌웨어 바이너리 전송.
    5.  Request Transfer Exit ($37).
    6.  ECU Reset ($11).

## 4. 기술 스택 (Tech Stack)
*   **Language**: C (MISRA-C 준수 지향), Assembly (Startup code 일부)
*   **Target MCU**: STM32F103 (Cortex-M3) 또는 QEMU (Emulation)
*   **IDE/Compiler**: STM32CubeIDE (GCC) or Keil uVision
*   **Tools**:
    *   Python (PC측 UDS 진단기/펌웨어 전송 툴 제작)
    *   Git (형상 관리)

## 5. 개발 로드맵
1.  **Phase 1 (환경 구축 및 MCAL)**: 개발 환경 셋업, LED/UART/Flash 드라이버 구현 (레지스터 기반).
2.  **Phase 2 (구조 설계)**: 폴더 구조 잡기 (BSW/MCAL/APP), 간단한 스케줄러 및 루프백 테스트.
3.  **Phase 3 (Bootloader & Memory Map)**: 메모리 섹션 분할(Linker Script 수정), Bootloader 진입/점프 로직 구현.
4.  **Phase 4 (OTA 프로토콜)**: PC용 Python 스크립트 작성, UDS 프로토콜을 이용한 바이너리 전송 및 Flash Writing 구현.
5.  **Phase 5 (검증 및 문서화)**: 업데이트 성공/실패 시나리오 테스트, 데모 영상 및 코드 문서화.

## 6. 예상 산출물
*   소스 코드 리포지토리 (GitHub)
*   아키텍처 설계 문서 (Doxygen 또는 다이어그램)
*   OTA 시연 영상 (PC 툴에서 전송 -> MCU 업데이트 -> 재부팅 후 동작 변경 확인)
