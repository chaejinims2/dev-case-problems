# dev-case-problems

nvme-device-driver-mpf : C25MNV
ssink-image-3d-simulator
pdamp-pcb-damage-inspector
cskils-levelup-codespace
cutils-custom-toolset

## list
1.	gmgmt-struct → x_mgmt, x_slot, x_pci 등의 전역 구조체 설계 및 초기화 문제
2.	pci-bus-map → pdev → bus/dev 기반으로 Slot/Port를 매핑하는 기능 문제
3.	mpf-major-calc → slot/port/func를 기반으로 major number를 산출하는 로직 문제
4.	mpf-init → 드라이버 init 시 PF 구조 출력 (dmesg 기반 확인)
5.	pf-ioctl → PF별 상태 변수(nssro, use_sgl, stat_dead, is_rdy)를 동적 제어하는 사용자 ioctl 문제 
6.	ns-array-view → PF별 namespace array 구조를 출력 및 확인하는 기능 문제
7.	mock-pci-probe → mock pci_dev를 이용하여 probe 동작을 시뮬레이션하는 문제
8.	modsize-check → 최적화 전/후 모듈 크기 비교 및 경량화 검증 (예: 198MB → 225KB)
9.	user-major-query → 사용자 영역에서 ioctl 요청으로 major number를 받아오는 흐름 검증 문제
10.	hist-cqueue → circular queue 기반 PF별 history 저장 및 spinlock 동기화 구현 문제
11.	mpf-dynamic-alloc → 실제 장치 수 기반으로 I/O thread, queue depth, namespace 수 등을 동적으로 할당하는 문제
12.	pf-control-csts → 사용자 요청에 따라 특정 PF의 CSTS, ERRCODE 등만 선택적으로 제어하는 문제
13.	inst-refcnt-list → nvme_ctrl 수정 없이, 새로운 전역 구조체에서 refcnt와 list를 활용해 인스턴스 단위 자원 관리하는 문제



## License
This repository is licensed under the [CC BY-NC-SA 4.- License](https://creativecommons.org/licenses/by-nc-sa/4.0/).
