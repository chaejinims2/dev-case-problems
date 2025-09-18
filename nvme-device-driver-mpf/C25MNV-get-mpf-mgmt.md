# get-mpf-mgmt: MPF 인스턴스 초기화를 위한 전역 구조 접근 함수 구현

## Legacy System Context

기존 NVMe 드라이버는 구조는 장치 연결 여부와 관계없이 전역 구조(`x_mgmt`, `x_slot` 등)를 기반으로 설계되어 있으며, 각 PF 인스턴스는 슬롯 배열 내에 동적으로 할당됩니다. 사용자는 특정 PCI 장치 정보에 기반하여 자신이 속한 PF 인스턴스에 접근해야 하며, 이를 위한 매핑 함수가 필요합니다.

기존 방식은 단일 PF 전제 하에 `nvme_ctrl` 하나만을 관리하므로, MPF 구조에서는 적절한 인스턴스를 찾아주는 별도 로직이 필요합니다.

## Task Objective

PCI 장치 정보(`struct pci_dev`)를 기반으로 전체 `g_mgmt` 구조에서 **해당 PF 인스턴스에 대응하는 전역 구조체 포인터**를 찾아 반환하는 함수 `get_mpf_mgmt()`를 구현하세요.

## Design Requirements

- 함수 입력값은 `struct pci_dev* pdev`이어야 함
- 내부적으로 `my_pci_info` 테이블을 기반으로 `slot`을 계산해야 함
- `g_mgmt.slot_array[slot]` 내에서 해당 `x_pci` 인스턴스를 찾아 반환

## Validation Criteria

- `pdev`의 bus/device/function 정보가 정확히 매핑되는지 확인
- 유효하지 않은 경우 `NULL`을 반환하고 로그 출력
- 반환된 구조체 포인터가 실제 데이터와 일치하는지 검증

## Technical Constraints

- `pci_bus`, `pci_devfn`, `PCI_SLOT`, `PCI_FUNC` 등의 매크로 활용
- `my_nvme_find_dn_bus()` 함수 내에서 구현할 것

## Expected Deliverables

- `get_mpf_mgmt()` 함수 정의 및 테스트 코드 포함
- `printk()`를 통한 슬롯/인스턴스 정보 로그 출력
- `validate.sh`를 통한 정상 매핑 여부 자동 검사
