# 선거 벽보, 색으로 풀어봤다!
<br/>
선거철 길거리에서 쉽게 찾아볼 수 있는 선거 벽보. 당신은 선거 벽보를 유심히 살펴본 적이 있으신가요? 벽보는 후보가 유권자들에게 자신을 알리는 가장 강력한 수단 중 하나이기에 그 안의 요소들에는 모두 각각의 의미가 있습니다. 특히 벽보를 보는 유권자가 가장 먼저 지각하는 요소 중 하나인 색에는 단순한 시각 정보 그 이상의 이야기가 담겨있습니다.
<br/>
<br/>
저희 RGB팀은 2000년부터 2022년까지 진행된 6번의 국회의원선거와 6번의 전국동시지방선거의 벽보 5,306장의 색을 분석하였습니다. 각 벽보의 대표색을 추출한 뒤 선거별, 정당별로 모아보니 벽보의 색에 선거 판세, 더 나아가 정치적 사건과 흐름이 반영되어 있다는 것을 확인할 수 있었습니다. 저희는 이 내용을 [“선거 벽보, 색으로 풀어봤다!"]([https://electionpostercolo.wixsite.com/my-site)에 총 4가지의 인사이트로 정리하였습니다.
<br/>
<br/>
이 깃헙 리포지토리는 프로젝트에 사용된 파이썬 파일을 공유하고 데이터 수집부터 시각화까지 전 과정을 상세히 설명하기 위한 용도로 제작되었습니다.
<br/>
<br/>
<br/>

## 팀 소개: 서울대학교 RGB(Real Gradient Breakdown)
2024-1 연합전공 정보문화학의 비주얼라이제이션 수업을 통해 구성된 팀입니다.
<br/>
- 명찬호
  - 지리학과 22학번
  - 이메일: timmy1383@gmail.com
- 손유진
  - 자유전공학부 21학번 
  - 이메일: qeugene@snu.ac.kr
- 신승주
  - 인류학과 21학번
  - 이메일: tmdwn02@snu.ac.kr
- 장한이
  - 건설환경공학부 18학번
  - 이메일: hanyi0201@snu.ac.kr
- 정해인
  - 자유전공학부 21학번
  - 이메일: jhi710@kakao.com
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
선거 종류에 따라 검색 링크가 달라지기에 각각의 링크를 가져와 선거 대수와 페이지 숫자를 포매팅하여 사용했다. 
전체 코드는 이 리포지토리의 [data_scraping.ipynb](https://github.com/SohnEugene/election-poster-color-visualization/blob/main/%E1%84%8F%E1%85%B3%E1%84%85%E1%85%A9%E1%86%AF%E1%84%85%E1%85%B5%E1%86%BC.ipynb) 파일에서 확인할 수 있다.
<br/>
<br/>
원래 계획은 한창 프로젝트 진행 당시 치러진 22대 총선까지를 프로젝트에 포함시키는 것이었다.
선거 기간에는 선관위 홈페이지에 정책공약 마당에서 모든 후보의 공보물을 확인할 수 있었기에 벽보와 상당히 유사한 공보물 표지를 사용할 계획이었다.
다음은 선관위 도서관 사이트에서 가져온 21대 총선 서울 관악구 을 선거구에 출마하여 당선된 더불어민주당 정태호 후보자의 공보물 첫 페이지(좌)와 벽보(우)의 이미지이다. 유사성을 확인할 수 있다.

<img width="300" alt="image" src="https://github.com/user-attachments/assets/c6234f1c-4dcf-44e1-ad71-1d40afdbbca7">
<img width="300" alt="image" src="https://github.com/user-attachments/assets/30a09aee-fac9-409e-9a9f-eddb54b1cdd4">

<br/>
<br/>
그러나 선거 다음날 바로 선관위 정책공약마당에서 낙선한 후보자들의 공보물이 내려갔기에 계획이 무산되었다.
선관위에 문의한 결과, 위 공보물들은 개인정보 보호를 위해 내려갔으며 선관위 도서관 홈페이지에 아카이빙되기까지는 2-3개월이 소요된다는 답변을 받았으나, 
같은 해 10월까지 선관위 도서관 사이트에 22대 총선 후보자선전물이 게시되지 않았기에 어쩔 수 없이 선관위 도서관 사이트에서 접근 가능한 2022년 진행된 8회 지선까지의 데이터를 가지고 프로젝트를 진행하였다.
<br/>
<br/>
<br/>


## 2. 데이터 전처리
### JPG 파일로의 변환
벽보 이미지에서 대표색을 추출하기 위해 pdf 파일을 jpg 파일로 변환했다. PIL, pdf2image 패키지를 사용했으며 코드는 [pdf_to_jpg.ipynb]()에서 확인할 수 있다.
<br/>
<br/>
### 이미지 리사이즈
다음에 나올 인물 마스킹을 하는 과정에서, 시간이 지나치게 많이 소요되며 작은 사이즈의 이미지로 테스트했을 때보다 결과가 안 좋게 나오는 것을 확인했다. pdf 파일에서 바로 변환된 jpg 파일의 경우 사이즈가 대략 3000x4000 이였기에 사이즈가 1MB가 넘는 데이터도 다수 있었다. 따라서 이 문제를 해결하기 위해 width = 1000으로 통일하여 리사이즈, 400KB 정도의 파일을 분석에 활용하였다.
<br/>
<br/>
### 인물 마스킹
프로젝트 계획 단계에서 가장 큰 우려 중 하나는 대표색이 제대로 추출되지 않을 가능성이였다.
아무런 마스킹 처리가 되지 않은 이미지에서 대표색을 추출할 경우, 얼굴에서 추출된 살구색이나 머리와 옷에서 추출된 검정색이 대표색으로 추출될 것이라고 예상했다.
우리가 알고 싶었던 것은 모든 벽보에 공통적으로 들어가 있는 사람의 얼굴 이외의 다른 요소에 어떤 색이 얼마나 활용되었는지였기에 후보자를 마스킹처리하여 대표색 추출 시 포함되지 않도록 하였다.
위와 같은 과정을 거치면 후보자가 입고 있는 옷이나 넥타이의 색깔이 누락되기에 필요한 색 정보가 소실된다는 우려가 있었지만, 이에 해당하는 사례가 비교적 적었기에 일괄적으로 마스킹 처리를 하기로 결정하였다.

인물 인식을 위해서는 Semantic Segmentation(의미적 분할)을 사용하였다. 
Semantic Segmentation은 입력된 이미지에 각 픽셀에 특정한 클래스 라벨을 할당하는 것을 목적으로 하는 컴퓨터 비전의 한 분야로 자율주행, 영상 의학 등 다양한 분야에 널리 활용되고 있다.
아래 사진[(출처)](chrome-extension://efaidnbmnnnibpcajpcglclefindmkaj/https://cs231n.stanford.edu/slides/2017/cs231n_2017_lecture11.pdf)은 Semantic Segmentation이 적용된 예시이다. 
이미지가 그 내용(semantic)에 따라 분할(segmentation)된 것을 확인할 수 있다.
<br/>
<br/>
<img width="377" alt="image" src="https://github.com/user-attachments/assets/5f8d2423-2284-48ef-bfe7-f88e87b8950e">
<br/>
<br/>
[Deeplab](https://github.com/tensorflow/models/tree/master/research/deeplab)은 구글이 2015년 공개한 Semantic Segmentation 모델의 이름이다. 
Semantic Segmentation 모델은 일반적으로 downsampling-upsampling과정을 거치는데, Deeplab은 그 과정에 Astrous Convolution(확장된 합성곱)을 사용하여 해상도를 유지하는 것을 특징으로 하며 상당히 좋은 성능을 보인다. 
이 프로젝트에서는 Deeplab 모델을 사용하여 벽보 이미지를 여러 클래스로 나누고, 인물 클래스에 해당하는 픽셀만을 마스킹처리하여 사용하였다. 
디테일한 코드 구현은 Deeplab에서 제공하는 [데모](https://colab.research.google.com/github/tensorflow/models/blob/master/research/deeplab/deeplab_demo.ipynb) 코랩 파일과 [링크](https://meissa.tistory.com/37)를 참조했다.
<br/>
<br/>
다음은 올해 4월 치러진 22대 총선, 관악구 갑, 을 선거구에 출마한 더불어민주당과 국민의힘 4명의 후보의 공보물 표지 이미지에 테스트로 인물 마스킹을 적용한 결과이다. (초상권 문제로 인해 블러 처리)
<br/>
<br/>
![Frame 1 (1)](https://github.com/user-attachments/assets/f947fd7c-1c9f-4ae4-ae22-bfe6f69c7b1b)
<br/>
<br/>
인물과 텍스트 영역이 깔끔하게 구분되지 않은 이미지의 경우 Deeplab 모델의 인식 결과가 충분하지 않았다. 
따라서 텍스트 영역을 미리 마스킹 처리하여 semantic segmentation에서 제외시키는 방법을 시도해보았다. 
여러 번 시도했을 떄 복잡한 얼굴 영역도 텍스트로 잡는 경우가 대다수 있었기에 얼굴 인식하여 그 영역을 제외한 후 텍스트 인식 알고리즘을 적용하였다. 
텍스트 영역 인식을 위해서는 openCV에서 제공하는 [convexHull](https://prlabhotelshoe.tistory.com/42) 알고리즘을, 얼굴 영역 인식을 위해서는 마찬가지로 openCV에서 제공하는 [Haar-Cascade face detection](https://bkshin.tistory.com/entry/%EC%BB%B4%ED%93%A8%ED%84%B0-%EB%B9%84%EC%A0%84-1-%ED%95%98%EB%A5%B4-%EC%BA%90%EC%8A%A4%EC%BC%80%EC%9D%B4%EB%93%9C-%EC%96%BC%EA%B5%B4-%EA%B2%80%EC%B6%9C-Haar-Cascade-Face-Detection) 알고리즘을 사용하였다.

위 파이프라인을 적용한 결과는 다음과 같다. (초상권 문제로 인해 블러 처리)
<br/>
<br/>
![Frame 2 (1)](https://github.com/user-attachments/assets/424350ba-8d2a-4950-b31e-333fc0a909e8)
<br/>
<br/>

그러나 위 파이프라인은 이미지에 따라서 가장 적절한 값이 달라지는 parameter가 너무 많은 탓에 서너개의 테스트 데이터가 아닌 수백 개의 실제 데이터셋에 적용되었을 때 결과가 만족스럽지 않았다. 
따라서 다양한 사이즈의 텍스트가 빽빽하게 들어가 있어 인식이 어려운 이미지 하단 일정 비율을 인물 추출에서 제외하는 방식을 시도하였다. 
벽보의 구성에 따라서 인물 마스킹이 얼마나 잘 되었는지의 차이는 있었지만, 전체적으로 원하던 결과가 나왔기에 이렇게 만들어진 파이프라인을 사용하였다. 
코드는 [masking_pipeline.ipynb]()에서 확인 가능하다. 
또 대략 2-3%의 비율로 얼굴 영역 인식이 실패하거나 추출이 실패하는 데이터가 존재, 이에 대해서는 팀원들이 직접 마스킹 작업을 수행하였다. 
다음은 완성된 마스킹 알고리즘을 적용한 전처리된 벽보 이미지의 예시이다.
<br/>
<br/>
![Frame 3](https://github.com/user-attachments/assets/e4d9651b-680d-4d76-9105-611301cf22ea)
<br/>
<br/>
<br/>


## 3. 대표색 추출

전처리된 이미지에서 K-means clustering을 사용하여 대표색을 추출하였다. 
K-means clustering은 평균을 사용하여 입력된 데이터를 K개의 군집으로 묶는 알고리즘으로, [scikit-learn](https://github.com/scikit-learn/scikit-learn)에서 제공하는 KMeans 함수를 사용하였다. 
인물 마스킹에 사용된 초록색(0, 255, 0)을 제외하도록 설정했으며, 3개의 군집을 형성한 다음 각 군집의 평균 값과 군집에 속하는 픽셀의 갯수를 세어 전체 픽셀 대비 비율을 계산하였다. 

최종적으로 데이터를 스크래핑할 때 함께 가져온 (후보자 명 / 정당 명 / 출마 지역 / 당선 여부) 와 선관위가 공공데이터포털을 통해 제공하는 [중앙선거관리위원회 투·개표정보](https://www.data.go.kr/data/15000900/openapi.do)에서 가져온 각 후보별 득표율, 그리고 벽보 이미지에서 추출한 3가지 색과 각각의 비율이 저장된 데이터 프레임을 만들어서 시각화와 분석에 사용하였다. 이 과정에서 선관위 도서관 아카이빙에 누락된 자료가 존재함을 확인되었지만, 이 데이터들을 확보할 다른 방법을 찾을 수 없었기에 그대로 분석을 진행하였다. 이 과정을 다룬 전체 코드는 [color_extraction.ipynb](color_extraction.ipynb)에서 확인할 수 있다.

<img width="1680" alt="image" src="https://github.com/user-attachments/assets/88669c70-3bb5-45dd-a3fb-1d4d659efdda">
<img width="1680" alt="image" src="https://github.com/user-attachments/assets/66b22a65-1df5-4c91-97d6-9df5ca755dab">

<br/>
<br/>
<br/>


## 4. 시각화

프로젝트를 시작할 때 우리는 후보자들은 해당 지역구에서 자신의 소속 정당이 얼마나 유리한지를 따져서 벽보에 정당색을 얼마나 사용할 지를 결정할 것이며 일반적으로 선거 전의 판세와 선거 결과가 일치할 것이라는 가정 하에 "득표율이 높은 후보일수록 벽보에 정당색을 더 많이 사용할 것이다"라는 가설을 세웠다. 
이를 확인하기 위해 각 후보의 대표색(가장 비율이 높은 색)을 우리가 계산한 득표율 인덱스(승리한 후보자의 경우 2위 후보자, 패배한 후보자의 경우 1위 후보자와의 득표율 차)을 기준으로 대표색을 정렬한 스펙트럼 이미지를 만들었다. 
다음은 21대 총선 더불어민주당과 미래통합당의 득표율 순 정렬 스펙트럼이다.
<br/>
<br/>
![21대 총선 더불어민주당(득표율 순)](https://github.com/user-attachments/assets/fd45ab51-49ab-4f54-b0af-50b087a1450e)
![21대 총선 미래통합당(득표율 순)](https://github.com/user-attachments/assets/a7a9b6d8-c251-41ce-b23f-2b8c602cf30a)
<br/>
<br/>
예상과 달리 득표율과 대표색 간의 유의미한 관계를 찾을 수 없었고 이는 다른 선거에서도 마찬가지였다. 
이외에도 대표색 대신 3가지 색의 평균, 가중평균 등을 사용해봤지만 이 역시 유의미한 관계가 나타나지 않았다. 
이에 우리는 후보 개인의 유리함이 아니라 정당의 유리함이 벽보의 정당색 반영과 더욱 관련이 있을 것이라고 생각해 특정 정당의 특정 선거 결과가 아니라 선거 간, 정당 간의 정당색 사용 정도를 비교하는 쪽으로 프로젝트 방향을 수정하였다. 
벽보의 대표색이 대개 하얀색 / 정당색 / 검은색 중 한 가지의 계열과 인접하게 나타나는 바, grey scale값(Gray scale = Red * 0.299 + Green * 0.587  + Blue * 0.114)을 계산하여 이를 기준으로 스펙트럼을 정렬해 보았다. 
다음은 21대 총선 더불어민주당과 미래통합당의 명도 순 정렬 스펙트럼이다.
<br/>
<br/>
![21대 총선 더불어민주당(색상 명암 순)](https://github.com/user-attachments/assets/4ce82b4e-8371-4e62-b365-dc5d0fe3a6f6)
![21대 총선 미래통합당(색상 명암 순)](https://github.com/user-attachments/assets/40249bb5-f23b-465e-9d40-2a6b320e8b5b)
<br/>
<br/>
이렇게 만들어진 스펙트럼을 사용하여 벽보에서의 정당색 사용과 선거 판세의 관계, 벽보에서의 정당색 사용 정도의 변화와 경향을 분석을 진행하였다. 
분석한 내용은 기사 ["선거 벽보, 색으로 풀어냈다!"](https://electionpostercolo.wixsite.com/my-site)에서, 전체 코드는 [spectrum.ipynb](spectrum.ipynb)에서 확인할 수 있다.
<br/>
<br/>
<br/>


## 5. 대표색 = 정당색의 판단

정당색을 선거 벽보의 대표색으로 활용하였는지 판단할 때 선거 벽보의 대표색이 정당색과 가까운 색이면, 선거 벽보에 정당색을 사용했다고 판단했다. 
이때 정당색과 가까운 색이란 정당색 RGB값을 좌표로 볼 때, 정당색 좌표와의 유클리드 거리가 75 이하인 색으로 정의했다. 
> 정당색 rgb (r1, g1, b1) 대표색 rgb (r2, g2, b2) 일 때, 루트{(r1-r2)^2+(g1-g2)^2+(b1-b2)^2} <= 75 
<br/>
<br/>
<img width="400" alt="image" src="https://github.com/user-attachments/assets/deed96e2-03be-4b49-b8e5-28bbd33c023b">
<img width="400" alt="image" src="https://github.com/user-attachments/assets/8c91e576-1d4d-4006-8bd8-f8d7aa522265">
<br/>
<br/>

각 선거에서 정당색을 대표색으로 사용한 벽보를 센 후, 각 선거 전체 벽보 수로 나누어 정당색을 벽보 대표색으로 사용한 비율을 계산하였다. 
코드는 [color_distance.ipynb](color_distance.ipynb)에서 확인할 수 있다.
<br/>

