# feel_classifications_kobert
## kobert를 이용한 한국어 감정 분류 모델 및 활용방안

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
