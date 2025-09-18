# get-mpf-major: Slot, Func 기반 MPF 드라이버 인스턴스 Major 번호 계산 함수 설계

## Legacy System Context

기존 NVMe 드라이버는 단일 PF를 전제로 설계되었기 때문에, 인스턴스 식별을 위한 Major 번호 체계가 별도로 존재하지 않았습니다. 하지만 MPF(Multi PF) 구조가 도입되면서, 각 PF 인스턴스에게 고유한 Major 번호를 부여하고 이를 통해 논리적으로 식별할 수 있는 체계가 필요해졌습니다.

기존 설비에서는 `slot`과 `func` 정보를 기반으로 PF 인스턴스를 구분해야 하는 상황입니다. 따라서 Slot 번호와 PCI Function 번호를 이용하여 고유 Major 번호를 산출할 수 있는 계산 함수의 구현이 요구됩니다.

## Task Objective

슬롯 기반으로 동작하는 MPF 장치에서, 각 PF 인스턴스에 대해 `slot`과 `func` 정보를 조합하여 고유 Major 번호를 산출하는 함수를 작성하세요.

## Design Requirements

- `my_nvme_get_major_num(struct pci_dev*)` 함수 내에서 Major 번호를 계산할 것
- `x_conf_info` 구조체에서 PF 수(`conf->pf`)를 동적으로 조회할 것
- `x_pci_info` 또는 `PCI_SLOT()/PCI_FUNC()`를 활용하여 Slot 및 Func를 추출할 것

## Validation Criteria

- Major 계산식이 **확장성 있게** 구성되어야 함
- Slot과 Func 추출이 정확히 이루어져야 함
- Major = Slot * PF_Total + Func 형태로 계산되는 구조가 유지되어야 함
- 로그 출력 (`printk`)을 통해 계산 과정을 검증할 수 있어야 함

## Technical Constraints

- PCI 함수 번호는 `PCI_FUNC(devfn)` 매크로로 추출할 것
- Slot 정보는 기존 PCI 버스 매핑(`x_pci_info`) 또는 `PCI_SLOT(devfn)`에서 획득
- Slot → PF 매핑을 위한 테이블이 있을 경우, 반드시 최신화된 버전 참조

## Expected Deliverables

- 구현 파일: `my_nvme_major.c` 또는 기존 `my_devctrl.c` 내 함수 추가
- 테스트 로그: Major 번호 계산식, 입력값(slot, func), 출력값(major)을 포함할 것
- 자동 테스트 스크립트: `mpf_major_test.sh` 내 단위 테스트 포함 (선택 사항)

