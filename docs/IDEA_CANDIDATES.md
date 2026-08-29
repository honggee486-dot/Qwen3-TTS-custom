# 후속 아이디어 후보

현재 커스텀 음성 생성 환경을 안정적으로 반복 사용하기 위한 운영 후보 모음이다. 기존 모델 코드·라이선스·원본 기능 계약을 바꾸지 않으며, 실제 병목이 확인될 때만 구현한다.

## 1. Runtime Capability Probe

실행 전에 현재 장비와 설치 상태에서 가능한 profile을 자동 진단하는 후보.

- CUDA/device 사용 가능 여부
- VRAM 여유
- 지원 dtype/attention backend
- 선택 모델 로드 가능성
- streaming 가능 여부
- voice clone / design / preset voice capability

진단 실패와 실제 합성 실패를 구분한다.

## 2. Resource Profile

장비 조건에 따라 반복 가능한 실행 profile을 저장하는 후보.

```text
low-memory
balanced
quality
streaming
```

각 profile은 dtype, attention, batch, offload, model size 같은 실행 옵션을 묶되 특정 PC 경로를 저장소 계약으로 고정하지 않는다.

## 3. Reproducible Sample Artifact

음성 비교 시 다음을 함께 보존하는 로컬 artifact 후보.

- 입력 텍스트 hash
- language / speaker / instruction
- generation parameter
- model/profile fingerprint
- 생성 시간
- 출력 duration

실제 음성 파일과 개인 voice reference는 Git에 넣지 않는다.

## 4. 고정 Evaluation Set

한국어 일반 문장, 대화, 숫자, 고유명사, 감정·속도 지시 등 작은 고정 평가 세트를 두고 변경 전후를 같은 조건으로 비교하는 후보.

평가 항목:

- 발음 오류
- 자연스러움
- speaker identity 유지
- instruction 준수
- latency
- peak VRAM

## 5. Job / Progress Contract

향후 다른 앱에서 호출할 때 직접 라이브러리 내부 상태를 읽지 않고 공통 job/result 경계를 사용하는 후보.

```text
prepare
→ load
→ synthesize
→ write
→ completed / failed
```

## 6. Voice Reference 안전 경계

개인 음성 reference와 생성 결과는 local-only data로 취급하고 경로·hash·메타데이터가 실수로 공개 저장소에 들어가지 않도록 별도 검사 후보를 둔다.
