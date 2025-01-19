
문제를 지워버려서 ㅋㅋㅋ
코드를 보면서 생각나는대로 적었습니다.

 - 토스 과제전형 - mortgage.ts 
	- 주택담보대출 기능을 구현하시오

> 개인적으로 느낀 특징 
>  main 브랜치가 기본형태 
	- 환경적인 특징
		- main 브랜치에 기본적인 플로우가 갖춰져있음 
		- 모든 UI 구성이 이미 존재했음 
	- 기능구현 문제
		- feature 브랜치를 만들고 2시간안에 기능을 구현하여 PR 보내시오
		- 
			- 기능 구현 특징
				- main 브랜치에 모든 형태와 UI가 구성되어 있었음 
				- UI
---
### Resume
- 프로젝트 별 어떤 문제가 있었고 어떻게 해결했는가

### Common
- CI/CD (큰돌)
- JSON (큰돌)
- git flow | trunk based
	
### Computer Science
- 운영체제
		- https://www.youtube.com/watch?v=4ZyUAZ6Vrfc
		- 부동소수점
	1. 
- 데이터베이스
		- https://www.youtube.com/watch?v=xkCeakMCKHc&t=8s
- 소프트웨어 공학
		- Agile
- [ ] 네트워크
		- https://www.youtube.com/watch?v=JOwZ9YEAMrY&t=4s
		- HTTP, HTTPS
		- HTTP 1.1 2.0 3.0
		- REST API, GraphQL 
- [ ] 자료구조
		- https://www.youtube.com/watch?v=rEkeloZV1yc&t=39s 
		- 리스트 vs 배열
		- 스택 과 큐
		- 힙
		- 트리와 그래프
		- 해쉬 테이블
- [ ] 알고리즘
		- [ ] 위상정렬 
		- [ ]  배열
		- [ ] 스택, 큐, 덱, 우선순위 큐(힙)
		- [ ] 문자열
		- [ ] 정렬알고리즘
		- [ ] 부루트 포스(완전탐색)
		- [ ] 재귀
		- [ ] 백트래킹
		- [ ] 동적계획법
		- [ ] BFS/DFS 
		
		- [ ] BFS 정리 https://www.youtube.com/watch?v=ansd5B27uJM
		- [ ] DFS 정리 https://www.youtube.com/watch?v=3_eVkTkBbJE
		- [ ] 백트래킹 https://www.youtube.com/watch?v=atTzqxbt4DM
		- [ ] 시뮬레아션 https://www.youtube.com/watch?v=ql82YcFisUI
		- [ ] 투포인터 https://www.youtube.com/watch?v=U0TXIFiCIu0
		- [ ] 이진탐색 https://www.youtube.com/watch?v=D1ad7UCbWHY
		- [ ] 그리디 https://www.youtube.com/watch?v=LD4AKF9tjro
		- [ ] DP https://www.youtube.com/watch?v=qLkFBk5-HrY
		- 정렬
			- [ ] 위상정렬
		- 문자열
			- 
- ETC
	- Bit Operation





Monorepo들은 보통 sym-link를 이용해 
공유하는 라이브러리가 변하면 자동으로 consumer(패키지들을 사용하는 프로젝트)들은 최신 버전의 패키지를 사용할 수 있다는 장점이 있다. 

이때 가장 


챕터회의 때 지크님이 말씀하신 
모노레포의 공유패키지들의 버전관리 문제점에 대해 스택오버플로우에서 찾아봤는데요

여러 글에서도 
모노레포는 공유패키지들을 심링크로 연결하여 
프로젝트에서 최신 버전을 사용하기 때문에 
브레이킹체인지가 있을 때 쉽지않다.. 라고 말하고 있더라구요 

근데 스택오버플로우에서 모노레포 운영 중, 
브레이킹 체인지가 발생했을 때 아직 준비가 안된 프로젝트들에 영향을 미치지 않도록 도와주는 
changesets 라는 라이브러리가 있다고 합니다.. 

물론 챕터회의 때 말씀드린 모노레포 안에서의 버져닝은 아니지만
이렇게 프로젝트 자체를 올드버전의 패키지 상태로 lock 할 수 있는 방법도 있는 것 같습니당..


https://stackoverflow.com/questions/74472915/monorepo-lock-app-to-stay-on-specific-package-version
https://brionmario.medium.com/changesets-is-a-game-changer-fe752af6a8cc

요건  모노레포와 브레이킹체인지 관련한 블로그 글입니다.
https://www.benlorantfy.com/the-problem-with-shared-libraries-and-monorepos
