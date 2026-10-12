# Q42. this 키워드란 무엇인가요?

### this 키워드의 의미

- **this**: 함수가 호출될 때 결정되는, **“자신을 호출한 객체”**를 참조하는 특별한 키워드. (자신 아님)
- 다른 객체 지향 언어(예: 자바, C++)에서는 this가 보통 ‘인스턴스 자신’임.
- 반면 자바스크립트의 this는 **호출 시점의 상황에 따라 동적으로 바인딩**됨.

### this 키워드를 결정하는 4가지 바인딩 규칙

> this의 값은 함수가 “어떻게” 호출되었는지에 따라 결정됨.
> 
1. **기본 바인딩**
    - 함수가 어떤 객체의 메서드로 호출되지 않고 **독립적으로 호출될 때** 적용되는 규칙
    - 이 경우 `this`는 **전역 객체**를 가리키며 **브라우저 환경**에서는 `window` 객체, **`node.js` 환경**에서는 `global` 객체를 참조함.
    - **엄격 모드**(`’use strict’;`)에서는 전역 객체로 자동 바인딩되지 않고 `this`의 값이 **`undefined`로 설정**됨.
        
        → **의도치 않은 전역 객체 참조 방지**
        
        ```jsx
        function showThis() {
        	console.log(this);
        }
        
        showThis(); // 브라우저: window 객체 / node.js: global 객체 또는 undefined(엄격 모드)
        
        const obj = {
        	name: '객체',
        	logName: function() {
        		funciton innerFunction() {
        			console.log(this); // 여기서의 this는 전역 객체(또는 undefined)
        		}
        		innerFunction(); // innerFunction은 독립적으로 호출됨
        	}
        };
        obj.logName();
        ```
        
    - 가장 쉽게 확인하는 방법은 호출할 때 **함수 이름 앞에 점(.)이 있는지** 보는 것.
        
        ```jsx
        obj.logName(); // 점 앞에 obj가 있음 -> this는 obj
        showThis(); // 점이 없음 -> 주인이 없음 -> 기본 바인딩
        ```
        
        - 점 없이 그냥 함수() 형태로 부르면 **이 함수를 부른 주인 객체가 없음**. **이때 JS가 정한 기본값이 적용됨.**
            - 일반 모드: 주인이 없으니까 **전역 객체(브라우저의 `window`)**를 `this`로 넣어줌.
            - 엄격 모드: **엉뚱한 전역 객체를 건드리지 않도록** 그냥 `undefined`로 둠.
    - 예제 코드에서 헷갈리는 부분
        
        ```jsx
        const obj = {
        	name: '객체',
        	logName: function() {
        		console.log(this); // ① obj
        		
        		function innerFunction() {
        			console.log(this); // ② window 또는 undefined
        		}
        		innerFunction(); // 점이 없음
        	}
        };
        
        obj.logName(); // 점 앞에 obj
        ```
        
        - ① `logName`은 `obj.logName()`으로 호출되었으니까 `this`는 `obj`.
        - ② `innerFunction`은 `obj` 안에 작성되어 있지만, 호출할 때는 `innerFunction()`처럼 점 없이 불림. 그래서 바깥 함수의 `this`를 이어받지 않고, 기본 바인딩이 적용되어서 `window`(엄격 모드면 `undefined`).
            
            → obj 안에 있으니까 this도 obj겠지?라고 생각할 수 있으나, **위치는 상관없고 호출 형태만 중요하다!**
            
