#### 학습 내용
- 마크업과 렌더링 로직 같이 사용하는 이유
- JSX vs HTML
- JSX로 정보 보여주는 방법

#### 역사
- React에서 렌더링 로직과 마크업이 같은 위치에 함께 있게 된 이유:
	- web이 인터랙티브해지면서, 로직이 내용을 결정하는 케이스 많아짐

#### JSX: JavaScript에 마크업 넣기
- 효과 : 
	- 버튼의 렌더링 로직과 버튼의 마크업이 함께 있으면, 매번 변화가 생길 때마다 서로 동기화 상태를 유지할 수 있음

#### JSX의 규칙
- 하나의 루트 엘리먼트로 반환
	- 불필요한 태그를 마크업에 추가하고 싶지 않을 경우, 
	  fragment `<>` `</>` 사용
	- 🤔 vue에서 v-frag 쓸 때 이슈있어서 안 썻는데, 이유가 기억이 안남... 
- 모든 태그는 닫아주기
	- 태그 열기만 하고 닫지 않으면 오류남
- JSX 내 어트리뷰트는 JavaScript 변수 규칙을 따름
	- 대부분 캐멀 케이스로 작성
	- JSX에서 작성된 어트리뷰트 = JavaScript 객체의 키이며,
	  컴포넌트에서 어트리뷰트를 변수로 읽는 경우가 있는데, 
	  JavaScript 변수 규칙상 대시 포함 불가하기 때문
	- html 과 다른 어트리뷰트가 있으므로 [[https://transform.tools/html-to-jsx|변환기]] 활용 가능
		- 예) class -> className
			- 위와 같은 이유, class는 예약어라 변수 규칙상 사용 불가
			- 이외 [[https://ko.react.dev/reference/react-dom/components/common|표준 DOM Props]]
		- 예외) aria-* / data-*
			- SKIP(todo : 역사적인 이유가 있다고 함)

#### 왜 JSX 태그를 하나로 감싸줘야 하는지
- 보이는 형태 : HTML
- 실제 형태 : JavaScript 객체
	- 배열로 감싸지 않은 하나의 함수에서 두 개 객체 반환 불가하기 때문
