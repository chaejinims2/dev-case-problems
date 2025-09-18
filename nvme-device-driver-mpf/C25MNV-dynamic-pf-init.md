# dynamic-pf-init: 동적 디바이스 수 기반 PF 자원 구조 초기화

## Legacy System Context

기존 NVMe 드라이버는 구조는 장치 연결 여부와 관계없이 slot 기반 고정 구조를 따르며, slot 내 PF 수는 설정에 따라 유동적으로 변경됩니다. 하지만 실제 테스트 환경에서는 **설비에 연결된 디바이스 수만큼만 자원을 초기화**하고, 나머지는 비워 두는 방식으로 메모리 낭비를 줄여야 합니다.

이를 위해 사용자 설정(config) 및 디바이스 스캔 결과에 따라 각 PF에 필요한 스레드, 큐, NS, 히스토리 자원을 동적으로 초기화할 수 있어야 합니다.

## Task Objective

전체 슬롯 구조는 고정되어 있으나, 실제 연결된 장치 수에 따라 **각 PF 단위 자원만을 동적으로 초기화**할 수 있는 구조를 설계하고 구현하세요. 이때 사용 가능한 메모리 제한 크기를 고려해야합니다. 

## Design Requirements

- `x_conf_info`에 설정된 max_ns, max_thread, max_queue를 기반으로 PF 구조를 초기화해야 함
- 실제 연결 여부 판단은 초기 장치 스캔 단계에서 처리 (ex: probe success)
- `x_pf` 내부 자원(`x_ns_array`, `x_thread_ctx[]`, `x_q_ctx[]`)은 필요할 때만 동적 생성

## Validation Criteria

- 실제 장치가 연결된 PF에 대해서만 자원이 생성되어야 함
- 설정값과 생성된 구조가 일치해야 함
- 장치 제거 시 정리 루틴이 동작해야 함

## Technical Constraints

- 자원 초기화 여부는 `is_rdy`, `stat_dead` 등의 상태 플래그로 관리
- 상태에 따라 `init_pf()` 또는 `free_pf()` 경로 분기 처리 필요
- PF 수가 많아도 메모리 낭비 없이 작동해야 함

## Expected Deliverables

- `cinit.c`, `cinit.h`, 또는 `cinit_test.c` 구성
- 자원 초기화/해제 여부를 확인하는 로그 출력 필수
- PF 상태 기반 초기화 여부를 테스트하는 스크립트 (선택 사항)