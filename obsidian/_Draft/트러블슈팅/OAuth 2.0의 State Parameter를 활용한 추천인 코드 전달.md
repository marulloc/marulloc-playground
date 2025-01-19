## **문제 개요**

소셜 로그인 과정에서 **inviteCode**(추천인 코드)가 백엔드에 전달되지 않아, 추천인 보상 기능(추천인과 피추천인에게 각각 10,000포인트 지급)이 동작하지 않는 문제가 발생했습니다. 이를 해결하기 위해 OAuth 2.0의 state 파라미터를 활용한 과정을 공유합니다.

## **초기 구현 시도**

### 1. 추천인 코드 전달 방법

- 로그인 프로바이더(Kakao, Naver)로 리디렉션 요청 시 inviteCode를 쿼리스트링에 포함하여 전달:
    

```
https://nid.naver.com/oauth2.0/authorize?redirect_uri=http%3A%2F%2Flocalhost%3A3001%2Fapi%2Fauth%2Fcallback%2Fnaver&inviteCode=추천인코드
```

- 리다이렉트 후 NextAuth의 signIn 콜백에서 inviteCode를 추출하여 백엔드로 전달하려고 시도.
    

### 2. 발생한 문제

- 리디렉션 시 inviteCode가 포함된 쿼리스트링만이 잘려서 전달되지 않음.
    
- NextAuth signIn 콜백에서 inviteCode를 읽지 못하고 여전히 추천인 코드는 동작하지 않았음
    
- Kakao Developers 문서에서 이와 관련된 제약 사항을 확인:
    
    - redirect_uri에는 OAuth 2.0 표준에 따라 허용된 파라미터만 전달 가능.
        
    - 추가적인 파라미터 전달이 필요할 경우 state 파라미터 또는 쿠키 등을 사용해야 함.
        

```
인가요청 후, 서비스측 redirect_uri로 리디렉션 될 때, 가이드의 파라미터를 제외하고는 모두 제거됩니다.
이는 OAuth 2.0 표준 스펙에 따른 처리입니다.
```

## **해결 과정**

### **1. Kakao Developers에서 같은 사례 확인**

- 먼저 [Kakao Dev Talk](https://devtalk.kakao.com/t/nextauth-signin-additional-query-param-signin-callback/133802)에 문의내용들을 추적.이 과정에서 redirect_uri에 허용되지 않은 파라미터는 제거된다는 사실을 확인. 추가적인 파라미터 전달이 필요할 경우 state 파라미터 또는 쿠키 등을 사용해야 함.
    
    ```
    인가요청 후, 서비스측 redirect_uri로 리디렉션 될 때, 가이드의 파라미터를 제외하고는 모두 제거됩니다.
    이는 OAuth 2.0 표준 스펙에 따른 처리입니다.
    ```
    
- 그러나 state 파라미터의 의미를 여전히 이해하지 못했음  
    

### **2. NextAuth 도큐먼트 탐색**

- [도큐먼트](https://next-auth.js.org/getting-started/client#additional-parameters) 내에 state 파라미터에 대한 설명은 없었지만  
    signIn 함수의 세 번째 인자를 활용하면 state 파라미터를 설정할 수 있다는 점을 발견
    
    ```
    signIn("auth0", null, { login_hint: "info" });
    ```
    
- 이렇게 호출한 경우 프로바이더에게 전달되는 redirect_uri의 쿼리스트링으로  
    아래 세번째 인자(객체)가 key=value 형태로 직렬화되어 전달됨
    
    ```
    redirect_uri={리다이렉트 주소}%2Fcallback%2Fnaver&login_hint=info
    ```
    
- 그러나 여전히 리디렉션 이후 login_hint와 같은 쿼리스트링은 삭제되어 리디렉션됨.
    

### **3. RFC 문서 확인**

- state 파라미터에 대한 이해도를 위해 [RFC문서의 OAuth2.0 부분](https://datatracker.ietf.org/doc/html/rfc6749#section-4.1.1)을 탐색
    
    ```
    An opaque value used by the client to maintain state between the request and callback.
    The authorization server includes this value when redirecting the user-agent back to the client.
    The parameter SHOULD be used for preventing cross-site request forgery as described in Section 10.12
    ```
    
- 말 그대로 key값이 state인 파라미터를 의미했음
    
- state 파라미터가 **클라이언트-인증서버 간 데이터 유지**(원래 목적은 CSRF 공격 방지)를 위한 표준 방식임을 이해.
    
    - 카카오와 같은 프로바이더에서도 이 규격을 통해 통신하고 있음을 파악하고 state를 key값으로하고 데이터를 직렬화하여 전송하니, 실제로 리디렉션 이후 데이터가 유지됨을 확인 (아래는 예시)
        
        ```
        redirect_uri={리다이렉트 주소}%2Fcallback%2Fnaver&state='{"inviteCode":"ABCDEFG"}'
        ```
        

### **4. 카카오/네이버의 동작 차이로 인한 에러 발견 및 문제 해결 완료**

- state 파라미터를 사용하면 리디렉션 이후 데이터가 유지됨을 확인했으나, 에러를 발견
    
    - 리디렉션 이후 NextAuth signin 콜백에서 state 파라미터의 값을 파싱하는 과정에서 에러 발생
        
    - 프로바이더마다 state 파라미터가 다른 형태로 인코딩되어 전달되고 있었음
        
    - **Kakao**: state로 전달된 객체를 **URL 인코딩** 처리.  
        **Naver**: state로 전달된 객체를 **HTML 엔티티 인코딩** 처리.
        
- 인코딩 방식 차이를 해결하기 위해 state로 전달되는 데이터를 **Base64**로 인코딩 및 디코딩
    
    - 객체가 프로바이더에서 따로 직렬화되는 것을 방지하기 위해  
        state 파라미터의 값으로 담아 보낼 객체를 Base64로 인코딩하여 문제 해결
        
        ```
        // 인코딩
        const stateValue = Buffer.from(JSON.stringify({ inviteCode: "추천인코드" })).toString("base64");
        signIn("naver", null, { state: stateValue });
        
        // 디코딩
        const decodedState = JSON.parse(Buffer.from(state, "base64").toString("utf-8"));
        ```
        

## **결론**

- OAuth 2.0의 state 파라미터는 CSRF 방어와 데이터 유지에 사용된다.
    
- OAuth에서 프로바이더와 리디렉션 이후 데이터 유지를 원한다면 state를 활용
    
- 플랫폼 간 인코딩 방식 차이에 유의하여 일관된 방식(Base64)을 적용하면 문제를 해결
    
- 참고자료
    
    - [https://devtalk.kakao.com/t/nextauth-signin-additional-query-param-signin-callback/133802/6](https://devtalk.kakao.com/t/nextauth-signin-additional-query-param-signin-callback/133802/6)
        
    - [https://next-auth.js.org/getting-started/client#additional-parameters](https://next-auth.js.org/getting-started/client#additional-parameters)
        
    - [https://datatracker.ietf.org/doc/html/rfc6749#section-4.1.1](https://datatracker.ietf.org/doc/html/rfc6749#section-4.1.1)