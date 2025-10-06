🧠 문서 의도 해석 기반 멀티모달 AI 모델

Document Layout & Text Understanding System using YOLO + Qwen2-VL

📘 프로젝트 개요

문서(.pdf, .pptx, .jpg 등)에 포함된 텍스트, 표, 이미지, 수식 등의 시각적 요소를 인식하고
인간이 문서로 표현한 의미와 구조를 해석하기 위한 멀티모달 AI 모델을 개발함.

이 시스템은

YOLOv12l 모델로 문서 레이아웃을 탐지하고

Qwen2.5-VL-3B-Instruct 모델을 활용해 각 영역의 텍스트를 OCR 및 의미 단위로 추출하며

pytesseract를 보조 OCR로 사용하여 오류 상황에도 안정적으로 텍스트를 인식함.

📁 본 프로젝트는 삼성전자 AI 경진대회 “문서 의도 해석 AI” 코드 제출형 과제를 위해 개발되었음.

🧩 주요 기능
기능	설명
문서 레이아웃 탐지	YOLOv12l 모델을 활용하여 문서 내 title, subtitle, text, image, table, equation 등 6개 요소 탐지
OCR 텍스트 인식	Qwen2-VL 멀티모달 LLM으로 이미지 내 텍스트를 인식하고 자연어 형태로 출력
백업 OCR	모델 로딩 실패나 에러 발생 시 pytesseract 기반 OCR로 자동 전환
PDF/PPTX 변환	LibreOffice + pdf2image를 활용하여 모든 문서 포맷을 이미지로 변환
시각화 기능	감지된 요소의 바운딩박스 및 텍스트 영역을 이미지로 시각화
자동 제출 파일 생성	추론 결과를 submission.csv 형식으로 자동 저장 (평가 서버 호환)
🛠️ 기술 스택
구분	사용 기술
언어	Python 3.10
딥러닝 프레임워크	PyTorch, Transformers, Ultralytics YOLO
OCR 및 비전 처리	Qwen2-VL, pytesseract, pdf2image, PIL
환경	CUDA (T4 GPU, 16GB VRAM), 오프라인 환경
기타	LibreOffice (PPTX → PDF 변환), Pandas, Subprocess
📂 프로젝트 구조
📦 project_root
├── model/
│   ├── yolov12l-doclaynet.pt           # 문서 레이아웃 YOLO 모델
│   └── qwen2.5-vl-3b-instruct/         # 로컬 멀티모달 OCR 모델
├── data/
│   ├── test.csv                        # 테스트 데이터 경로 및 메타 정보
│   └── test/                           # 입력 문서(pdf, pptx, jpg 등)
├── output/
│   └── submission.csv                  # 추론 결과 (대회 제출 파일)
├── script.py                            # 메인 추론 스크립트
└── requirements.txt                     # 패키지 의존성

🚀 실행 방법
1️⃣ 환경 세팅
# 가상환경 생성
conda create -n vdu python=3.10
conda activate vdu

# 필수 패키지 설치
pip install -r requirements.txt

2️⃣ 모델 파일 구조
model/
 ├── yolov12l-doclaynet.pt
 └── qwen2.5-vl-3b-instruct/
      ├── config.json
      ├── tokenizer/
      ├── processor/
      └── model.safetensors

3️⃣ 추론 실행
python script.py


결과 파일
./output/submission.csv
→ 평가 서버 제출용 CSV 자동 생성됨.

🧠 모델 구성
모델	역할	설명
YOLOv12l (DocLayNet fine-tuned)	문서 레이아웃 탐지	문서 내 주요 구성요소(title, text 등)의 영역을 탐지함
Qwen2.5-VL-3B-Instruct (VLM)	OCR 및 텍스트 인식	이미지 내 텍스트를 추출하고 문맥적으로 정제된 텍스트를 생성함
pytesseract	백업 OCR	모델 불안정 시 fallback용 텍스트 추출
📈 결과 및 성과

다양한 문서 포맷(PDF, PPTX, 이미지)에 대한 통합 추론 파이프라인 구축함.

YOLO + Qwen2-VL 기반 멀티모달 모델을 활용해 문서 구조와 텍스트 의미를 동시 인식함.

대회 평가 환경(T4 GPU, 오프라인, 60분 제한) 내 완전 자동 실행 검증 완료함.

시각화 및 debug 기능을 통해 탐지 정확도 개선 및 오류 분석 용이성을 확보함.

🔍 예시 결과
원본 문서	탐지 결과

	
💬 프로젝트 회고

YOLO와 VLM 간 입력 크기 및 bbox 좌표 정합 문제를 직접 해결하며 멀티모달 데이터 흐름 이해도 향상함.

GPU 메모리(OOM) 및 추론 시간 최적화를 위해 이미지 크기, confidence threshold, batch 처리 등을 실험적으로 조정함.

OCR 품질 향상을 위해 pytesseract와 Qwen-VL의 결과를 비교 및 조합하며 모델 안정성을 확보함.

문서의 시각적 구조와 의미를 AI가 이해하도록 하는 기반 기술을 실제 구현 경험으로 체득함.
