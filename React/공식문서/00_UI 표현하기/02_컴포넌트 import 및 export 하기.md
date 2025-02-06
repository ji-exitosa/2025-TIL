#### 학습 내용
- Root 컴포넌트 정의
- 컴포넌트 import/export
- default/named imports/exports 사용법
- 한 파일 내 여러 컴포넌트 import/export
- 여러 컴포넌트 여러 파일로 분리

#### Root 컴포넌트란
- `App.js`라는 **root 컴포넌트 파일**에 존재
- 설정에 따라 root 컴포넌트가 다른 파일에 위치할 수 있음
	- ex) Next.js처럼 파일 기반으로 라우팅하는 프레임워크

#### 컴포넌트를 import하거나 export 하는 방법
 - 컴포넌트 추가 할 JS 파일 생성
 - 새로 만든 파일에서 함수 컴포넌트 export
	 - default / named export 방식 사용
 - 컴포넌트를 사용할 파일에서 import
	 - default / named import 방식 사용
 - import 할 때, 아래와 같이 .js 붙여주는 방식이 [[https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Modules|native ES Modules]] 사용 방법에 더 가까움
 ``` js
 import A from './A.js';
```

#### Default와 Named Exports
- 보통, 
	- 하나의 컴포넌트만 export 할 때 default export
	- 여러 컴포넌트를 export 할 때 named export
- default, named export 혼용 가능
- default
	- 하나의 파일에서 딱 하나만 존재 가능
	- import 하는 곳에서 새로운 이름으로 사용 가능
	```js
	import A from './file.js';
```
- named export
	- 여러개 존재 가능
	- import/export 하는 곳에서 동일한 이름으로만 사용 가능
	```js
	import A from './file.js'; // default 값 가져옴
	import {B} from './file.js'; // 가능 (B 가져옴)
```

#### 한 파일에서 여러 컴포넌트를 import 하거나 export 하는 방법
- 그냥 vue에서 하던대로..

