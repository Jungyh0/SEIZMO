# SEIZMO
> *Seize the moment — 순간을 잡는다.*

AI 얼굴 합성(Face Swap) 기술을 활용한 포토부스 웹 서비스입니다.  
사용자가 촬영한 사진에 영화 포스터, 밈, 셀럽 등 다양한 배경과 얼굴을 자연스럽게 합성하여 특별한 순간을 만들어드립니다.

---

## 팀원
| 최강성 | 정영환 | 김민수 | 윤태빈 |
|:---:|:---:|:---:|:---:|

---

## 주요 기능

- **BASIC MODE** — 일반 촬영 후 프레임 선택 및 사진 출력
- **AI PLUS MODE** — AI 얼굴 합성 기술로 영화 포스터, 밈, 셀럽 이미지에 사용자 얼굴을 자연스럽게 합성
- 8장 순차 촬영 → 4장 선택 → 프레임 적용 → AI 합성 → 결과 출력
- QR코드를 통해 촬영 결과물 저장 가능

---

## 기술 스택

| 분류 | 기술 |
|------|------|
| Frontend | HTML5, CSS3, JavaScript |
| Backend | Python, Flask |
| AI | InsightFace (buffalo_l), ONNX Runtime, PyTorch |
| 카메라 | OpenCV (selfycam) |
| 환경 | Anaconda |

---

## 시스템 아키텍처

```
USER (Photo Booth)
    ↓
selfycam (OpenCV)
    ↓
Frontend (HTML / JS / CSS)  ←→  Backend (Python / Flask)
                                        ↓
                              Face Swap (InsightFace + ONNX)
                                        ↓
                                  결과 이미지 출력 (PRINT)
```

---

## 설치 방법

### 1. 저장소 클론
```bash
git clone https://github.com/Jungyh0/SEIZMO.git
cd SEIZMO
```

### 2. 가상환경 생성 (Anaconda 권장)
```bash
conda create -n seizmo python=3.12
conda activate seizmo
```

### 3. 라이브러리 설치
```bash
pip install flask insightface onnxruntime torch torchvision opencv-python pillow numpy matplotlib
```

### 4. 모델 파일 다운로드 ⚠️
아래 파일들은 용량 문제로 GitHub에 포함되어 있지 않습니다.  
**[Google Drive에서 다운로드](https://drive.google.com/drive/u/0/folders/1raIWTha1gkdu7glr_oNARf2tMzNju6R7)**

다운로드 후 아래 경로에 넣어주세요:

```
static/
└── weights/
    ├── inswapper_128.onnx          ← 얼굴 교체 핵심 모델 (528MB)
    └── models/
        └── buffalo_l/
            ├── 1k3d68.onnx         ← 3D 랜드마크 검출
            ├── 2d106det.onnx       ← 2D 랜드마크 검출
            ├── det_10g.onnx        ← 얼굴 감지
            ├── genderage.onnx      ← 성별/나이 추정
            └── w600k_r50.onnx      ← 얼굴 인식
```

---

## 실행 방법

```bash
python selfy_webserver.py
```

실행 후 브라우저에서 접속:
```
http://localhost:5000
```

---

## 서비스 플로우

1. 초기 화면 접속
2. 촬영 페이지에서 **8장 순차 촬영** (10초 간격)
3. 촬영된 이미지 중 **4장 선택**
4. 프레임 적용 (1차)
5. AI PLUS MODE 선택 시 → 영화 포스터 / 밈 / 셀럽 이미지에 **얼굴 합성**
6. 결과 이미지 확인 및 **QR코드로 저장**
