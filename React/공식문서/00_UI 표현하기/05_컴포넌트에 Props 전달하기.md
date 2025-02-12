- 함수의 인수와 동일한 역할
- 컴포넌트에 대한 유일한 인자
#### 학습 내용
- props 전달 방법
- props 읽는 방법
- props 기본값 지정 방법
- 컴포넌트에 JSX 전달 방법
- 시간에 따라 props가 변하는 방식

#### 컴포넌트에 props 전달하기
- 자식 컴포넌트에 props 전달하기
```jsx
export default function Profile() {  
	return (  
		<Avatar  
			person={{ name: 'Lin Lanying', imageId: '1bX5QH6' }}  
			size={100}  
		/>  
	);  
}
```
- 부모 컴포넌트에서 props 읽기
	- 기본값 지정 가능 
		- 예) size의 기본값은 50
		  - size가 없거나 undefined일 때 적용됨
		  - size가 null이거나 0일 때는 적용되지 않음
```jsx
function Avatar({ person, size = 50 }) {  
	// person과 size는 이곳에서 사용가능합니다.  
}
```

#### spread 문법으로 props 전달
```jsx
function Profile(props) {  
	return (  
		<div className="card">  
			<Avatar {...props} />  
		</div>  
	);  
}
```

#### 자식을 JSX로 전달
```jsx
import Avatar from './Avatar.js';

function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}

export default function Profile() {
  return (
    <Card>
      <Avatar
        size={100}
        person={{
          name: 'Katsuko Saruhashi',
          imageId: 'YfeOqp2'
        }}
      />
    </Card>
  );
}

```


#### 시간에 따라 props가 변하는 방식
- 컴포넌트가 props를 변경해야 하는 경우
	- ex1, 사용자의 상호작용
	- ex2, 새로운 데이터에 대한 응답
- 부모 컴포넌트에 다른 props, 즉, 새로운 객체를 전달하도록 요청
- 그러면 이전의 props는 버려지고, 결국 자바스크립트 엔진은 기존 props가 차지했던 메모리를 회수
- 읽기 전용 스냅샷, 렌더링 할 때마다 새로운 버전의 props를 받음
- Props는 변경할 수 없음
- 상호작용이 필요한 경우 state를 설정해야 함

#### todo : 예제 풀어보기