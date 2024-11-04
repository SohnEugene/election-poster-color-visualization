# 선거 벽보, 색으로 풀어봤다!
(프로젝트 소개)  
(윅스 링크)
<br/>
<br/>
<br/>

## 팀 소개: 서울대학교 RGB(Real Gradient Breakdown)
2024-1 연합전공 정보문화학의 비주얼라이제이션 수업을 통해 구성된 팀입니다.
<br/>
- 명찬호
  - 지리학과
- 손유진
  - 자유전공학부 21학번 
  - 이메일: qeugene@snu.ac.kr
- 신승주
  - 인류학과
- 장한이
  - 건설환경공학부
- 정해인
  - 자유전공학부
<br/>
<br/>

## 1. 데이터 수집
[중앙선거관리위원회의 도서관 사이트](https://library.nec.go.kr/neweps/main.do)에 아카이빙된 선거자료 > 후보자선전물 자료를 활용했다.
<br/>
위 사이트에서는 역대 모든 선거 후보자들의 선거 벽보와 공보 pdf 파일을 열람, 다운로드할 수 있다.
<br/>
<br/>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/514957ff-bf2b-4ba3-81cc-7e6652c1b37b">
<br/>
<br/>
위 pdf 파일을 Python을 통해 스크래핑하여 구글 드라이브에 저장하였다. 
환경은 Google Colab, requests와 Beautiful Soup 패키지를 사용하였다.  
선관 종류에 따라 검색 링크가 달라지기에 각각의 링크를 가져와 선거 대수와 페이지 숫자를 포매팅하여 사용했다.  
전체 코드는 이 리포지토리의 [data_scraping.ipynb](https://github.com/SohnEugene/election-poster-color-visualization/blob/main/%E1%84%8F%E1%85%B3%E1%84%85%E1%85%A9%E1%86%AF%E1%84%85%E1%85%B5%E1%86%BC.ipynb) 파일에서 확인할 수 있다.





## 2. 데이터 전처리
### JPG 파일로의 변환
대표색 추출을 위해서는 pdf 파일을 jpg 파일로 변환해야 했다.  




## 3. 대표색 추출
## 4. 시각화
## 기타
## 참고 자료
