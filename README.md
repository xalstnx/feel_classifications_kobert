# feel_classifications_kobert
## kobert를 이용한 한국어 감정 분류 모델 및 활용방안

---

#### 기본코드는 네이버 영화 리뷰 분류를 참조함
- https://github.com/e9t/nsmc
#### batch_size=16인 fine-tuning하여 저장한 model
- https://drive.google.com/file/d/1dcbUb0E9nEnHrVKG7ZgKokgwxVfNS4db/view?usp=sharing

---

## colab 링크
### https://colab.research.google.com/drive/1jWubP_Y5fZDTRuxmE9q24K_fKDFJQerq?usp=sharing

---

### 사용한 데이터셋
#### AI Hub의 한국어 감정 정보가 포함된 단발성 대화 데이터셋을 사용
- 7개 감정(기쁨, 슬픔, 놀람, 분노, 공포, 혐오, 중립) 레이블링 수행
- 총 데이터 개수: 38,594 문장
- https://aihub.or.kr/opendata/keti-data/recognition-laguage/KETI-02-009

---

### 구글 드라이브 연결
![image](https://user-images.githubusercontent.com/22045179/124860165-291d6400-dfec-11eb-9031-00ffb7320b87.png)

위 코드를 통해 자신의 구글 드라이브와 연결하여 구글 드라이브 내의 파일을 사용

![image](https://user-images.githubusercontent.com/22045179/124860302-61bd3d80-dfec-11eb-965f-73f99b6e1634.png)

위와 같은 경로의 파일 사용

![image](https://user-images.githubusercontent.com/22045179/124860371-7f8aa280-dfec-11eb-96f3-3f878fb1cffd.png)


---

### 감정 라벨링(문자 -> 숫자)
#### {0: '공포', 1: '놀람', 2: '분노', 3: '슬픔', 4: '중립', 5: '행복', 6: '혐오'}
LabelEncoder를 사용하여 감정 라벨링

![image](https://user-images.githubusercontent.com/22045179/124860405-8f09eb80-dfec-11eb-9b58-dc52e3a46b6f.png)

---

### train/val 분리
![image](https://user-images.githubusercontent.com/22045179/124861131-e78db880-dfed-11eb-9df9-f605f8857c07.png)

- train 80%
- val 20%

---

### 두가지 테스트 방식에 따른 데이터변환
![image](https://user-images.githubusercontent.com/22045179/124861251-2885cd00-dfee-11eb-83bd-a79d8330dbf7.png)

---

### 파라미터 세팅
![image](https://user-images.githubusercontent.com/22045179/124861292-3b000680-dfee-11eb-96ed-d09218ffcc7d.png)

- 기본 batch_size는 64이지만 32일때와 16일때 결과를 비교해보면 batch_size가 16일때 loss가 더 낮고, 정확도가 높은 결과가 나옴

#### train/ test(val), batch_size 32/16 별 loss, 정확도 비교
###### loss 비교
![image](https://user-images.githubusercontent.com/22045179/124865132-18252080-dff5-11eb-819d-f9fcf2bdf4fc.png)

###### train acc 비교
![image](https://user-images.githubusercontent.com/22045179/124865216-35f28580-dff5-11eb-9d95-4d8634fbc089.png)

###### test(val) acc 비교
![image](https://user-images.githubusercontent.com/22045179/124865394-89fd6a00-dff5-11eb-92d7-77e785abbf9e.png)

경향성에 대한 내용은 결론에 기술

---

### pre-train된 kobert모델에 새로운 층을 추가하여 감정 분류 fine-tuning
![image](https://user-images.githubusercontent.com/22045179/124865598-eb253d80-dff5-11eb-9548-a3967e8b16c2.png)

- train dataset의 감정분류가 7가지로 되어 있어 num_classes=7로 변경

---

### dropout
![image](https://user-images.githubusercontent.com/22045179/124865775-46efc680-dff6-11eb-88ad-662caf5a20f4.png)

- dropout값을 0.5로 설정

---

### optimizer
![image](https://user-images.githubusercontent.com/22045179/124865966-97672400-dff6-11eb-910e-4004fe80b48a.png)

- 기존에 설정되어 있는 Adam으로 설정
- SGD로 변경하여 train해봤으나 Adam보다 안좋은 결과가 도출

---

### 모델 초기화 및 불러오기
![image](https://user-images.githubusercontent.com/22045179/124866174-f036bc80-dff6-11eb-9e79-93025f3e34c4.png)

- 구글 드라이브의 /Colab Notebooks/nlp/weights/ 에 저장된 모델 load

---

### csv로 저장된 테스트파일로 테스팅
![image](https://user-images.githubusercontent.com/22045179/124866266-1a887a00-dff7-11eb-9943-53e2a7e5d213.png)

#### 결과
![image](https://user-images.githubusercontent.com/22045179/124866357-41df4700-dff7-11eb-9527-6ded74405894.png)
![image](https://user-images.githubusercontent.com/22045179/124866379-4c99dc00-dff7-11eb-97d9-0db66ae5a9c6.png)

- 마지막의 test acc는 test set의 예측값과 label값이 같을 경우 증가함

---

### 실시간 입력으로 테스팅
![image](https://user-images.githubusercontent.com/22045179/124866582-9b477600-dff7-11eb-94a9-68f87a77cfcd.png)
![image](https://user-images.githubusercontent.com/22045179/124866605-a7cbce80-dff7-11eb-8f04-0ccd4c39885d.png)
![image](https://user-images.githubusercontent.com/22045179/124866632-b1edcd00-dff7-11eb-98a8-c404c627573e.png)

- 가장 높은 값을 출력하고, 그 다음으로 높은 값을 출력
- 두번째로 높은 값이 0보다 작으면 출력하지 않음 

---

## 결론
train - epoch가 진행되면서 loss가 점점 떨어지고, 정확도가 이상적으로 높아짐
test(val) - epoch가 진행되면서 loss가 떨어지다가 다시 증가하고, 정확도는 0.5정도에서 큰 변동이 없음

### train은 잘되었지만 test(val)에서 loss가 증가하고, 정확도가 낮은 원인 분석
1. overfitting
2. 학습데이터 라벨링의 문제
3. 학습데이터 부족

##### 1. overfitting
- 오버피팅을 해결하기 위해 dropout, lr, optimizer를 변경
- dropout은 이미 0.5로 설정되어 있어, 일반적으로 0.5보다 높게 설정하면 좋지 않은 결과가 나옴
- learning rate(Adam)는 기존에 설정된 5e-5와 2e-5로 설정하여 비교하였더니 5e-5가 더 좋은 결과를 도출
- optimizer를 Adam에서 SGD(lr=0.01)로 변경하여 train하였더니 Adam이 더 좋은 결과를 도출

##### 2. 학습데이터 라벨링의 문제
- 학습데이터셋(AIHUB의 한국어 감정 정보가 포함된 단발성 대화 데이터셋)의 labeling이 정확도가 낮음

##### 3. 학습데이터 부족
- 학습데이터셋의 데이터수가 38,594문장으로 fine-tuning을 하기에 부족한 양은 아니라고 판단됨

### test(val)에서의 낮은 정확도와 높은 loss는 학습데이터 라벨링의 문제

---

### 테스팅
#### csv로 저장된 테스트파일로 테스팅에서의 정확도 분석
- 직접 만든 14개의 테스트데이터로 테스팅

![image](https://user-images.githubusercontent.com/22045179/124868888-69381300-dffb-11eb-9cd4-49da5fc8d84b.png)

- 테스트 정확도 = 0.6428571428571429
- 테스트 결과 대략 0.643정도의 정확도가 도출되었는데, 두번째로 높은 감정까지 포함하게 된다면 0.8571428571428571의 정확도가 도출됨
- 테스팅결과 test(val)에서 도출된 정확도 0.5보다는 더 높은 정확도가 나오는 것을 볼 수 있음

---

## 활용방안
- 사용자가 입력한 대화내용을 실시간으로 딥러닝 모델로 분석하여 나온 감정에 맞는 이모티콘을 추천해주는 시스템
- 현재 Line의 이모티콘(스티커) 추천과 갤럭시키보드 스티커 추천방식은 전체 문장의 감정이 아닌 최근 입력 단어에 국한되어 있음
- 사용자의 입력 문장을 분석하여 추천해주기 때문에 더 문장내용에 적합한 이모티콘을 추천해 줄 수 있음
