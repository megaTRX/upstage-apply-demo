# SafeOTA-ECU

**AUTOSAR 계층 구조 기반의 신뢰성 있는 OTA 업데이트 시스템**

현대모비스 MCU 임베디드 SW 직무 지원을 위한 포트폴리오 프로젝트입니다.

## 📊 프레젠테이션

프로젝트 소개 프레젠테이션은 다음 링크에서 확인하실 수 있습니다:

🔗 **[SafeOTA-ECU 프레젠테이션 보기](https://megatrx.github.io/upstage-apply-demo/)**

### 로컬에서 프레젠테이션 보기

```bash
# 의존성 설치
npm install

# 프레젠테이션 빌드
npm run build:presentation

# 로컬 서버로 프레젠테이션 보기 (http://localhost:8080)
npm run serve
```

## 🎯 프로젝트 목표

현대모비스 'MCU 임베디드 SW' 직무 핵심 역량 증명:
- AUTOSAR 기반 BSW 개발
- MCU 제어 및 MCAL/CDD 개발
- OTA 업데이트 기능 구현
- 기능 안전 (무결성 검증)

## 🏗️ 시스템 아키텍처

```
┌─────────────────────────────────────┐
│  ASW (Application Software)        │  ← OTA 테스트 로직
├─────────────────────────────────────┤
│  RTE (Runtime Environment)          │  ← 인터페이스 & 스케줄러
├─────────────────────────────────────┤
│  BSW (Basic Software)               │
│  - DCM Lite (UDS)                   │  ← 진단 통신
│  - MEMIF (Memory Interface)         │  ← 메모리 추상화
├─────────────────────────────────────┤
│  MCAL (Microcontroller Abstraction) │
│  - GPT, UART, Fls Drivers           │  ← 하드웨어 제어
└─────────────────────────────────────┘
```

## 🔧 기술 스택

- **Core**: C (MISRA-C 지향), Assembly
- **Hardware**: STM32F103 (Cortex-M3) / QEMU
- **Tools**: 
  - IDE: STM32CubeIDE / Keil
  - PC Tool: Python (UDS 진단 툴)
  - Version Control: Git

## 📋 개발 로드맵

### Phase 1: Environment Setup & MCAL Driver Implementation
- [x] Development Environment Setup
- [ ] MCAL Driver Development (GPIO, UART, GPT, Flash)

### Phase 2: SW Architecture & Basic Scheduler
- [ ] Layered Architecture Setup
- [ ] Scheduler Implementation

### Phase 3: Bootloader & Memory Map Design
- [ ] Memory Layout Design
- [ ] Bootloader Core Logic

### Phase 4: OTA Protocol Implementation (UDS)
- [ ] UDS Service Implementation
- [ ] PC Tool Development

### Phase 5: Verification & Portfolio Completion
- [ ] System Integration & Debugging
- [ ] Documentation & Demo

## 📚 문서

- [프로젝트 기획](./Ideation.md)
- [제품 요구사항 문서 (PRD)](./docs/PRD.md)
- [기술 스택](./docs/TechStack.md)
- [작업 목록](./docs/Task.md)
- [프레젠테이션](./docs/Presentation.md)

## 🚀 향후 계획

AI 기반 진단 보조 시스템 도입:
- **Chunking**: ECU 매뉴얼을 의미 있는 단위로 분할
- **ChromaDB**: 벡터 데이터베이스를 활용한 빠른 정보 검색
- **RAG**: 에러 코드 발생 시 관련 매뉴얼 페이지 자동 제공

## 📄 License

MIT License

---

**현대모비스 MCU 임베디드 SW 직무 지원 포트폴리오**
