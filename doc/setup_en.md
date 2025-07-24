# 설치 가이드

## 시스템 요구 사항

- **OS**: Ubuntu 18.04 이상 권장  
- **GPU**: Nvidia GPU  
- **Driver 버전**: 버전 525 이상 권장  

---

## 1. 가상 환경 생성

훈련 또는 배포 프로그램은 가상 환경에서 실행하는 것이 권장됩니다. 가상 환경을 생성하기 위해 Conda를 사용하는 것이 좋습니다. Conda가 이미 시스템에 설치되어 있다면 1.1 단계를 건너뛸 수 있습니다.

### 1.1 MiniConda 다운로드 및 설치

MiniConda는 가상 환경을 생성하고 관리하기에 적합한 경량 배포판입니다. 다음 명령을 사용하여 다운로드 및 설치하십시오:

```bash
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm ~/miniconda3/miniconda.sh
```

설치 후 Conda를 초기화하십시오:

```bash
~/miniconda3/bin/conda init --all
source ~/.bashrc
```

### 1.2 새 환경 생성

다음 명령을 사용하여 가상 환경을 생성하십시오:

```bash
conda create -n unitree-rl python=3.8
```

### 1.3 가상 환경 활성화

```bash
conda activate unitree-rl
```

---

## 2. 종속성 설치

### 2.1 PyTorch 설치

PyTorch는 모델 훈련 및 추론에 사용되는 신경망 계산 프레임워크입니다. 다음 명령을 사용하여 설치하십시오:

```bash
conda install pytorch==2.3.1 torchvision==0.18.1 torchaudio==2.3.1 pytorch-cuda=12.1 -c pytorch -c nvidia
```

### 2.2 Isaac Gym 설치

Isaac Gym은 Nvidia에서 제공하는 강체 시뮬레이션 및 훈련 프레임워크입니다.

#### 2.2.1 다운로드

[Nvidia 공식 웹사이트](https://developer.nvidia.com/isaac-gym)에서 Isaac Gym을 다운로드하십시오.

#### 2.2.2 설치

패키지를 추출한 후 `isaacgym/python` 폴더로 이동하여 다음 명령을 사용하여 설치하십시오:

```bash
cd isaacgym/python
pip install -e .
```

#### 2.2.3 설치 확인

다음 명령을 실행하십시오. 1080개의 공이 떨어지는 창이 열리면 설치가 성공한 것입니다:

```bash
cd examples
python 1080_balls_of_solitude.py
```

문제가 발생하면 `isaacgym/docs/index.html`에 있는 공식 문서를 참조하십시오.

### 2.3 rsl_rl 설치

`rsl_rl`은 강화 학습 알고리즘을 구현하는 라이브러리입니다.

#### 2.3.1 다운로드

Git을 사용하여 저장소를 클론하십시오:

```bash
git clone https://github.com/leggedrobotics/rsl_rl.git
```

#### 2.3.2 브랜치 전환

v1.0.2 브랜치로 전환하십시오:

```bash
cd rsl_rl
git checkout v1.0.2
```

#### 2.3.3 설치

```bash
pip install -e .
```

### 2.4 unitree_rl_gym 설치

#### 2.4.1 다운로드

Git을 사용하여 저장소를 클론하십시오:

```bash
git clone https://github.com/unitreerobotics/unitree_rl_gym.git
```

#### 2.4.2 설치

디렉토리로 이동하여 설치하십시오:

```bash
cd unitree_rl_gym
pip install -e .
```

### 2.5 unitree_sdk2py 설치 (선택 사항)

`unitree_sdk2py`는 실제 로봇과의 통신에 사용되는 라이브러리입니다. 훈련된 모델을 물리적 로봇에 배포해야 하는 경우 이 라이브러리를 설치하십시오.

#### 2.5.1 다운로드

Git을 사용하여 저장소를 클론하십시오:

```bash
git clone https://github.com/unitreerobotics/unitree_sdk2_python.git
```

#### 2.5.2 설치

디렉토리로 이동하여 설치하십시오:

```bash
cd unitree_sdk2_python
pip install -e .
```

---

## 요약

위 단계를 완료하면 가상 환경에서 관련 프로그램을 실행할 준비가 됩니다. 문제가 발생하면 각 구성 요소의 공식 문서를 참조하거나 종속성이 올바르게 설치되었는지 확인하십시오.

