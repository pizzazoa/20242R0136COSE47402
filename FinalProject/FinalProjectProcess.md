# 프로세스

## 1. 데이터 수집 및 탐색

- beautifulSoup이나 selenium을 이용한 데이터크롤링, github 기존 연구 사례 데이터셋 활용, open face image 데이터셋 활용
- 주의 사항)
테스트를 통해 성능을 제대로 평가하기 위해서는 이미 결과를 알고 있는 데이터셋이 필요함. 이를 위해 이미 퍼스널 컬러가 공개된 인물들의 사진을 테스트 데이터로 수집할 필요가 있음. 
따라서 퍼스널컬러가 공개된 인물 50~100명 정도의 사진을 500장 이상 구해서 훈련을 할 예정

## 2. 데이터 전처리

- 여러 논문들을 참조한 결과
    - 인물의 얼굴만 크롭할 필요가 있고
    - 조명에 따른 노이즈를 막기 위해 화이트밸런싱 등을 시도할 수 있으며
    - 데이터셋 증강을 시도할 수 있다.

## 3. 데이터 특성을 반영할 수 있는 적절한 모델과 회귀식

- 전처리가 끝난 데이터에서 어떤 특징을 추출할 것인지 결정
    - feature 목록
        - 피부 톤
        - 머리 색
        - 눈동자 색
        - 사진의 전체적인 밝기 - 추후 feature들의 가중치 조절
- 해당하는 특징들을 추출하는 모델 선택
    - 사진 밝기 추출 → 안면 크롭 → 눈, 피부, 헤어 색 추출
        - 밝기추출은 그냥 HSV 색 공간에서 RGB 평균 등을 계산하여 수행하면 됨.
        - 얼굴 인식 및 크롭
            - MTCNN, dlib의 ResNet 기반 얼굴 인식 모델, 구글의 MediaPipe Face Detection
        - 안면 특징 추출
            - 구글의 DeepLab V3, FaceNet 또는 dlib 랜드마크 모델
- 추출한 피처맵을 통해 분류할 분류기 선택
    - 4가지 feature를 바탕으로 적절히 분류하는 모델 작성
    - 특징 분석 및 퍼컬 추천(분류)
        - ResNet, EfficientNet, ImageNet, CLIP, VGG16/VGG19 등을 파인 튜닝하여 전이 학습.
            - 전이학습하는 방법 = 기존 모델에 새로운 layer를 추가하여 퍼컬 분류용으로 학습시키면 됨. GOAT!

## 4. 데이터로부터 모델에 필요한 정보 추출

- 위에서 결정한 것들로 실제 진행

## 5. 모델 최적화 (튜닝)

- 분류 성능을 위한 재검토

## 6. 모델 평가 및 SOTA 비교

- 성능 평가 (4~6은 계속 되풀이 될 수 있음)

## 7. 최종 보고서 작성

- 진행 과정을 기록해나가면서 보고서 작성

# 참고자료

- 퍼스널컬러 분류 및 메이크업 생성 발표
[https://cms.kookmin.ac.kr/dna/dna/conference.do?mode=view&articleNo=5906376&title=[찾아줄게%2C+PersonalAI+Color]+퍼스널컬러+분류+및+메이크업+생성](https://cms.kookmin.ac.kr/dna/dna/conference.do?mode=view&articleNo=5906376&title=%5B%EC%B0%BE%EC%95%84%EC%A4%84%EA%B2%8C%2C+PersonalAI+Color%5D+%ED%8D%BC%EC%8A%A4%EB%84%90%EC%BB%AC%EB%9F%AC+%EB%B6%84%EB%A5%98+%EB%B0%8F+%EB%A9%94%EC%9D%B4%ED%81%AC%EC%97%85+%EC%83%9D%EC%84%B1)
- https://universe.roboflow.com/capstonea-9fv4r/personal-color
- https://github.com/PSY222/Colorinsight
- https://medium.com/@karthikayyala/color-analysis-with-deep-learning-be9a7a8c2cd3
- 논문 및 리서치들
    
    [2022최종보고서_01_감따러가조_퍼스널컬러진단및스타일링시스템.pdf](https://prod-files-secure.s3.us-west-2.amazonaws.com/53547203-fa23-4383-883c-11311cacccb7/b8bfd187-7144-4c08-a847-d09920bd52df/2022%EC%B5%9C%EC%A2%85%EB%B3%B4%EA%B3%A0%EC%84%9C_01_%EA%B0%90%EB%94%B0%EB%9F%AC%EA%B0%80%EC%A1%B0_%ED%8D%BC%EC%8A%A4%EB%84%90%EC%BB%AC%EB%9F%AC%EC%A7%84%EB%8B%A8%EB%B0%8F%EC%8A%A4%ED%83%80%EC%9D%BC%EB%A7%81%EC%8B%9C%EC%8A%A4%ED%85%9C.pdf)
    

# 세부 목표

- 원래라면 가을 웜 뮤트, 봄 웜 라이트, 봄 웜 브라이트 등 세부적으로 8가지로 나눌 수 있지만, 간단히 봄, 여름, 가을, 겨울 4가지로만 분류하고자 함.
- 데이터셋 플랜
    - A) 퍼스널 컬러가 알려진 유명인을 계절별로 10명 정도(남 4 여 6) 총 40명을 선정. 인당 15장의 사진을 수집. 총 600장. 500은 학습/50은 검증/50은 실험
    - B) 그냥 있는 데이터셋 가지고 수행