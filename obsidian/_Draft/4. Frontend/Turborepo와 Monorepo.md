더 나은 서비스를 위해 끊임없이 문제제기하는 것과
환경에 대한 불만보다, 불편한 환경을 개선할 해결책을 제시하고 행동하는 데에 
장점이 있다고 생각하며, 

# Turborepo vs workspace ?
---
Turborepo를 설치할 때 npm, yarn, pnpm, bun workspace 중에 하나를 고르라고한다. 
Turborepo는 뭐고 Workspace는 뭔가 ? 

### Turborepo
Turborepo는 JS, TS 프로젝트를 위한 빌드시스템이다. 
Turborepo는 즉 모노레포 환경에서 빌드, 테스트, 배포 작업을 관리하도록 설계되어 있는데 

특화된 기능으로, 
1. 로컬 및 원격 캐싱으로 빌드 시간 단축하고 
2. 의존성 그래프를 기반으로 동시에 실행 할 수 있는 작업을 자동으로 파악하여 여러 작업을 병렬
3. 코드 재사용 : 당연히 모노레포 관리 툴이니까 공용 코드를 재사용해서 중복을 최소화하지

### Workspace
Workspace는 패키지매니저가 제공하는 것으로 
한개의 저장소 내에서 여러개의 패키지를 효율적으로 관리 할 수 있도록 돕는다. 

workspace의 일반적인 기능으로
1. 의존성 관리 : 프로젝트의 모든 패키지가 같은 라이브러리를 공유하여, 패키지간 의존성을 쉽게관리
2. 단일 설치 : 중복되는 패키지의 의존성을 한 번 설치하여 관리 복잡성 줄이고 설치 시간 줄인다.
3. 프로젝트간 링크 : 개발중인 로컬 패키지간에 심볼릭 링크를 자동으로 생성하여 하나의 패키지에서 다른 패키지로 변경사항이 실시간 변경할 수 있게 한다. 

> 즉 Turborepo는 Workspace 기능을 이용하여 의존성을 관리하고 빌드를 최적화 해주는 것이다. 

> Workspace만 써도 monorepo를 구축 할 수 있지만, Turborepo를 활용해 빌드-테스트-배포 프로세스를 최적화하고 간편하게 처리 할 수 있도록 만들 수 있다. 


! 의존성 그래프 ???
! 로컬 및 원격 캐싱으로 빌드시간 단축 ?? 어떻게 ??

 

# Yarn berry Workspace setUp
https://techblog.woowahan.com/7976/
https://velog.io/@dev_jiminn/MonoRepo-with-Yarn-Berry

