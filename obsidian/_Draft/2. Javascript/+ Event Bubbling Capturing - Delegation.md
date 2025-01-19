
https://mydeveloplife.tistory.com/62

- [x] 이벤트 버블링 캡쳐링 딜리게이션  
	- DOM 이벤트가  여러 DOM 들을 통해 전파되는 것인데 
	1. **캡처링 단계**: 최상위 노드에서부터 이벤트가 발생한 요소까지 이벤트가 내려갑니다.
	2. **타겟 단계**: 이벤트가 실제 발생한 요소(타겟)에서 이벤트 리스너가 실행됩니다.
	3. **버블링 단계**: 이벤트가 발생한 요소에서 시작하여, 최상위 노드 방향으로 이벤트가 올라갑니다.

	- [x] 버블링
		-  event target에서 최상위(document)까지 전파
		- 막으려면 StopPropagation
			- 버블링을 통해 event delegation 처리 가능 
	- [x] 캡쳐링
		- DOM Tree 최상위(document)에서 event target까지로 전파
		- addEventListener의 세번째 인자에 true해야 캡처링 활성화 
		- 타겟에 도달하기전에 인터셉트 하는 용도 


- [x] event.target 과 event.currentTarget
	- [x] currentTarget
		- 이벤트가 등록된 요소
		- 이벤트 딜리게이션 될때 사용 , Form에서 onSubmit 같은 곳에서 사용
	- [x] target
		- 이벤트가 발생한 요소