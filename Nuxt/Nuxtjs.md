- Nuxt.js 란?
	- Vue.js 개발 편의성을 높여주는 오픈소스 프레임워크

- 장점
	- Vue.js만으로 개발할때에 비해 생산성이 높음
	- 코드 분할 자동화
	- SSR / SPA 지원
	- pages 폴더 기반의 자동 라우팅 설정
	- 정적 파일 전송
	- ES2015+ 지원
	- 전처리기 지원 (SASS, LESS, Stylus..)
	- Pre-rendering

- 버전 확인 방법
	- `nuxt.config.js` 파일 확인
		- Nuxt 2.x 기준,  `mode: 'universal', // Universal 모드 <-> spa(SPA 모드)`
		- Nuxt 3.x 기준, `ssr: true, // Universal 모드 <-> false(SPA 모드)`
		- 단, Nuxt 2.13 이후로는 `mode` 속성을 명시하지 않으면 기본값은 `'universal'`로 설정됩니다.
			- 현재 mbfo는 이 상태