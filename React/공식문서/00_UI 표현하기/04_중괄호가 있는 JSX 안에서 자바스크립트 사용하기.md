- 중괄호를 사용하면
	- JavaScript 로직 추가 가능
	- 마크업 내부의 동적 프로퍼티 참조 가능

#### 학습내용
- 따옴표로 문자열 전달
- 중괄호가 있는 JSX 안에서 
	- JavaScript 변수 참조 방법
	- JavaScript 함수 호출 방법
	- JavaScript 객체 사용 방법

#### 문자열 전달
- 정적인 값 : 작은 따옴표나 큰따옴표로 묶어야 한다
- 동적인 값 : { } 로 감싸서 전달
	- 단, 태그는 해당 방법으로 동적 할당 불가
	- 객체 전달 가능, 이중 중괄호 활용
```jsx

function formatDate(date) {
  return new Intl.DateTimeFormat(
    'en-US',
    { weekday: 'long' }
  ).format(date);
}

export default function Avatar() {
  const description = '대체 텍스트';
  const name = 'Gregorio Y. Zara';
  return (
	<> 
	    <h1>{name}'s To Do List</h1>
	    <img
	      className="avatar"
	      src="https://i.imgur.com/7vQD0fPs.jpg"
	      alt={description}
	    />
	    <!-- 주의! style 프로퍼티도 캐멀 케이스로 작성 -->
	    <span style={{
		  backgroundColor: 'black', 
		  color: 'pink'
		}}>today is {formatDate(today)}</span>
	</>
  );
}
```