2. **암시적 바인딩**
    - 함수가 객체의 속성, 즉 **메서드로 호출될 때** 적용되는 규칙.
    - 이 경우 `this`는 **해당 객체 자신을 가리키므로** 호출 시점의 **마침표(.) 앞의 객체가 `this`**가 됨.
        
        ```jsx
        const person = {
        	name: '김민준',
        	sayHello: function() {
        		console.log(`안녕하세요, 저는 ${this.name}입니다.`);
        	}
        };
        
        person.sayHello(); // ① 안녕하세요, 저는 김민준입니다.
        
        const anotherPerson = {
        	name: '이수진',
        };
        anotherPerson.sayHello = person.sayHello;
        anotherPerson.sayHello(); // ② 안녕하세요, 저는 이수진입니다.
        ```
        
        - ① 점 앞에 `person`이 있으니까 `this`는 `person`이다. 그래서 `this.name`은 `person.name`, 즉 김민준이 됨.
        - ② 앞줄에서는 함수 자체를 복사하는 것이 아니라, **같은 함수를 `anotherPerson`도 가리키게 하는 것**.
            
            **→ 함수는 하나인데 두 객체가 각자 그 함수를 속성으로 들고 있는 상태.**
            
            ```
            person.sayHello ──────┐
                                  ├──▶  function() { console.log(this.name...) }
            anotherPerson.sayHello┘
            ```
            
        - 예제 핵심: this는 함수가 어느 객체에 정의됐는지와 상관없이 **호출 순간에 정해진다.**
3. **명시적 바인딩**
    - `call()`, `apply()`, `bind()`를 이용한 호출.
    - 개발자가 함수를 실행할 때 **`this`로 사용할 객체를 직접 지정**할 수 있다.
    - `call()`, `apply()`: **첫 번째 인자로 this로 바인딩할 객체를 전달**
        - `call()`: **두 번째 인자부터 개별 인자**로 전달
        - `apply()`: 실행에 필요한 인자들을 **하나의 배열 형태**로 전달
    - `bind()`: 함수를 즉시 실행하지 않고 **전달한 객체가 this로 영구적으로 고정된 새로운 함수를 반환**
        - 영구적으로 고정 → `bind`로 만든 함수는 이후에 **어떻게 호출하든 `this`가 바뀌지 않음.**
        
        ```jsx
        function introduce(job, hobby) {
        	console.log(`안녕하세요, 저는 ${this.name}이고, 직업은 ${job}, 취미는 ${hobby}입니다.`);
        }
        
        const user = { name: '박영희' };
        
        introduce.call(user, '개발자', '코딩');
        // ① 안녕하세요, 저는 박영희이고, 직업은 개발자, 취미는 코딩입니다.
        
        introduce.apply(user, ['디자이너', '그림 그리기']);
        // ② 안녕하세요, 저는 박영희이고, 직업은 디자이너, 취미는 그림 그리기입니다.
        
        const boundIntroduce = introduce.bind(user, '학생');
        boundIntroduce('독서');
        // ③ 안녕하세요, 저는 박영희이고, 직업은 학생, 취미는 독서입니다.
        ```
        
        - ① **call:** **바로 실행**, 인자는 쉼표로
            - 첫 번째 인자 `user`가 `this`가 됨
            - 나머지 `개발자`, `코딩`은 순서대로 `job`, `hobby`에 들어감
            - 그리고 **즉시 실행**됨
        - ② **apply**: **바로 실행**, 인자는 배열로
            - call과 하는 일은 똑같고, **인자를 배열 하나로 묶어서 넘긴다**는 것만 다름.
        - ③ **bind**: **실행하지 않고, this가 고정된 새 함수를 만듦**
            - `introduce.bind(user, ‘학생’)`: 아직 아무것도 출력되지 않음.
                
                → 대신 `this`는 `user`, 첫 번째 인자는 `학생`으로 **미리 채워둔 “새 함수”**를 돌려줌.
                
            - `boundIntroduce(’독서’)`: **이제 실행**됨. **미리 채운 학생이 `job`에**, 지금 넘긴 독서가 `hobby`에 들어감.
