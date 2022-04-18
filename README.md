## Data Collection

#### 일반적인 데이터 처리 단계

- 1. 요구사항 분석
- 2. `데이터 수집`
- 3. 데이터 준비/전처리/적제
   - 전처리, 정제, 적제
   - 이상치, 결측치 처리
- 4. 데이터 분석
   - EDA(탐색적 분석)
   - 인과분석
   - ....
   - 시각적 분석
- 5. 모델 구축
   - 통계 모델(모형)
   - 머신러닝 모델
   - 딥러닝 모델
- 6. 시스템 구축/서비스 구성/레포트 => 산출물



#### 데이터 수집
- 난이도에 따라 구성
    - Lv 1
      - 데이터를 제공받는 경우.
      - 공공데이터, 공모전 데이터(활용x), 연구기관/교육기관 제공.
      - 사내데이터.(회사 내부 데이터)
    
    - Lv 2
        - open API가 존재하는 경우.
        - 인증키를 통해서, 하루에 적정량의 데이터를 질의하여 활용할수 있는 경우.
        - 데이터는 통상 json 또는 xml로 제공해줌.
        - 반정형 데이터.(데이터에 구조(스키마) 포함되어 있음)
        - 공개되어 있지는 않지만, 웹을 분석하고 http로 통신되는 부분을 체킹하여 수집 가능.(합법적인가?) -> Wireshark(프로그램) <- 서비스 구축하는 곳에서는 https + 암호화
     
    - Lv 3
        - 해당 웹페이지에서 바로 데이터를 수집.
        - Web Scarpping(웹 스크레핑)
        - request, bs4(beautiful soup)
            - requests 모듈 : 직접 주소를 입력하여 요청할 수 있음. requests 모듈을 통해 요청해서 받아오는 데이터는 웹 브라우저에서 소스보기 했을 때 나오는 코드를 의미함. 
            - 웹 브라우저는 이 코드를 분석 실행하여 다른 코드를 생성한 다음 그 생성된 코드로 눈에 보이는 화면을 구성함. 
            - 따라서 requests 모듈을 통해 받아온 데이터에는 브라우저 화면에 있는 데이터가 없을 수도 있음.
        - bs4 : 받아온 html, xml 데이터를 분석하여 수집.
        - 비정형 데이터.(날것의 데이터, 구조(스키마)가 없음)
        
    - Lv 4
        - 해당 웹페이지가 사용자와 인터렉션을 통해서(반응해서) 데이터가 노출됨.
        - 더보기, 스크롤, 로그인, 검색 등등 케이스, ajax를 사용한 사이트
        - selenium(셀레니움) + 웹드라이버(브라우저 회사별로 제공하는)
             - selenium 모듈 : 웹 브라우저를 조작하여 원하는 페이지로 이동한 다음에 데이터를 받아옴. 크롬 개발자 도구의 elements 탭에 있는 데이터를 받아 올 수 있음.
        - 비정형 데이터.(날것의 데이터, 구조(스키마)가 없음)
        - 매크로(이 기술을 좋지 못한 목적으로 사용하는 자동화 프로그램)
        - 고급기술 => proxy를 중계하여 작업을 요청한 클라이언트를 숨기는 기술.
        
    - 자동화
        - os 레벨에서 자동으로 데이터를 수집하게 하는 활동을 작성 / 운용 
        - lv3/lv4 걑은 경우는 단시간에 빠른 접속을 지속적으로 시도하면
            - 디도스로 간주할수 있기 때문에, 적절한 시간 조절.
            - 고급(접속한 유저의 ip를 우회하여(플락시 서버 활용) 처리.


#### 데이터 수집 전략

- 데이터 수집 주요 모듈
    - requests : 직접 주소를 입력하여 요청이 가능. requests 모듈을 통해 요청해서 받아오는 데이터는 웹 브라우저에서 소스보기 했을 때 나오는 코드를 의미함. 웹 브라우저는 이 코드를 분석 실행하여 다른 코들 생성한 다음 그 생성된 코드로 눈에 보이는 화면을 구성한다. 따라서 requests 모듈을 통해 받아온 데이터에는 브라우저 화면에 있는 데이터가 없을 수도 있음.
    - selenium : 웹 브라우저를 조작하여 원하는 페이지로 이동한 다음에 데이터를 가져옴. 크롬 개발자 도구의 elements 탭에 있는 데이터를 받아 올 수 있음.
    - bs4 : 받아온 html, xml 데이터를 분석하여 수집.
    - json : 받아온 json 데이터를 분석하여 수집.
    - feedparser : 받아온 rss 데이터를 분석하여 수집.

- 주소 분석이 가능하다면 requests 모듈로 요청할 것.
- 주소 분석이 어렵다고 판단된다면 selenium을 권장.
    - requests는 속도가 빠른 반면 selenium은 상대적으로 속도가 느림.
- 소스보기 했을 때 데이터가 존재한다면 : requests 모듈로 요청하고 bs4로 수집.
- 소스보기 했을 때는 데이터가 없지만 elements 탭에 데이터가 존재할 경우 : ajax, 웹 소켓
- ajax라면 주소를 분석하여 직접 요청해서 수집.
- 웹 소켓이라면 selenium을 사용.
- 소스보기 했을 때 데이터가 없고 elements 탭에도 데이터가 없는 경우 : iframe
- iframe이 있는지 확인해 보고 iframe이 있다면 iframe에 셋팅된 주소로 요청해서 수집.

- id 속성 : HTML에서 사용하는 모든 태그에 사용할 수 있는 속성. 이 속성은 하나의 HTML 문서 안에서 같은 id를 가지고 있는 태그는 없음. id 는 웹 개발자가 특정 태그 하나를 지칭하기 위해 사용하는 태그
- class 속성 : css는 html 태그를 꾸미는 용도로 사용.
- nth-child : 같은 태그가 동시에 여러개 나열되어 있을 경우 그 순서를 구분하기 위해 사용하는 필터(1 부터 시작)
- 여러 항목들을 가져오고자 할 때는 그 항목들을 둘러싸고 있는 태그부터 가져올 것.

- 주소 분석.
- 한 페이지의 데이터를 수집.
- 다음 페이지로 넘어갈 수 있는 방법을 고려.
- 마지막 페이지를 어떻게 구분할 수 있을지를 고려.
- while 문을 말들어 마지막 페이지까지 갈 수 있도록 구성.
- 반드시 종료가 되게끔 할 것.
- 매 반복마다 반드시 딜레이를 줄 것.(권장 3초)
