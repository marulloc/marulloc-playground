> Stateless HTTP를 보완하고 서버와 데이터를 공유하고자 사용된다.

> 쿠키는 서버와 클라이언트 간 상태 정보를 저장하기 위한 저장소
> 세션은 서버측에서 사용자 정보를 저장하고 클라이언트에는 세션ID만을 저장하게함 (보통 쿠키에)
> 토큰은 사용자 인증정보를 문자열로 만들것 - 세션의 DB조회의 비효율성을 개선하기 위해 등장

###  **세션 기반 인증과 토큰 기반 인증의 차이에 대해 얘기해주세요.**
---
> 세션 기반 인증은 클라이언트로부터 요청을 받으면 클라이언트의 상태 정보를 저장하므로 Stateful한  
> 토큰 기반 인증은 상태 정보를 서버에 저장하지 않으므로 Stateless한 구조를 가집니다.

- 세션기반인증
	- **장점:**
		- **상태 유지(Stateful):** 서버가 사용자의 로그인 상태를 기억하고 관리합니다. 이는 사용자가 애플리케이션과 상호작용할 때 일관된 사용자 경험을 제공할 수 있게 합니다.
		- **보안성:** 세션 ID는 일반적으로 안전한 채널을 통해 전송되며, 서버 측에서 세션 정보를 완전히 통제할 수 있습니다.
	- **단점:**
		- **확장성:** 세션 정보를 서버 메모리에 저장하기 때문에 대규모 분산 시스템에서 관리하기가 어려울 수 있습니다.
		- **자원 사용량:** 많은 사용자가 동시에 애플리케이션을 사용할 경우, 서버에 부하를 줄 수 있습니다.

- 토큰기반인증
	- 장점
		- **상태 비저장(Stateless):** 인증 정보를 클라이언트 측에서 관리하므로 서버는 사용자의 로그인 상태를 유지할 필요가 없습니다. 이는 서버의 자원 사용량을 줄이고 확장성을 향상시킬 수 있습니다.
		- **분산 시스템 친화적:** 토큰은 서버, 모바일 애플리케이션, 다른 서비스 간에 쉽게 전달되어 인증을 용이하게 할 수 있습니다.
		- **크로스 플랫폼:** 다양한 클라이언트(웹, 모바일 등)에서 사용하기 적합합니다.
	- 단점
		- **보안 취약점:** 토큰은 클라이언트 측에서 관리되므로, 잘못 구현된 경우 보안에 취약할 수 있습니다. 특히, XSS(크로스 사이트 스크립팅)나 CSRF(크로스 사이트 요청 위조) 공격에 노출될 위험이 있습니다.
		- **토큰 만료 관리:** 토큰은 만료 기간이 정해져 있으며, 만료된 토큰을 새로 발급하는 과정이 필요할 수 있습니다.

##### !!! 핀다나 뱅샐에서는 어떤 인증방식 사용할래요 ?

아무래도 핀테크기업에는 서버가 여러개일 것 같다 
여러 호스트에 인증정보를 보내야하므로 토큰기반인증을 선택할 것 같은데 

보안적 측면에선 XSS를 막기위해 httpOnly 쿠키를 사용하고
sameSite옵션을 통해 CSRF 공격을 방어하는 것을 선택할 것 같다

또 AccessToken 유호기간을 잛게 두고 
RefreshToken을 일회용으로 사용하면 될 것 같다는 생각 



# Cookie
---
- Cookie : 서버와 쿠키는 데이터 공유를 위한 매개체 
	- 서버가 클라이언트 브라우저에 데이터를 저장할 수 있음

	- 보통 유저를 기억하기 위해 쓰임
		- response에 `set-cookie` 헤더를 받으면 브라우저가 저장하게됨
		- 브라우저는 그 url에 데이터를 보낼때마다 
			- 쿠키를 req 헤더에 담아서 보내게됨 
			- 즉 쿠키는 도메인에 따라 분리됨
	- 쿠키는 유효기간 존재
	- 인증 뿐만 아니라 여러가지 정보 저장 
		- 웹사이트 언어 설정 바꾸면 서버는 설정한 언어에 대한 쿠키를 주고 
		- 다음에 방문할때 쿠키를 같이 넘겨주면서, 서버는 그 기억한 쿠키에 대한 데이터를 가지고 줄 응답을 고려해보겠지 


 - `Secure` Cooke : 쿠키가 오직 HTTPS 프로토콜을 통해 전송되어야 함
	 - 네트워크를 통해 전송될 때 도청(중간자 공격)으로부터 보호하기 위해 사용
 - `SameSite` Cookie 
	 - `Strict`: 같은 사이트에서만 쿠키를 전송합니다. 즉, 사용자가 직접 URL을 입력하거나 사이트 내의 링크를 클릭할 때만 쿠키가 포함됩니다.
	 - 사용자를 대신하여 자동으로 이루어지는 요청(예: CSRF 공격)에 대해 쿠키를 보호하기 위해 사용됩니다.
 - `httpOnly` Cookie :쿠키에 JavaScript를 통한 접근을 금지합니다.
	 - 쿠키 정보가 XSS 공격을 통해 탈취되는 것을 방지하기 위해 사용됩니다.