4. **new 바인딩**
    - **생성자 함수**를 `new` 키워드와 함께 호출할 때 적용되는 규칙.
    - 이 경우 자바스크립트 엔진은 **새로운 객체 인스턴스를 생성**하고 **함수 내부의 `this`는 새로 만들어진 객체에 바인딩**함.
        
        → 생성자 함수 안에서 `this`를 사용하면 **앞으로 생성”될” 인스턴스 자신**을 가리키게 됨.
        
        ```jsx
        function Person(name, age) {
        	// new 키워드로 호출될 때, 이 this는 새로 생성될 객체 인스턴스를 가리킴.
        	this.name = name;
        	this.age = age;
        	this.sayName = function() {
        		console.log(`제 이름은 ${this.name}입니다.`);
        	};
        }
        
        const p1 = new Person('최지우', 28);
        const p2 = new Person('정우성', 40);
        
        p1.sayName(); // 제 이름은 최지우입니다. (this는 p1 객체)
        p2.sayName(); // 제 이름은 정우성입니다. (this는 p2 객체)
        ```
        
        - `new Person(’최지우’, 28)`을 실행하면 내부에서 일어나는 일
            
            ```jsx
            function Person(name, age) {
            	// 1. const this = {}; 빈 객체 생성
            	// 2. this에 속성 채우기
            	this.name = name; // { name: '최지우' }
            	this.age = age; // { name; '최지우, age: 28 }
            	this.sayName = function () { ... };
            	// 3. return this; 다 채운 객체를 자동으로 반환
            }
            ```
            

### 엄격 모드에서의 this 키워드

- 자바스크립트의 **엄격 모드**: 일반 모드에서는 허용되던 **일부 문법적 오류나 비정상적인 동작**을 **오류로 처리하거나 제한**하여 더 안전하고 예측 가능한 코드를 작성하도록 도움.
    
    ```jsx
    'use strict'; // 엄격 모드 선언
    
    function showStrictThis() {
    	console.log(this); // undefined. window 아님!
    }
    
    showStrictThis(); // 점 없음 -> 기본 바인딩 -> undefined
    
    const strictObj = {
    	method: function() {
    		console.log(this); // strictObj (암시적 바인딩은 그대로)
    		
    		function innerStrict() {
    			console.log(this); // undefined
    		}
    		innerStrict(); // 점 없음 -> 기본 바인딩 -> undefined
    	}
    };
    
    strictObj.method();
    ```
    

### 연쇄적인 호출부와 this

