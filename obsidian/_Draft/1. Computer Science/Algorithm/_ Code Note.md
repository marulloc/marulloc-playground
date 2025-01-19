

# [1] n열 m행 2차원 배열 초기화

- 객체 배열로 초기화하면, 요소가 공유되기 때문에 새로운 객체 리터럴을 생성해서 할당해줘야한다.
	- 숫자로 초기화할땐 괜찮다.
	- fill을 쓰면 같은 값을 참조하기 때문에 객체로 초기화 할땐 쓰면 안된다.


#### 원시값 초기화 
- from은 콜백함수를 받아서 각 원소마다 동작을 수행할 수 있다는 차이가 있다 ?
- new Array도 하고 Array.from의 차이
```js
const init2DArray = (행개수, 열개수,초기화값) =>{
	return Array.from({length:행개수}, ()=> new Array(열개수).fill(초기화값))
}
```

#### 객체 초기화
```js
const init2DArray = (행개수, 열개수) => {
  return Array.from({ length: 행개수 }, (_, rowIdx) =>
    Array.from({ length: 열개수 }, (_, colIdx) => 
	    ({ row : rowIdx, col : colIdx }) // 초기화 값
	  ),  
  );
};
```


##### 나선형 채우기
https://school.programmers.co.kr/learn/courses/30/lessons/181832?language=javascript

