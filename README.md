# 🩹 데일리 스킨 케어 플래너 (Daily Skin Care Planner)

> AI 기반 상처 케어 관리 서비스

![image](https://github.com/user-attachments/assets/45816be1-451b-418c-ad94-b09b78287579)







[![Azure Custom Vision](https://img.shields.io/badge/Azure-Custom%20Vision-0078D4?style=flat-square&logo=microsoft-azure)](https://azure.microsoft.com/en-us/products/ai-services/ai-custom-vision/)
[![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![Gradio](https://img.shields.io/badge/Gradio-UI-FF6B35?style=flat-square)](https://gradio.app/)

## 📋 프로젝트 개요

**데일리 스킨 케어 플래너**는 AI가 사용자가 촬영한 상처 사진을 자동으로 분석하여 상처 유형을 분류하고, 맞춤 관리 팁과 주변 병원·약국 정보를 제공하는 서비스입니다.

### 🎯 주요 기능

- 🔍 **AI 상처 분석**: 화상, 멍, 외상, 여드름 등 상처 유형 자동 분류
- 💡 **맞춤형 관리 팁**: 상처별 특화된 케어 가이드 제공
- 📍 **위치 기반 서비스**: 사용자 실제 위치 반영한 병원·약국 추천
- ⚡ **빠른 진단**: 이미지 업로드 후 즉시 결과 확인

## 🏆 프로젝트 성과

### 📊 모델 성능
- **Precision**: 93.5% → 99.4% (개선)
- **Recall**: 90.3% → 99.2% (개선)
- **AP (Average Precision)**: 97.3% → 100% (개선)
- **학습 데이터**: 905장의 상처 이미지

### 🏅 프로젝트 정보
- **팀명**: Team 7조 (질럿몰게 맞지마)
- **기관**: MS AI School 6th
- **개발 기간**: 2024년

## 🛠️ 기술 스택

### AI/ML
- **Azure AI Custom Vision**: 이미지 분류 모델 학습 및 배포
- **ResNet**: 딥러닝 백본 네트워크
- **데이터 증강**: 이미지 회전, 밝기 조정, 패딩 등

### Backend & API
- **Python**: 메인 개발 언어
- **Azure Custom Vision SDK**: 모델 학습 및 예측
- **REST API**: 모델 서빙

### Frontend & UI
- **Gradio**: 웹 인터페이스 구축
- **HTML/CSS/JavaScript**: 사용자 인터페이스

### 위치 서비스
- **Kakao Map API**: 국내 병원·약국 검색
- **Google Maps API**: 지도 서비스 및 위치 정보

## 🚀 시작하기

### 필요 조건
- Python 3.8+
- Azure 구독 (Custom Vision 리소스)
- Kakao Developers API 키
- Google Maps API 키

### 설치 및 실행

1. **저장소 클론**
```bash
git clone https://github.com/your-username/daily-skin-care-planner.git
cd daily-skin-care-planner
```

2. **의존성 설치**
```bash
pip install -r requirements.txt
```

3. **환경 변수 설정**
```bash
# .env 파일 생성
AZURE_CUSTOM_VISION_ENDPOINT=your_custom_vision_endpoint
AZURE_CUSTOM_VISION_PREDICTION_KEY=your_prediction_key
AZURE_CUSTOM_VISION_PROJECT_ID=your_project_id
KAKAO_API_KEY=your_kakao_api_key
GOOGLE_MAPS_API_KEY=your_google_maps_api_key
```

4. **애플리케이션 실행**
```bash
python app.py
```

5. **브라우저에서 접속**
```
http://localhost:7860
```

## 📁 프로젝트 구조

```
daily-skin-care-planner/
├── data/
│   ├── train/                    # 학습 데이터
│   ├── test/                     # 테스트 데이터
│   └── preprocessing/            # 전처리 스크립트
├── models/
│   ├── custom_vision_model/      # Azure Custom Vision 모델
│   └── resnet_model/            # ResNet 백업 모델
├── src/
│   ├── data_preprocessing.py     # 데이터 전처리
│   ├── model_training.py         # 모델 학습
│   ├── prediction.py            # 예측 로직
│   └── location_service.py      # 위치 기반 서비스
├── web/
│   ├── app.py                   # Gradio 앱
│   ├── static/                  # 정적 파일
│   └── templates/               # HTML 템플릿
├── requirements.txt             # 의존성
├── .env.example                # 환경 변수 예시
└── README.md
```

## 🔧 주요 구현 내용

### 1. 데이터 전처리
```python
def adjust_brightness(image, value):
    """이미지 밝기 조정"""
    hsv = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)
    h, s, v = cv2.split(hsv)
    v = np.clip(v + value, 0, 255).astype(np.uint8)
    adjusted_hsv = cv2.merge([h, s, v])
    return cv2.cvtColor(adjusted_hsv, cv2.COLOR_HSV2BGR)
```

### 2. Azure Custom Vision 활용
```python
def predict_wound_type(image_path):
    """상처 유형 예측"""
    from azure.cognitiveservices.vision.customvision.prediction import CustomVisionPredictionClient
    
    predictor = CustomVisionPredictionClient(endpoint, credentials)
    with open(image_path, "rb") as image_contents:
        results = predictor.classify_image(project_id, published_name, image_contents.read())
    
    return results.predictions
```

### 3. 위치 기반 병원 검색
```python
def find_nearby_hospitals(latitude, longitude):
    """주변 병원 검색"""
    url = f"https://dapi.kakao.com/v2/local/search/keyword.json"
    params = {
        'query': '병원',
        'x': longitude,
        'y': latitude,
        'radius': 2000
    }
    response = requests.get(url, params=params, headers=headers)
    return response.json()
```

## 📈 모델 성능 개선 과정

### Before & After
| 메트릭 | 초기 모델 | 최종 모델 | 개선율 |
|--------|-----------|-----------|--------|
| Precision | 93.5% | 99.4% | +5.9% |
| Recall | 90.3% | 99.2% | +8.9% |
| AP | 97.3% | 100% | +2.7% |

### 개선 방법
1. **데이터 증강**: 회전, 밝기 조정, 크로핑을 통한 데이터 다양성 확보
2. **배경 제거**: DeepLab v3 ResNet-101 모델을 활용한 배경 노이즈 제거
3. **하이퍼파라미터 튜닝**: 학습률, 배치 크기 최적화

## 🎨 사용자 인터페이스

### 메인 화면
![메인 화면](docs/images/main_screen.png)

### 분석 결과
![분석 결과](docs/images/analysis_result.png)

### 병원 추천
![병원 추천](docs/images/hospital_recommendation.png)

## 🔮 향후 발전 방향

### 1단계: 데이터 확보 및 라벨링
- [ ] 다양한 상처 유형 데이터 수집
- [ ] 의료 전문가와 협업한 정확한 라벨링
- [ ] 데이터 증강 기법 적용
- [ ] 사용자별 데이터베이스 업데이트

### 2단계: 사용자 인터페이스 개선
- [ ] 직관적인 사용자 경험 설계
- [ ] 접근성 향상을 위한 UI 개선
- [ ] 사용자 피드백 반영 구조

### 3단계: 보안 및 개인정보보호
- [ ] 사용자 동의 프로세스 개선
- [ ] 개인 로그인 방식 구현
- [ ] 의료진과의 협업 로직 개발

### 4단계: 서비스 확장 및 협업
- [ ] 의료 기관과 협력 확대
- [ ] 상처 관리 커뮤니티 기능 추가
- [ ] 텔레메디슨 서비스 연동

## 🤝 기여하기

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 라이선스

이 프로젝트는 MIT 라이선스 하에 배포됩니다. 자세한 내용은 `LICENSE` 파일을 참조하세요.

## 👥 팀 구성

| 이름 | 역할 | 담당 업무 |
|------|------|-----------|
| 김용휘 | 팀장 | 프로젝트 관리 및 기획안 작성 |
| 김영교 | 데이터 담당 | 데이터 수집, 추체 선정 피드백, 프로젝트 질의응답 |
| 김용환 | 팀원 | 데이터 전처리 |
| 배용석 | 팀원 | ResNet 활용 모델 개발 |
| 양태윤 | 팀원 | 데이터 라벨링, Google map API 활용 |
| 이소연 | 팀원 | Kakao map API 활용, Custom Vision활용 모델 개발 |
| 추성호 | 팀원 | 웹 개발 - gradio |


---

<div align="center">

**🩹 건강한 일상을 위한 스마트 상처 케어 서비스 🩹**

[⭐ Star this repo](https://github.com/your-username/daily-skin-care-planner) | [🐛 Report Bug](https://github.com/your-username/daily-skin-care-planner/issues) | [💡 Request Feature](https://github.com/your-username/daily-skin-care-planner/issues)

</div>
