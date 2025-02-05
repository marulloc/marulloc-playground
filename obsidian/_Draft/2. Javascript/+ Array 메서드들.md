
# Array
---
### 원본 배열을 변경하는 메소드들

- **`push()`**: 배열의 끝에 하나 이상의 요소를 추가하고, 변경된 배열의 새로운 길이를 반환합니다.
- **`pop()`**: 배열의 마지막 요소를 제거하고, 그 요소를 반환합니다.
- **`shift()`**: 배열의 첫 번째 요소를 제거하고, 그 요소를 반환합니다.
- **`unshift()`**: 배열의 처음에 하나 이상의 요소를 추가하고, 변경된 배열의 새로운 길이를 반환합니다.

- **`splice()`**: 배열의 기존 요소를 삭제 또는 교체하거나 새 요소를 추가하여 배열의 내용을 변경합니다.

- **`sort()`**: 배열의 요소를 적절하게 정렬하고 배열을 반환합니다.
- **`reverse()`**: 배열의 요소 순서를 반전합니다.
- **`fill()`**: 배열의 시작 인덱스부터 끝 인덱스까지 정적인 값 하나로 채웁니다.

### 원본 배열을 변경하지 않는 메소드들

- **`concat()`**: 여러 배열을 결합하여 새 배열을 반환합니다.
- **`join()`**: 배열의 모든 요소를 연결해 하나의 문자열로 만들고 반환합니다.

- **`slice()`**: 배열의 일부분에 대한 얕은 복사본을 새 배열 객체로 반환합니다.
 
- **`filter()`**: 주어진 함수의 테스트를 통과하는 모든 요소로 이루어진 새 배열을 반환합니다.
- **`map()`**: 호출된 함수의 결과로 생성된 새 배열을 반환합니다.
- **`reduce()`**, **`reduceRight()`**: 배열의 각 요소에 대해 주어진 리듀서(reducer) 함수를 실행하고, 하나의 결과값을 반환합니다.


- **`some()`**: 원소중 하나라도 판별 함수를 통과하는지 테스트하고, true/false
- **`every()`**:  모든 요소가 주어진 판별 함수를 만족하는지 테스트하고, true/false
- **`includes()`**: 배열이 특정 요소를 포함하고 있는지 판별하고, 그에 따라 true/false
- **`find()`**: 판별 함수를 만족하는 첫 번째 요소의 값을 반환.  없다면 `undefined`
- **`findIndex()`**:  판별 함수를 만족하는 배열의 첫 번째 요소의 인덱스를 반환/  없으면 `-1`

- **`indexOf()`**: 배열에서 지정된 요소를 찾을 수 있는 첫 번째 인덱스를 반환합니다. 없으면 `-1`을 반환합니다.
- **`lastIndexOf()`**: 배열에서 지정된 요소를 찾을 수 있는 마지막 인덱스를 반환합니다. 없으면 `-1`을 반환합니다.



# Slice / Splice
---

##### Slice, 중간 추가 할때 사용
```ts
const origin = [1,2,3]

const insert = (index:number, target:number)=>{
	return [ ...origin.slice(0,index), target, ...origin.slice(index)]
}
```



##### Splice
원본 변경하면서

원하는 인덱스에서부터 n개를 지우거나
원하는 인덱스에서부터 n개를 지우고 m개의 원소를 추가 할 수있음
```js
const fruits = ['apple', 'banana', 'cherry', 'date']; 
// 인덱스 2번 부터 2개의 요소(cherry, date)를 제거하고, 'grape', 'kiwi'를 삽입 
const removedElements = fruits.splice(2, 2, 'grape', 'kiwi'); console.log(fruits); // 원본 배열 : ['apple', 'banana', 'grape', 'kiwi'] 
console.log(removedElements); // 삭제된 요소 배열 : ['cherry', 'date']
```



# String
---
- `"1".padStart(3,"0") ====> 001`
- `"1".padEnd(3,"0") ====> 100`
