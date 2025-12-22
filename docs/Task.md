# Task List: SafeOTA-ECU Project

## Phase 1: Environment Setup & MCAL Driver Implementation
*기반 환경을 구축하고 하드웨어를 직접 제어하는 드라이버를 레지스터 레벨에서 구현합니다.*

### Epic 1.1: Development Environment Setup
- [ ] **Task 1.1.1**: Install STM32CubeIDE & ARM GCC Toolchain.
- [ ] **Task 1.1.2**: Create Project Repository & Initialize Git (Setup `.gitignore`).
- [ ] **Task 1.1.3**: Configure Initial STM32 Project (Clock, Debug Port) using CubeMX (or Manual Setup).

### Epic 1.2: MCAL Driver Development (Register Level)
- [ ] **Task 1.2.1**: Implement **GPIO Driver** (Mode setting, Output, Input reading) directly controlling registers (`CRL`, `CRH`, `IDR`, `ODR`).
- [ ] **Task 1.2.2**: Implement **UART Driver** (Polling & Interrupt mode) for debug output.
- [ ] **Task 1.2.3**: Implement **GPT (Timer) Driver** using SysTick or TIMx for time measurement.
- [ ] **Task 1.2.4**: Implement **Fls (Flash) Driver** (Unlock, Page Erase, Program Word) for internal flash memory.
- [ ] **Task 1.2.5**: Unit Test: Verify all drivers function correctly on hardware (or QEMU).

---

## Phase 2: SW Architecture & Basic Scheduler
*AUTOSAR Layered Architecture를 모사한 폴더 구조를 잡고 기초적인 스케줄러를 구현합니다.*

### Epic 2.1: Layered Architecture Setup
- [ ] **Task 2.1.1**: Define Directory Structure (`ASW`, `RTE`, `BSW`, `MCAL`).
- [ ] **Task 2.1.2**: Define Common Types & Macros (`Std_Types.h`, `Platform_Types.h`) mimicking AUTOSAR standards.
- [ ] **Task 2.1.3**: Refactor Phase 1 Drivers into `MCAL` layer with standardized APIs.

### Epic 2.2: Scheduler Implementation
- [ ] **Task 2.2.1**: Implement `Os_Tick` using SysTick Handler.
- [ ] **Task 2.2.2**: Develop a simple **Round-Robin Scheduler** (or Cyclic Executive).
- [ ] **Task 2.2.3**: Create `RTE` layer to connect `ASW` and `BSW`/`MCAL`.
- [ ] **Task 2.2.4**: Implement Basic `ASW` (e.g., Blinky LED) running on the Scheduler.

---

## Phase 3: Bootloader & Memory Map Design
*펌웨어 업데이트를 위한 메모리 맵을 설계하고 부트로더의 핵심 로직을 상세 구현합니다.*

### Epic 3.1: Memory Layout Design
- [ ] **Task 3.1.1**: Define Memory Map (Bootloader region, Application region, Shared Data/Flag region).
- [ ] **Task 3.1.2**: Modify **Linker Script (`.ld`)** to enforce memory partitions.
- [ ] **Task 3.1.3**: Verify memory allocation using `.map` file analysis.

### Epic 3.2: Bootloader Core Logic
- [ ] **Task 3.2.1**: Implement Bootloader Entry Logic (Check GPIO or Shared RAM Flag).
- [ ] **Task 3.2.2**: Implement **Application Validity Check** (CRC32 or Magic Number verification).
- [ ] **Task 3.2.3**: Implement **Jump to Application** Logic (Reset Stack Pointer & Program Counter).

---

## Phase 4: OTA Protocol Implementation (UDS)
*UDS 프로토콜(ISO 14229)을 이용한 펌웨어 다운로드 기능을 구현합니다.*

### Epic 4.1: UDS Service Implementation (ECU Side)
- [ ] **Task 4.1.1**: Implement Basic UDS Handler Structure (Service ID Dispatcher).
- [ ] **Task 4.1.2**: Implement Service `$10` (Session Control) & `$11` (ECU Reset).
- [ ] **Task 4.1.3**: Implement Service `$27` (Security Access) - Simple Seed & Key logic.
- [ ] **Task 4.1.4**: Implement Service `$31` (Routine Control) for Flash Erase.
- [ ] **Task 4.1.5**: Implement Service `$34`, `$36`, `$37` for Firmware Download & Data Transfer.

### Epic 4.2: PC Tool Development
- [ ] **Task 4.2.1**: Set up Python Environment (`pyserial` library).
- [ ] **Task 4.2.2**: Develop Python Script to parse Firmware Binary (`.bin`).
- [ ] **Task 4.2.3**: Implement UDS Protocol flow in Python (Session -> Auth -> Erase -> Write -> Reset).

---

## Phase 5: Verification & Portfolio Completion
*전체 시스템 통합 테스트를 진행하고 성과를 문서화합니다.*

### Epic 5.1: System Integration & Debugging
- [ ] **Task 5.1.1**: Integrate Bootloader, BSW, and ASW into a single build/deploy flow (or dual project setup).
- [ ] **Task 5.1.2**: Perform Full OTA Test (Update LED, blink pattern change via FW update).
- [ ] **Task 5.1.3**: Test Exception Cases (Update Interruption, Invalid CRC).

### Epic 5.2: Documentation & Demo
- [ ] **Task 5.2.1**: Write detailed `README.md` (Build guide, Architecture explanation).
- [ ] **Task 5.2.2**: Generate Doxygen or Architecture Diagrams.
- [ ] **Task 5.2.3**: Record **Demo Video** (Showcasing Terminal logs & HW behavior).
