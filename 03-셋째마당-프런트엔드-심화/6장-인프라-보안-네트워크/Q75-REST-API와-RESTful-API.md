# Q75. REST API와 RESTful API를 설명할 수 있나요?

### REST와 REST API

- **REST**(REpresentational State Transfer): **분산 하이퍼미디어 시스템을 위한 아키텍처 스타일**. 즉 **설계 철학**이다.
    - 웹을 잘 동작하게 만드는 설계 원칙을 정리한 것.
- **REST API**: 이 **REST 설계 원칙을 따르는 API.**
    - **클라이언트-서버 구조**: 클라이언트와 서버의 **역할을 명확히 분리**
    - **무상태성(statelessness)**: **서버는 클라이언트의 상태를 저장하지 않음**. 각 요청은 독립적으로 처리되어야 함.
    - **캐시 가능성(cacheable)**: 클라이언트는 **서버 응답을 캐싱하여 재사용**할 수 있어야 함.
    - **균일한 인터페이스(uniform interface)**: **리소스는 URI로 식별**되고 **표준 HTTP 메서드**를 통해 **상태를 변경**함.
        - (참고) URL / URI / URN 차이점
            
            !image.png
            
            - URI: 자원의 식별자. 인터넷에 있는 자원을 어디에 있는지 자원 자체를 식별하는 방법
            - URL: 위치(Location)
            - URN: 이름(Name)
    - REST API는 자원(resource)을 중심으로 설계된다.
        - 학생(students)이라는 자원이 있다면 다음과 같이 HTTP 메서드와 결합하여 API를 구성한다.
        - `GET /students`: 모든 학생 목록을 조회한다(Read).
        - `POST /students`: 새로운 학생을 생성한다(Create).
        - `GET /students/123`: ID가 123인 특정 학생의 정보를 조회한다(Read).
        - `PUT /students/123`: ID가 123인 학생의 정보를 전체 수정한다(Update).
        - `DELETE /students/123`: ID가 123인 학생을 삭제한다(Delete).
    - 무엇을 할 것인지 → HTTP 메서드가 담당
    - **어떤 대상인지 → URI가 자원(명사)으로 표현**
    

### RESTful API와 HATEOAS

- **RESTful**: **REST의 원칙을 정말 충실하게** 잘 지켰다는 의미를 담은 “형용사”.
- **RESTful API**: **REST의 설계 원칙을 완벽하게 준수**하여 구현된 “API”.
- **HATEOAS(Hypermedia As The Engine Of Application State)**: 응답에 다음에 할 수 있는 행위의 링크를 포함해 보내 주는 것.
    
    → 균일한 인터페이스 원칙의 하위 요소이다.
    
- **진정한 RESTful API**
    
    > `GET /students/123`
    > 
    - 일반적인 REST API 응답
        
        ```json
        {
        	"id": 123,
        	"name": "홍길동",
        	"major": "컴퓨터공학"
        }
        ```
        
        - 데이터만 담고 있다.
        - ‘홍길동’ 학생의 수강 과목을 보려면 클라이언트 개발자가 `/students/123/courses`라는 **URI를 미리 알고 있어야 한다.**
    - HATEOAS를 준수한 RESTful API 응답
        
        ```json
        {
        	"id": 123,
        	"name": "홍길동",
        	"major": "컴퓨터공학",
        	"_links": {
        		"self": { "href": "/students/123" },
        		"courses": { "href": "/students/123/courses" },
        		"update": { "href": "/students/123", "method": "PUT" }
        	}
        }
        ```
        
        - 데이터 + **다음에 가능한 행위에 대한 링크를 함께 제공**

### 결론

- REST = 웹이 잘 동작하도록 돕는 설계 **철학**
- RESTful = 그 철학의 이상적인 **구현체**

### 모의 면접

1. REST API의 핵심 원칙 4가지가 무엇인지 설명할 수 있나요?
    
    > 첫째, 클라이언트-서버 분리로, 클라이언트와 서버의 역할이 명확히 분리되어야 합니다.
    둘째, 무상태성으로 서버는 클라이언트의 상태를 저장하지 않고 **각 요청이 필요한 정보를 모두 담아야 합니다.**
    셋째, 캐싱 가능성으로, 클라이언트는 서버의 응답을 캐싱하여 재사용할 수 있어야 합니다.
    넷째, 균일한 인터페이스 설계로 리소스는 URI로 식별되고 표준 HTTP 메서드로 상태를 변경해야 합니다.
    > 
2. 일반적인 REST API와 RESTful API의 차이를 HATEOAS 관점에서 설명할 수 있나요?
    
    > 일반적인 REST API는 데이터만 응답하기 때문에, **클라이언트가 다음 요청의 URI를 미리 알고 코드에 고정해둬야 합니다**. 
    반면 HATEOAS를 적용하면 응답에 다음에 가능한 행위에 대한 링크를 함께 제공하여 **클라이언트가 그 링크를 따라가며 동적으로 다음 행동을 파악**할 수 있게 합니다.
    > 

### 추가 공부

- 그렇다면 실무에서는 HATEOAS를 사용하는가?
    - 오버헤드 및 **비용 대비 효용**이 낮음.
        - 대부분의 프론트엔드(React, Vue, iOS, Android 등)나 모바일 앱은 **API 엔드포인트 구조를 사전에 합의(Swagger/Open API 문서 등)하고 정적으로 코딩**하는 편이 훨씬 직관적이고 빠름.
    - **클라이언트 구현 복잡도 증가**
        - 클라이언트가 **매번 응답에 포함된 링크(`rel` 등)를 해석해서 다음 요청을 동적으로 구성**해야 하므로, 단순 JSON을 파싱하는 것보다 로직이 복잡해짐.
    - 실무에서는 보통 **REST 성숙도 모델(RMM) Level 2** 선상에서 타협함.
        - Level 0: HTTP를 단순 전송 수단으로만 사용
        - Level 1: 자원(Resource) 도입
        - Level 2: HTTP 메서드(Verbs) 사용
        - Level 3: 하이퍼미디어 컨트롤(HATEOAS)