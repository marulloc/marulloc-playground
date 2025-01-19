https://www.youtube.com/watch?v=q1fQnGG1bgU

- [x] Window 와 Document
	- [x] Window
		- `window` 객체는 브라우저에서 제공하는 글로벌 객체로
		- 브라우저 환경에서 동작하는 여러 API들의 가교 역할을 한다. document, history, location, setTimout, cookieStore 등 다른 브라우저 api에 접근을 제공한다.
		- 브라우저 탭에 대한 접근 권한
	- [x] Document
		- `window` 객체의 속성 중 하나입니다.
		- `document` 객체는 웹 페이지 그 자체를 대표하며, 페이지 내의 HTML 문서에 접근하기 위한 인터페이스를 제공합니다.



# JS는 어떻게 HTML과 CSS를 읽고 수정 할 수 있을까?
---
### BOM ( Browser Object Model )
> 웹 브라우저의 UI, URL, 주소입력창 등 브라우저를 조작 할 수 있도록 만든 Javascript 객체다.

window로 BOM에 접근 가능하다.
전역객체이며 모든 객체들의 최상위 객체임 


BOM은 자바스크립트의 최상위 객체, 모든 객체가 소속된 객체
- 그래서 var 변수는 window 객체의 속성이 되어버림  


### DOM ( Document Object Model )

> 아예 다른 형식의 문서인 HTML을 Javascript가 제어할 수 있도록 만든 브라우저가 제공하는 JS 객체

브라우저는 HTML, XML 문서를 DOM 이라는 객체로 파싱해서 저장한다. 
일종의 Javascript 자료구조 이기 때문에, Javascript가 DOM을 수정하는 것이고
그 DOM을 기반으로 브라우저가 화면에 그리는 것

document를 통해 DOM 접근 가능 

##### Document 객체
DOM 트리의 최상위의 루트노드를 Document 객체라고 한다.
그리고 Javascript에서 Document에 접근할 수 있는 진입점이 되어준다.
그리고 BOM의 window 객체의 하위 객체이다. 

##### DOM API
브라우저가 제공하는 API로 자바스크립트로 DOM에 접근하거나 변경할 수 있게 해준다.



 
