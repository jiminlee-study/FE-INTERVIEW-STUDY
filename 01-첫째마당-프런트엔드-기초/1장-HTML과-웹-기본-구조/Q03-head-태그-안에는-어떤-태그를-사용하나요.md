# Q03. head 태그 안에는 어떤 태그를 사용하나요?

### `<head>` 태그: 웹 사이트 설명서

- 웹 페이지는 크게 두 부분으로 나뉜다.
    - `<body>`: 사용자에게 직접 보이는 부분
    - `<head>`: 화면에 보이지 않는 곳에서 웹사이트를 제어.
- `<head>`: 웹 브라우저에 문서의 특징, 필요한 파일, 해석 기준을 알려줌.
    
    → 웹 사이트의 정체성을 정의하는 **메타데이터**를 담는 공간.
    
- `<head>` 태그 안에 자주 사용하는 주요 태그들
    - `<title>`
    - `<meta>`
    - `<link>`
    - `<style>`
    - `<script>`

### `<title>` 태그: 웹 사이트의 이름표

- `<title>`: 웹 브라우저의 탭이나 즐겨찾기에 표시되는 **웹 사이트의 제목을 정의**.
    
    ```html
    <title>나의 첫 웹 페이지</title>
    ```
    

### `<meta>` 태그: 웹 사이트의 상세 정보

- `<meta>`: 웹 사이트의 다양한 정보를 담을 수 있는 만능 태그.
    - 대표적인 속성으로 viewport, description, og 등이 있음.
- **문자 인코딩**: ‘UTF-8’로 지정해야 한글이나 여러 나라 언어가 깨지지 않고 표시됨.
    
    ```html
    <meta charset="UTF-8">
    ```
    
- **뷰포트 설정**: 스마트폰, 태블릿 등 다양한 기기에서 웹 사이트의 너비와 배율을 어떻게 표시할지 결정.
    
    ```html
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    ```
    
- **웹 사이트 요약:** 구글 검색 결과에서 제목 아래에 표시되는 문구가 description.
    
    ```html
    <meta name="description" content="페이지 설명">
    ```
    
- **작성자 정보**: 페이지의 작성자 정보
    
    ```html
    <meta name="author" content="작성자">
    ```
    
- **미리 보기**
    - 카카오톡, 페이스북, X와 같은 소셜 미디어에서 웹 사이트를 공유했을 때 웹 사이트의 제목, 설명, 대표 이미지 등이 표시되도록 설정하는 기능.
    
    ```html
    <meta property="og:type" content= "article ">
    ```
    
    !image.png
    
    !image.png
    

### `<link>` 태그: 외부 파일 연결

- **CSS 파일 연결**: rel은 relationship의 줄임말로, 연결하는 파일의 종류를 웹 브라우저에 알려줌.
    
    ```html
    <link rel="stylesheet" href="style.css">
    ```
    
- **파비콘 설정**: 웹 브라우저 탭에 표시되는 작은 아이콘
    
    ```html
    <link rel="icon" href="favicon.ico">
    ```
    

### `<style>` 태그: 내부 스타일 시트

- **`<style>`**: HTML 문서 안에서 CSS 코드를 직접 작성하고 싶을 때 사용.
    - HTML 코드와 섞여 가독성이 떨어지므로 실무에서는 CSS 파일을 따로 작성하고 `<link>` 태그로 연결하는 방식을 더 많이 사용함.
    
    ```html
    <style>
    	p {
    		color: blue;
    	}
    </style>
    ```
    

### `<script>` 태그: 자바스크립트 연결

- **`<script>`**: 웹 페이지에 동적인 기능을 추가하는 자바스크립트.
    - `</body>` 태그 직전에 `<script>` 태그를 배치하면 HTML 구조를 먼저 읽고 자바스크립트 코드를 실행하여 렌더링 성능을 개선할 수 있음.
        
        ```html
        <script src=”app.js”></script>
        ```
        
    - 또는 `<script>` 태그 내부에 자바스크립트 코드를 직접 작성도 가능함.
        
        ```html
        <script>
        	alert("안녕하세요!");
        </script>
        ```