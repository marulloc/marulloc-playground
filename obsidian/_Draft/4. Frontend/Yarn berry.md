
https://helloinyong.tistory.com/343


yarn classic 의 어떤어떤 문제 때문에 



등장한 것으로 일종의 javascript 파일이다. 

> Yarn Classic은 OS 인터페이스에서 수행되는 Shell 명령어라면, Yarn Berry는 단순히 해당 프로젝트에서만 동작하는 자바스크립트로 (프로젝트 루트 경로에 **.yarn/release/yarn-3.2.3.cjs** 라는 파일)

Yarn Berry에서는 전통적인 `node_modules` 폴더를 사용하는 대신, Plug'n'Play (PnP) 기술을 도입하여 의존성 관리를 획기적으로 변경했습니다. PnP는 각 패키지가 필요할 때 로딩되도록 하며, `node_modules` 폴더를 생성하지 않고 각 패키지를 직접 참조합니다. 이 방식은 디스크 공간의 효율성을 높이고, 설치 시간을 단축합니다.

기존에는 node_modules 내에서 각 의존성 모듈 별로 중복되는 모듈을 상위로 호이스팅 하는 등, 각 의존성 트리가 굉장히 복잡하면서, 직접 의존성 추가를 하지 않은 모듈을 이용할 수 있는 유령 디펜던시 문제가 발생하는 등 의존성 관리에 대한 문제 요소들이 많았다.


- yarn dlx를 사용해 js가 아닌 실제 yarn classic 명령어를 사용하는거임 
- yarn berry는 일종의 js 실행코드이기 때문에, 뭔가를 실행..하는건 못하... ? 



# Plug'n'Play
---
yarn berry에서는 디펜던시 모듈을 관리할 수 있는 방식이 여러가지가 있는데, 아래와 같이 설정할 수 있다
```
nodeLinker: "pnp" // pnp(default), pnpm, node_modules
```

pnp 모드로 설정으로 디펜던시 모듈을 설치하게 되면, 기존처럼 node_modules로 디펜던시 모듈들이 설치되지 않고, 대신 필요한 라이브러리 모듈들이 .yarn/.cache 디렉토리에 zip 아카이브 파일로 관리하게 되고, zip으로 된 각 모듈의 의존성 트리 정보들은 프로젝트 루트의 .pnp.cjs 파일로 관리하게 된다.

zip으로 압축된 파일로 관리하게 됨으로써 디펜던시 사이즈를 많이 줄일 수 있는 장점

https://www.youtube.com/watch?v=Ix9gxqKOatY



.cache 폴더에 프로젝트의 의존성을 압축파일로 설치 
그리고 .pnp.cjs 파일이 일종의 해시테이블 역할을 해주는데 
-  파일이 어디있는지 
- 패키지 이름과 버전, 그리고 그 패키지의 의존성까지 명시해준다. 

yarn berry는 의존성을 압축파일로 관리하여 
Node.js 가 실행될때 파일I/O를 하지 않고 
.pnp.cjs 파일을 보고 .cache에 있는 압축파일을 읽도록 만든다. 
(pnp.cjs의 packageLocation에 해당 의존성의 위치가 적혀있다. )

-> 따라서 의존성을 불러오는 시간이 크게 단축된다. 


> enableGlobalCache ??
> 

https://velog.io/@dev_jiminn/PM-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90-Yarn-Berry

# zero-install
---
기존에 node_modules에서 모든 디펜던시를 새로 인스톨하려고 하면, 모든 디펜던시 모듈이 인스톨에만 굉장히 많은 시간이 소모하게 된다.

그러나 Zero Install을 이용하면 디펜던시 Install에 걸리는 시간을 없앨 수 있기 때문에, CI 단이나 배포 파이프라인 등에서 반복되는 install 시간을 단축 시킬 수 있게 되고 CI 실행시간 및 배포 시간을 대폭 감소 시킬 수 있다.




# yarn dlx @yarnpkg/sdks vscode 하는 이유 ?
---
PnP 방식은 많은 이점을 제공하지만, 기존의 Node.js 환경을 기대하는 도구들과의 호환성 문제를 발생시킬 수 있습니다. 특히, VSCode와 같은 IDE는 프로젝트 내의 의존성을 해석할 때 기존의 `node_modules` 경로를 기반으로 하는데, PnP 환경에서는 이러한 경로가 존재하지 않습니다. 이로 인해, TypeScript, ESLint 등의 도구가 패키지를 제대로 찾지 못하고 오류를 발생시킬 수 있습니다.

이러한 호환성 문제를 해결하기 위해, Yarn Berry는 VSCode와 같은 IDE에서 PnP 환경을 원활하게 지원하기 위한 SDKs를 제공합니다. 이 SDKs는 VSCode가 Yarn Berry의 PnP 방식을 인식하고 올바르게 의존성을 해석할 수 있도록 돕습니다.


### SDK ?




---
---
---
# yarn berry란 뭔가 ? 

https://helloinyong.tistory.com/343




# 왜 yarn berry는 typescript를 별도로 설정해줘야되는가 
https://kasterra.github.io/setting-yarn-berry/
https://velog.io/@remon/Yarn-berry%EC%97%90%EC%84%9C-React-TypeScript-Jest-Cypress-%EC%84%B8%ED%8C%85%ED%95%98%EA%B8%B0
## 2. Editor SDK 설정

```null
yarn dlx @yarnpkg/sdks vscode
```

Eslint와 TypeScript SDK가 설치됩니다.


??

https://blog.hwahae.co.kr/all/tech/11962
