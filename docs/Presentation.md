---
marp: true
theme: default
paginate: true
header: "SafeOTA-ECU 프로젝트"
footer: "현대모비스 포트폴리오"
backgroundColor: #fff
style: |
  section {
    font-family: 'Malgun Gothic', 'Apple SD Gothic Neo', sans-serif;
  }
  h1 {
    color: #002c5f;
  }
  h2 {
    color: #00509d;
  }
---

<!-- _class: lead -->

# SafeOTA-ECU
## AUTOSAR 계층 구조 기반의 신뢰성 있는 OTA 업데이트 시스템
### 현대모비스 MCU 임베디드 SW 직무 지원 포트폴리오

---

# 1. 프로젝트 개요

- **프로젝트명**: SafeOTA-ECU
- **목표**: 현대모비스 'MCU 임베디드 SW' 직무 핵심 역량 증명
    - AUTOSAR 기반 BSW 개발
    - MCU 제어 및 MCAL/CDD 개발
    - OTA 업데이트 기능 구현
    - 기능 안전 (무결성 검증)
- **개발 기간**: 4주 (예상)

---

# 2. 직무 요구사항 매핑

| 채용 공고 요구사항 | 프로젝트 구현 요소 | 기대 효과 |
| :--- | :--- | :--- |
| **AUTOSAR BSW** | 계층형 아키텍처 (App-RTE-BSW-MCAL) | 표준 스택/구조 이해도 증명 |
| **MCU MCAL/CDD** | STM32 레지스터 직접 제어 (UART/Flash) | HW 제어 능력 및 C 숙련도 |
| **OTA 다운로드** | 듀얼 뱅크/파티션 기반 펌웨어 업데이트 | 부트로더 및 메모리 설계 역량 |
| **차량 보안** | CRC-32 및 롤백(Roll-back) 메커니즘 | 신뢰성 및 안전성 고려 경험 |

---

# 3. 시스템 아키텍처 (Layered)

- **ASW (Application)**: OTA 테스트 로직 (LED 점멸, 상태 보고). RTE로 격리.
- **RTE (Run Time Env)**: 인터페이스 정의 및 스케줄러 (Cyclic Task).
- **BSW (Service Layer)**: 
    - **DCM Lite**: UDS (ISO 14229) 모사 ($10, $27, $34, $36, $37).
    - **MEMIF**: 플래시 메모리 추상화 인터페이스.
- **MCAL (Microcontroller Abstraction)**:
    - **Drivers**: GPT (타이머), UART (통신), Fls (내장 플래시).

---

# 4. 핵심 기능: OTA 시뮬레이션

## 부트로더 (Bootloader) 로직
1.  **진입**: GPIO 버튼 입력 또는 공유 플래그 확인.
2.  **검증**: App 영역의 CRC/Magic Number 무결성 검사.
3.  **점프**: 스택 포인터(SP) 및 PC 초기화 후 App 실행.

## 업데이트 시퀀스 (UDS 기반)
1.  **세션 제어 ($10)** -> **보안 접근 ($27)**
2.  **지우기 ($31)** -> **다운로드 ($34/$36)** -> **종료 ($37)**
3.  **ECU 리셋 ($11)**

---

# 5. 기술 스택 (Tech Stack)

- **Core**: C (MISRA-C 준수 지향), Assembly (Startup code)
- **Hardware**: STM32F103 (Cortex-M3) / QEMU
- **Tools**: 
    - **IDE**: STM32CubeIDE / Keil
    - **PC Tool**: Python (UDS 진단/전송 툴)
    - **Version Control**: Git

---

# 6. 개발 로드맵

1.  **Phase 1**: 환경 구축 및 Low-level MCAL 드라이버 개발.
2.  **Phase 2**: 계층형 아키텍처 구조 잡기 및 스케줄러 구현.
3.  **Phase 3**: 메모리 맵 설계 및 부트로더 진입/점프 로직.
4.  **Phase 4**: OTA 프로토콜 (UDS) 구현 및 PC 툴 제작.
5.  **Phase 5**: 전체 검증, 시연 영상 및 문서화.

---

# 향후 계획: AI 기반 진단 (RAG)

시스템을 고도화하기 위해 **AI 기반 진단 보조 시스템** 도입을 계획 중입니다.

## 핵심 개념
*   **Chunking (청킹)**
    *   **정의**: 방대한 문서(예: ECU 매뉴얼, 오토사 스펙)를 의미 있는 작은 단위로 쪼개는 기술.
    *   **이유**: LLM의 컨텍스트 제한을 극복하고, 정확한 정보 검색을 돕기 위함.
*   **ChromaDB (크로마DB)**
    *   **정의**: AI 애플리케이션에 최적화된 오픈소스 **벡터 데이터베이스**.
    *   **역활**: 청킹된 문서의 "임베딩(Embeddings)"을 저장. 특정 에러 코드(예: U0100) 발생 시 관련 매뉴얼 페이지를 즉시 검색하여 제공.

---

<!-- _class: lead -->

# 경청해 주셔서 감사합니다
## Q & A
