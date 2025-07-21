<div align="center">
  <h1 align="center">Unitree RL GYM</h1>
  <p align="center">
    <span> 🌎English </span> | <a href="README_zh.md"> 🇨🇳中文 </a>
  </p>
</div>

<p align="center">
  <strong>이 저장소는 Unitree 로봇을 기반으로 한 강화 학습 구현을 위한 것으로, Unitree Go2, H1, H1_2, G1을 지원합니다.</strong> 
</p>

<div align="center">

| <div align="center"> Isaac Gym </div> | <div align="center">  Mujoco </div> |  <div align="center"> Physical </div> |
|--- | --- | --- |
| [<img src="https://oss-global-cdn.unitree.com/static/32f06dc9dfe4452dac300dda45e86b34.GIF" width="240px">](https://oss-global-cdn.unitree.com/static/5bbc5ab1d551407080ca9d58d7bec1c8.mp4) | [<img src="https://oss-global-cdn.unitree.com/static/244cd5c4f823495fbfb67ef08f56aa33.GIF" width="240px">](https://oss-global-cdn.unitree.com/static/5aa48535ffd641e2932c0ba45c8e7854.mp4) | [<img src="https://oss-global-cdn.unitree.com/static/78c61459d3ab41448cfdb31f6a537e8b.GIF" width="240px">](https://oss-global-cdn.unitree.com/static/0818dcf7a6874b92997354d628adcacd.mp4) |

</div>

---

## 📦 설치 및 설정

설치 및 설정 단계는 [setup.md](/doc/setup_en.md)를 참조하십시오.

## 🔁 프로세스 개요

강화 학습을 사용하여 모션 제어를 달성하기 위한 기본 워크플로우는 다음과 같습니다:

`Train` → `Play` → `Sim2Sim` → `Sim2Real`

- **Train**: Gym 시뮬레이션 환경을 사용하여 로봇이 환경과 상호작용하고 설계된 보상을 최대화하는 정책을 찾도록 합니다. 훈련 중 실시간 시각화는 효율 감소를 방지하기 위해 권장되지 않습니다.
- **Play**: 훈련된 정책을 검증하고 기대에 부합하는지 확인합니다.
- **Sim2Sim**: Gym에서 훈련된 정책을 다른 시뮬레이터에 배포하여 Gym 특성에 과도하게 의존하지 않도록 합니다.
- **Sim2Real**: 정책을 실제 로봇에 배포하여 모션 제어를 달성합니다.

## 🛠️ 사용자 가이드

### 1. 훈련

다음 명령을 실행하여 훈련을 시작하십시오:

```bash
python legged_gym/scripts/train.py --task=xxx
```

#### ⚙️ 매개변수 설명
- `--task`: 필수 매개변수; 값은 (go2, g1, h1, h1_2)일 수 있습니다.
- `--headless`: 기본적으로 그래픽 인터페이스로 시작; 헤드리스 모드(효율성 향상)를 위해 true로 설정하십시오.
- `--resume`: 로그에서 체크포인트를 사용하여 훈련을 재개합니다.
- `--experiment_name`: 실행/로드할 실험 이름.
- `--run_name`: 실행/로드할 실행 이름.
- `--load_run`: 로드할 실행 이름; 기본값은 최신 실행입니다.
- `--checkpoint`: 로드할 체크포인트 번호; 기본값은 최신 파일입니다.
- `--num_envs`: 병렬 훈련을 위한 환경 수.
- `--seed`: 랜덤 시드.
- `--max_iterations`: 최대 훈련 반복 횟수.
- `--sim_device`: 시뮬레이션 계산 장치; CPU를 지정하려면 `--sim_device=cpu`를 사용하십시오.
- `--rl_device`: 강화 학습 계산 장치; CPU를 지정하려면 `--rl_device=cpu`를 사용하십시오.

**기본 훈련 결과 디렉토리**: `logs/<experiment_name>/<date_time>_<run_name>/model_<iteration>.pt`

---

### 2. 플레이

Gym에서 훈련 결과를 시각화하려면 다음 명령을 실행하십시오:

```bash
python legged_gym/scripts/play.py --task=xxx
```

**설명**:

- Play의 매개변수는 Train과 동일합니다.
- 기본적으로 실험 폴더의 마지막 실행에서 최신 모델을 로드합니다.
- `load_run` 및 `checkpoint`를 사용하여 다른 모델을 지정할 수 있습니다.

#### 💾 네트워크 내보내기

Play는 Actor 네트워크를 내보내며, 이를 `logs/{experiment_name}/exported/policies`에 저장합니다:
- 표준 네트워크(MLP)는 `policy_1.pt`로 내보냅니다.
- RNN 네트워크는 `policy_lstm_1.pt`로 내보냅니다.

### 플레이 결과

| Go2 | G1 | H1 | H1_2 |
|--- | --- | --- | --- |
| [![go2](https://oss-global-cdn.unitree.com/static/ba006789e0af4fe3867255f507032cd7.GIF)](https://oss-global-cdn.unitree.com/static/d2e8da875473457c8d5d69c3de58b24d.mp4) | [![g1](https://oss-global-cdn.unitree.com/static/32f06dc9dfe4452dac300dda45e86b34.GIF)](https://oss-global-cdn.unitree.com/static/5bbc5ab1d551407080ca9d58d7bec1c8.mp4) | [![h1](https://oss-global-cdn.unitree.com/static/fa04e73966934efa9838e9c389f48fa2.GIF)](https://oss-global-cdn.unitree.com/static/522128f4640c4f348296d2761a33bf98.mp4) |[![h1_2](https://oss-global-cdn.unitree.com/static/83ed59ca0dab4a51906aff1f93428650.GIF)](https://oss-global-cdn.unitree.com/static/15fa46984f2343cb83342fd39f5ab7b2.mp4)|

---

### 3. Sim2Sim (Mujoco)

Mujoco 시뮬레이터에서 Sim2Sim을 실행하십시오:

```bash
python deploy/deploy_mujoco/deploy_mujoco.py {config_name}
```

#### 매개변수 설명
- `config_name`: 구성 파일; 기본 검색 경로는 `deploy/deploy_mujoco/configs/`입니다.

#### 예: G1 실행

```bash
python deploy/deploy_mujoco/deploy_mujoco.py g1.yaml
```

#### ➡️ 네트워크 모델 교체

기본 모델은 `deploy/pre_train/{robot}/motion.pt`에 위치하며, 사용자 정의 훈련 모델은 `logs/g1/exported/policies/policy_lstm_1.pt`에 저장됩니다. YAML 구성 파일에서 `policy_path`를 업데이트하십시오.

#### 시뮬레이션 결과

| G1 | H1 | H1_2 |
|--- | --- | --- |
| [![mujoco_g1](https://oss-global-cdn.unitree.com/static/244cd5c4f823495fbfb67ef08f56aa33.GIF)](https://oss-global-cdn.unitree.com/static/5aa48535ffd641e2932c0ba45c8e7854.mp4)  |  [![mujoco_h1](https://oss-global-cdn.unitree.com/static/7ab4e8392e794e01b975efa205ef491e.GIF)](https://oss-global-cdn.unitree.com/static/8934052becd84d08bc8c18c95849cf32.mp4)  |  [![mujoco_h1_2](https://oss-global-cdn.unitree.com/static/2905e2fe9b3340159d749d5e0bc95cc4.GIF)](https://oss-global-cdn.unitree.com/static/ee7ee85bd6d249989a905c55c7a9d305.mp4) |


---

### 4. Sim2Real (물리적 배포)

물리적 로봇에 배포하기 전에 디버그 모드인지 확인하십시오. 자세한 단계는 [물리적 배포 가이드](deploy/deploy_real/README.md)를 참조하십시오:

```bash
python deploy/deploy_real/deploy_real.py {net_interface} {config_name}
```

#### 매개변수 설명
- `net_interface`: 로봇에 연결된 네트워크 카드 이름, 예: `enp3s0`.
- `config_name`: `deploy/deploy_real/configs/`에 위치한 구성 파일, 예: `g1.yaml`, `h1.yaml`, `h1_2.yaml`.

#### 배포 결과
| G1 | H1 | H1_2 |
|--- | --- | --- |
| [![real_g1](https://oss-global-cdn.unitree.com/static/78c61459d3ab41448cfdb31f6a537e8b.GIF)](https://oss-global-cdn.unitree.com/static/0818dcf7a6874b92997354d628adcacd.mp4) | [![real_h1](https://oss-global-cdn.unitree.com/static/fa07b2fd2ad64bb08e6b624d39336245.GIF)](https://oss-global-cdn.unitree.com/static/ea0084038d384e3eaa73b961f33e6210.mp4) | [![real_h1_2](https://oss-global-cdn.unitree.com/static/a88915e3523546128a79520aa3e20979.GIF)](https://oss-global-cdn.unitree.com/static/12d041a7906e489fae79d55b091a63dd.mp4) |

---

## 🎉 감사의 말

이 저장소는 다음 오픈 소스 프로젝트의 지원과 기여를 바탕으로 구축되었습니다. 특별히 감사드립니다:

- [legged\_gym](https://github.com/leggedrobotics/legged_gym): 훈련 및 실행 코드의 기반.
- [rsl\_rl](https://github.com/leggedrobotics/rsl_rl.git): 강화 학습 알고리즘 구현.
- [mujoco](https://github.com/google-deepmind/mujoco.git): 강력한 시뮬레이션 기능 제공.
- [unitree\_sdk2\_python](https://github.com/unitreerobotics/unitree_sdk2_python.git): 물리적 배포를 위한 하드웨어 통신 인터페이스.

---

## 🔖 라이센스

이 프로젝트는 [BSD 3-Clause License](./LICENSE)에 따라 라이센스가 부여됩니다:
1. 원래 저작권 고지를 유지해야 합니다.
2. 프로젝트 이름 또는 조직 이름은 홍보에 사용될 수 없습니다.
3. 모든 수정 사항은 공개되어야 합니다.

자세한 내용은 전체 [LICENSE 파일](./LICENSE)을 읽어보십시오.