- 함수가 **여러 단계에 걸쳐 호출**될 때 `this`는 **최종적으로 함수를 호출한 방식(마침표 앞의 객체)**에 따라 결정됨.
    
    ```jsx
    const person = {
    	name: '김서준',
    	friends: ['박지혜', '이민호'],
    	showFriends: function() {
    		console.log(this); // ① person
    		
    		this.friends.forEach(function(friend)) {
    			console.log(`${this.name}의 친구: ${friend}`); // ② 여기서의 this는 전역 객체(또는 undefined)
    		});
    	}
    };
    
    person.showFriends();
    // 예상: 김서준의 친구: 박지혜, 김서준의 친구: 이민호
    // 실제: (브라우저) window의 친구: 박지혜, window의 친구: 이민호
    //      (엄격모드) undefined의 친구: 박지혜, undefined의 친구: 이민호 (TypeError)
    ```
    
    - ① `showFriends` 안의 `this`
        - `person.showFriends()`로 호출했으니 점 앞이 `person`, 즉 **암시적 바인딩**이다.
        - 그래서 `this.friends`는 정상적으로 `person.friends`이므로 [`’박지혜’, ‘이민호’]`를 가리킴.
    - ② 콜백 함수 안의 `this`
        - `forEach`에 넘긴 `function (friend) { … }` 는 **우리가 직접 호출하는 게 아니라 `forEach`가 내부에서 호출**함.
            
            ```jsx
            // forEach 내부 동작을 아주 단순화하면
            for (let i = 0; i < arr.length; i++) {
            	callback(arr[i]); // 점 없이 호출
            }
            ```
            
            - 점 없이 호출되므로 기본 바인딩이 적용됨.
        - “연쇄적인 호출”이라는 말의 의미는, **바깥 함수의 `this`가 `person`이어도 안쪽 함수는 자기가 호출된 방식으로 `this`를 새로 정한다.**

### 콜백 함수에서의 this 문제와 해결책

- **콜백 함수**: **함수를 다른 함수의 인자로 전달**하는 형태. 이때 this 바인딩 문제가 자주 발생함.
1. **bind() 사용하기**
    - 콜백 함수에 this를 명시적으로 바인딩.
        
        ```jsx
        // ... person 객체 정의 ...
        showFriends: function() {
        	this.friends.forEach(function(friend) {
        		console.log(`${this.name}의 친구: ${friend}`);
        	}.**bind(this)**); // 여기에 .bind(this) 추가
        }
        
        person.showFriends() // 호출 시 이제 '김서준의 친구: ...'로 제대로 출력
        ```
        
2. **화살표 함수 사용하기**
    - 일반 함수와 달리 **자신만의 `this` 바인딩을 가지지 않음.**
    - 대신 **함수가 선언될 당시의 상위 스코프에 있는 `this`를 정적으로 상속받는** 특징이 있다.
        
        → 화살표 함수는 콜백 함수에서 **`this`가 의도와 다르게 바인딩되는 문제를 해결**하는 가장 편리한 방법이다.
        
        ```jsx
        // ... person 객체 정의 ...
        showFriends: function() {
        	this.friends.forEach((friend) **=> {** // 화살표 함수 사용!
        		console.log(`${this.name}의 친구: ${friend}`);
        	**}**);
        }
        
        person.showFriends() // 호출 시 이제 '김서준의 친구: ...'로 제대로 출력
        ```
        
3. **변수에 this 저장하기**
    - **`this`를 다른 변수에 저장한 후 사용**하는 방법.
    - 화살표 함수 도입 이전에 흔히 사용되던 방식이다.
        
        ```jsx
        showFriends: function() {
        	const self = this; // this를 self 변수에 저장
        	this.friends.forEach(function(friend) {
        		console.log(`${self.name}의 친구: ${friend}`); // self 사용
        	});
        }
        ```
        

### 모의 면접

1. 자바스크립트에서 this가 함수 호출 방식에 따라 달라지는 4가지 바인딩 규칙은 무엇이며 각각의 특징은 무엇인가요?
    
    > **this는 함수가 “어떻게 호출됐는지”에 따라 결정되며, 기본 바인딩, 암시적 바인딩, 명시적 바인딩, new 바인딩으로 4가지 규칙이 있습니다.**
    **기본 바인딩**이란 함수가 어떠한 메서드로 호출되지 않고 독립적으로 호출될 때 적용됩니다. 이때 일반 모드에서는 전역 객체로, 엄격 모드에서는 undefined가 됩니다.
    **암시적 바인딩**이란 함수가 어떠한 객체의 메서드로 호출될 때 적용되며, this는 호출한 객체, 즉 마침표 앞의 객체를 가리킵니다.
    **명시적 바인딩**이란 call(), apply(), bind()로 this를 직접 지정하는 방식입니다. call과 apply는 즉시 실행하고, bind는 this가 고정된 새 함수를 반환합니다.
    **new 바인딩**이란 생성자 함수를 new로 호출할 때 적용되는 규칙으로, 생성자 함수 안에서 this를 사용하면 new로 생성될 인스턴스 자신에 적용됩니다.
    > 
2. 콜백 함수 안에서 this가 의도한 객체를 가리키지 않을 때 이를 해결할 수 있는 방법 3가지는 무엇인가요?
    
    > **콜백 함수는 다른 함수가 내부에서 단독으로 호출하기 때문에 기본 바인딩이 적용되어 this를 잃어버립니다.**
    첫 번째로는 bind()로 this를 고정한 함수를 콜백으로 넘기는 방법이 있습니다.
    두 번째로는 바깥의 this를 self 같은 변수에 저장해두고 콜백에서 그 변수를 쓰는 방법으로, 화살표 함수 도입 이전에 많이 사용되던 방법입니다.
    세 번째로는 화살표 함수를 쓰는 방식입니다. 화살표 함수는 자신의 this가 없고 바깥 스코프의 this를 그대로 사용하기 때문에 가장 간단하고 현재 가장 많이 쓰입니다.
    >