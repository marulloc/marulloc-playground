https://velog.io/@kskim625/%ED%9A%A8%EC%9C%A8%EC%A0%81%EC%9D%B8-%EC%95%A0%EB%8B%88%EB%A9%94%EC%9D%B4%EC%85%98-CSS-vs.-JavaScript
# CSS Animation  
---
> CSS로 애니메이션을 만드는 방법은 "transition"과 @keyframe을 이용한 "animation" 이 있다.

`transition`은 간단한 인터랙션에 적합하며, 
`@keyframes`와 `animation`은 보다 복잡한 애니메이션을 구현할 때 유용합니다.
조금 더 복잡하다면 Javascript로 애니메이션을 만들어야되지 않나..


### Transition
- `transition : [Property] [duration] [timing-function] [delay]` 형태
	- 여러개 속성에대해 애니메이션 주려면 , 로 저 문장을 여러번 쓰면 된다.
- 보통 간단한 이동, 호버, 모달 같은 것에 쓰면 됨 
```css
.box {
  width: 100px;
  height: 100px;
  background-color: red;
  transition: background-color 0.5s ease-in-out, transform 0.5s ease-in-out;
}

.box:hover {
  background-color: blue;
  transform: translateX(100px);
}

```

### @Keyframe Animation
- 애니메이션의 중간상태를 정해두고 선택자에 할당하여 애니메이션 만들 수 있다.
- 중간 상태는 `from-to` 혹은 `0~100%` 로 결정할 수 있다. 
	- 빈 중간상태는 속도곡선에 따라서 진행된다.

- animation-name : @keyframe 이름
- animation-duration : 한번 실행되는데 걸리는 시간
- animation-iteration-count : 반복횟수
- animation-timing-function : 속도곡선
- animation-delay : 딜레이시간
- animatioon-direction : 재생방향 `normal | reverse | alternate`

```css
@keyframes example {
  from {background-color: red;}
  to {background-color: yellow;}
}

div {
  animation-name: example;
  animation-duration: 2s;
  animation-iteration-count: infinite;
  animation-direction: alternate;
}
```

##### 장점
- **성능**: CSS 애니메이션은 브라우저가 GPU(그래픽 처리 장치)를 활용하여 애니메이션을 렌더링 하므로, 복잡한 애니메이션에서도 더 좋은 성능을 제공한다.
- 또한, **transform**과 **opacity** 속성을 변경하는 애니메이션이나 `will-change` 속성을 사용한 요소들은
- 그래픽스(컴포지팅) 레이어로 분리되기 때문에 
- 컴포지팅 쓰레드가 GPU를 활용하여 독립적으로 빠르게 렌더링하기 때문에  매끄럽게 표현되고
  다른 요소들의 Reflow와 Repaint를 유발하지 않으며
  메인쓰레드를 점유하지 않기 때문에 메인쓰레드는 다른 작업을 계속 할 수 있다. 
  - 그러나 너무 많은 레이어를 만들면 GPU 메모리 사용량을 증가시킬 수 있다.

##### 단점
- 유저 액션과 복잡한 상호작용을 다루기 어렵다.



# Javascript Animation
---

Request Animation Frame / setInterval()


`transform` 의 `translate` 를 사용해서 구현할 수 있는 애니메이션을 JavaScript의 `style.top` 과 `style.left` 속성을 변화 시키게 되면  
  
브라우저 렌더링 과정에서 layout 이나 paint 단계를 거쳐야 할 경우가 생길 수 있기 때문에 성능 개선에 효율적이지 않을 수 있습니다.


- 애니메이션을 세밀하게 제어해야 하는 경우 JS를 사용 합니다.
	- Reflow와 Repaint가 연속적으로 발생할 가능성이 있어 성능이 좋지 않을 수 있다.
	- 그러나 RequestAnimationFrame을 사용하면 이를 해결 할 수 있다.
    
- 크로스 브라우징 측면에서 JS 애니메이션을 사용하는 것이 유리 합니다.(브라우저 호환성이 좋다.)
    
- GPU를 통한 하드웨어 가속을 제어할 수 있씁니다. CSS 애니메이션의 경우 특정 속성에 의한 GPU가속이 됨으로서 저사양의 컴퓨팅인 경우에 성능 하락을 발생시킬 수 있으나 이를 막을수 있습니다.



### Request Animation Frame
그 주사율에 맞춰 애니메이션 관리해준다는데 
알기로 repaint 전에 애니메이션을 그려준다는 것으로 알고 있고

브라우저는 화면을 16ms 60프레임에 맞춰서 그리는데
requestAnimationFrame은 별도의 TaskQueue에 담기는데 

원래 이벤트루프가 16ms/60프레임 정도로 동작한다고 알긴하지만
이 requestAnimation Frame Queue가 가장 우선순위를 가지고 
이벤트루프가 한번 체킹할때 큐 안에 모든 리퀘스트 프레임이 
리페인트 전에 실행한다고 들었다.