1. 먼저 한 디렉토리에서 yarn set verion berry
2. yarn init 을 통해 프로젝트 이닛 
3. package.json에 workspaces 입력 
	1. 나는 apps/* 와 packages/* 로 나눴음 
4. apps 경로로가서 CRA 같이 프로젝트 하나 생성
	1. NodeModules나 .lock 파일 삭제후 
5. yarn workspaces list 명령어를 입력했을때 프로젝트의 이름이 나오면 인식이 된 것임
	1. yarn 명령어로 의존성을 yarn berry 로 설치한다. 

6. yarn berry는 node_modules와 달리 파일을 인식하는 방식이 달라서 (yarn berry는 일종의 JS 코드임)
	1. vscode가 tsconfig나 lint 파일을 읽지 못한다. 
	2. 따라서 yarn berry에서 제공하는 sdk를 설치하여 vscode에게 yarn berry의 방식을 이해시켜야한다.

```cmd
yarn add -D typescript prettier eslint
yarn dlx @yarnpkg/sdks vscode
```

#### 의존성문제 해결 1 . testing-library
	1. 설치중에 peer-dependency 문제를 만났다. 
```cmd
➤ YN0002: │ project-a@workspace:apps/project-a doesn't provide @testing-library/dom (p1988a), requested by @testing-library/user-event.
➤ YN0086: │ Some peer dependencies are incorrectly met; run yarn explain peer-requirements <hash> for details, where <hash> is the six-letter p-prefixed code.
```
![[Screenshot 2024-04-23 at 5.36.23 PM.png]]
1. 여기서 yarn explain peer-requiredments p1988a를 쳐보자
	1. 음.. 아직 지원하지 않는 문제유형이라고한다.
![[Screenshot 2024-04-23 at 5.36.46 PM.png]]

2.  testinglibrary/dom을 설치하기전에 먼저 pnp.cjs를 확인해보니 testing-library/dom가 이미 설치는 되어있다. 그러면 이 @testing-library/user-event가 testing-libarary/dom 사용을 명시가 정확하지 않은 것이 이유 일 수 있다. 
```js
["@testing-library/user-event", [\
	["npm:13.5.0", {\
		"packageLocation": "../../.yarn/berry/cache/~~~~",\
		"packageDependencies": [\
			["@testing-library/user-event", "npm:13.5.0"]\
		],\
		"linkType": "SOFT"\
	}],\
	["virtual:b3cf3f283cb98d~~~~~~", {\
		"packageLocation": "./.yarn/__virtual__/@testing-library-user-event~~",\
		"packageDependencies": [\
			["@testing-library/user-event", "virtual:b3cf3f283cb98~~~~~"],\
			["@babel/runtime", "npm:7.24.4"],\
			["@testing-library/dom", null],\ // 여기 주목
			["@types/testing-library__dom", null]\
		],\
	
		"packagePeers": [\
			"@testing-library/dom",\ //여기 주목
			"@types/testing-library__dom"\
		],\
		"linkType": "HARD"\

	}]\
]],\
```

1. 여기를 보면 peer에 testing-library/dom에 명시는 되어있지만
2. packageDependency에 testing-library/dom의 버전이 null 로 명시되어 있는 것이 문제일 수 있다. 즉 버전을 인식하지 못하고 있는데, 이것은 의존성해석 과정에 문제가 발생 할 확률 이 크다 
3. 지금 어떻게 사용되고 있는지 why 옵션을 사용해보자 `yarn why @testing-library/dom`
![[Screenshot 2024-04-23 at 5.47.22 PM.png]]
- 여기보면 npm ^8.5.0 버전을 따르지만 pnp.cjs에는 null로 표현된다.
4. 이걸 해결하기 위해 2가지 방법이 존재한다.
	1. 프로젝트의 package.json에 testing-library/dom 을 직접 명시하는 것과
		1. 얘는 프로젝트에 명시되어 있어 파악하기 쉽다.
	2. yarnrc 파일에서 packageExtension을 사용해서 수동으로 명시하는 것
		1. 얘는 전역으로 의존성 버전 관리할 때 유용하고 
		2. 실제프로젝트의 package.json을 수정하지 않아도 된다.

```
packageExtensions: 
	"@testing-library/user-event@*": 
		peerDependencies: "@testing-library/dom": "^8.5.0"
```
- 근데 이 작업을 하니 yarn install 명령어시에 필요없다는 문구가 뜬다. 그러니까 다른 해결책을 써보자
- 그러니까 직접 프로젝틔 패키지제이슨에  "@testing-library/dom": "^8.5.0" 요걸추가해주니까
- 문제 해결
- 확인해보면 
```js
["@testing-library/user-event", [\
	["npm:13.5.0", {\
		"packageLocation": "../../.yarn/berry/cache/~~~~",\
		"packageDependencies": [\
			["@testing-library/user-event", "npm:13.5.0"]\
		],\
		"linkType": "SOFT"\
	}],\
	["virtual:b3cf3f283cb98d~~~~~~", {\
		"packageLocation": "./.yarn/__virtual__/@testing-library-user-event~~",\
		"packageDependencies": [\
			["@testing-library/user-event", "virtual:b3cf3f283cb98~~~~~"],\
			["@babel/runtime", "npm:7.24.4"],\
			["@testing-library/dom", "npm:8.20.1"],\ // 여기 주목
			["@types/testing-library__dom", null]\
		],\
	
		"packagePeers": [\
			"@testing-library/dom",\  
			"@types/testing-library__dom"\
		],\
		"linkType": "HARD"\

	}]\
]],\
```
- packageDependency에 버전이 명시가 되었다. 
- 에러도 안뜬다. 
![[Screenshot 2024-04-23 at 11.51.51 PM.png]]


### 의준성문제 해결 2 . 또다른 문제 

app.test.tsx 파일에서 toBeInDocument 메소드를 찾지 못한다. 

삭제해주고 다시 설치하면서 해결 
yarn remove @testing-library/jest-dom 
yarn add -D @testing-library/jest-dom

이제 이런 의존성문제가 발생한다. 
![[Screenshot 2024-04-23 at 6.36.06 PM.png]]

types/jest의 버전이 jest-dom  이 요구하는 것 보다 낮다는 의미인데 
types/jest 버전을 28위로 올려주거나 삭제했다 재설치하자
그럼 해결 








#### 여기서 왜 어떤건 루트에 설치하고 어떤건 패키지에만 설치하는가 ? 
- 공통된 의존성 (빌드툴, 테스팅라이브러리, 타입스크립트, 린트, 프리티어) 등은 루트레벨에
- 다른 패키지간에 다른 버전을 쓰는경우는 각 패키지레벨의 package.json 에 명시하자


Yarn Workspace 같은 모노레포 설정에서, 루트 레벨에 설치된 TypeScript를 사용하려면, 각 패키지에서 이를 명시적으로 참조할 필요가 없을 것 같다. 그리고 .yarn/.cache에 모든 zip 파일 형태로 의존성들이 존재해서 의존성의 중복을 막고 있지만, 이것만으로는 각 패키지에서 typescript를 사용 할 수는 없기에 

여기서 typescript 4.5.4 버전을 루트 package.json 에도 명시하고
패키지레벨의 package.json 에도 typescript 4.5.4 버전을 명시해야
yarn berry나 vscode가 해당 프로젝트가 어떤 의존성을 사용하는지 알 수 있게 된다.

그리고 보통 루트레벨에 설치하는 의존성들은 
각 패키지에서 상속하고 확장의 기준이  되는데 ts의 extends를 통해 
루트레벨의 tsconfig를 상속받아 패키지별로 확장해서 쓰면된다. 

만약 루트레벨의 ts 가 5버전으로 업되었고 패키지레벨에서는 아직 4. 버전을 쓰고 있으며 패캐지레벨에서는 extends를 통해 root 의 tsconfig를 상속받고 있어 이럴경우에 만약 루트에 있는 새로운 tsconfig 기능을 쓴다면 어떻게돼 ? 빌드가 안되어 빌드타임에 버그를 잡을 수도 있다 .