# Session
---
- Session  : 유저를 식별하기 위한 방법
	- 보통 인증 / 인가 할때 쓰이는데 
	- HTTP는 stateless라 모든 요청이 독립적이라 요청을 할때마다 인증을 해줘야함
	- 이를 해결하기 위한 방법으로 Session과  Token (JWT) 등장 

	- Session
		- 로그인하면 아디 비번 서버로 전송 HTTPS로 보내겠지 
		- 서버는 로그인이 성공하면 유저 식별자 같은 것을 Session DB에 저장 
			- 세션마다 별도의 ID 가 존재
		- 세션 아이디를 쿠키에 담아 보내고 브라우저에 저장시키지 
		- 그래서 요청을 보낼때마다 세션아이디를 쿠키에 담아 서버로 보내게되고 
			- 서버는 쿠키에 담긴 세션 아이디를 보고 유저를 식별 

		- => 모든 유저를 세션DB에 저장하니까 오버헤드가 커지겠지 


# JWT
---
- JWT
	- Session 방식으로 유저를 식별하는 것이 오버헤드가 커져서 이걸 해결하기 위해 등장
	- JWT 방식은 세션DB도 필요 없고 유저 인증을 위해 많은 일을 할 필요도 없음
	- JWT는 그냥 string임 서버에서 받아서 요청때마다 보내는 것은 똑같다. 

	- 로그인 성공한 유저 정보를 기반으로 JWT Token 생성하고 body 응답으로 준다. 
	- 요청 마다 JWT Token을 담아서 보내고 서버는 이 JWT Token이 유효한지 체크만 한다..



# Cookie로 전송 ?, Authorization Header로 전송 ?
---
CSRF 를 우려해서 Header 사용했던 것으로 기억

# Token은 Cookie에 ? LocalStorage에 ?
---
XSS가 더 치명적이라 판단해서 쿠키에 저장한다. 

RefreshToken은 samesite:strict, httponly,secure 쿠키 옵션을 사용해서 했고
accessToken은 즉시실행함수, 클로져 안에다가 저장해서 썼다.  (메모리에)

 

### Experience Auth
---
- 사용성보다 보안이 중요시 되는 회사였다. - 대기업 클라이언트들의 모든 매출 지표에 접근이 가능했으니까 
- CSRF보다 XSS가 치명적이라고 판단을 했다. 

- 따라서, 한시간의 유효시간 AccessToken과 , 세시간짜리 RefreshToken을 사용했고

- 로그인을 하면, 
	- AccessToken은 Body 와서, 클로져에다가 저장을 하고 
	- RefreshToken은 sameSite:string, secure, httpOnly 쿠키로 담겨져서 왔다. 
	  (XSS를 더 위협적이라 판단)
- Axios Interceptor에서 
	- HTTP Authorization 헤더에 Bearer 형식으로 토큰을 담아서 전송
- 401 응답으로 AccessToken이 만료되었다는 응답이 오면, axios interceptor에서 처리
	- 토큰 갱신 API를 호출하고 (어차피 쿠키에 담겨서 감)
	- 갱신성공하면 
		- 클로져에 다시 AccessToken을 담고, 실패한 요청을 다시 전송
	- 실패하면 로그인페이지로 이동


##### Token Manager : IIFE with Closure & 위의 예시 
---
- 토큰을 은닉하고 캡슐화하기 위해 IIFE로 사용

```js
import axios from 'axios';

const tokenManager = (function() {
  let accessToken = null; // 클로저 내부에서만 접근 가능한 변수 , IIFE

  function getAccessToken() {
    return accessToken;
  }

  function setAccessToken(token) {
    accessToken = token;
  }

  return {
    getAccessToken,
    setAccessToken,
  };
})();

// Axios 인스턴스 생성
const api = axios.create({
  baseURL: 'https://example.com/api',
});

// 요청 인터셉터 추가
api.interceptors.request.use(request => {
  const accessToken = tokenManager.getAccessToken(); // 클로저를 통해 토큰 얻기
  if (accessToken) {
    request.headers['Authorization'] = `Bearer ${accessToken}`;  
  }
  return request;
}, error => {
  return Promise.reject(error);
});

// 응답 인터셉터 추가
api.interceptors.response.use(
  response => response,
  async error => {
    const originalRequest = error.config;
    
    if (error.response.status === 401 && !originalRequest._retry) {
      originalRequest._retry = true;
      
      try {
        // 토큰 갱신 API 호출 (RefreshToken은 쿠키에 저장되어 있으므로) 자동으로 감
        const response = await api.post('/auth/refresh');
        const { accessToken: newAccessToken } = response.data;
        
        // 새 AccessToken으로 업데이트
        setAccessToken(newAccessToken);
        api.defaults.headers.common['Authorization'] = `Bearer ${newAccessToken}`;
        
         
        return api(originalRequest); // 원래 요청을 새 토큰으로 재전송
      } catch (refreshError) {
        // 토큰 갱신 실패 시, 로그인 페이지로 리다이렉트
        window.location.href = '/login';
        return Promise.reject(refreshError);
      }
    }
    return Promise.reject(error);
  }
);

export { api };

```