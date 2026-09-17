# PLAN — SkyPort

## 구현 방향

- 코드와 문서는 하나의 저장소에서 관리한다. 실행 코드·테스트·입력·요구사항(`spec.md`)·계획(`plan.md`)은 루트에, 검증 결과·논문·소개 자료는 `docs/`에 둔다. 작업 지침은 루트 `AGENTS.md`에 두고, 소개·사용법·문서 안내는 루트 `README.md` 하나로 통합한다.

- `main.py`(CLI)·`gui/`(정적 HTML)·`data_io/`(입력/CSV·요약) → `core/`(모델·엔진·스냅샷) → `schedulers.base`. `core`는 GUI에 의존하지 않는다.
- 스케줄러는 `core.models`와 `schedulers.base`만 import한다. `select(now, counter, queues)`는 선택 승객을 큐에서 제거해 반환하고, 없으면 `None`을 반환한다.
- 추가 시 `Scheduler`의 `name`·`select()` 구현 후 `schedulers/__init__.py`의 `SCHEDULERS`에 등록한다. CLI·GUI·비교에 자동 반영된다.
- 사용법은 [README.md](README.md), 설계 근거는 [논문](docs/HYBRID_MLQ_PAPER.tex).

## 검증 전략

- [README 테스트](README.md#테스트)의 명령으로 입력 처리·엔진 회귀를 검증한다.
- `python3 main.py --input <입력> --compare`를 [기준선](docs/validation.md#기준선)과 대조한다. 입력은 부하가 다른 `input_light.txt`·`input.txt`·`input_heavy.txt`를 쓴다.

## 리스크 및 미결정

없음. 부하 1.31·2.61에서 스케줄러 순위가 같고 aging 임계값도 둔감함을 [기준선](docs/validation.md#기준선)에서 확인했다.
