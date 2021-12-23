# 배열을 더 자유롭게 다루기 위하여

배열은 언제나 어렵다. 그래서 항상 쓰는 것 만 쓰게된다. 예를 들어 map이라거나 .. ~~map이라거나 또 map...~~  
이번 글에서는 배열을 능수능란하게 까지는 아니더라도 조금 더 자유롭게 다루기 위해 Array 객체의 여러 메소드들을 정리해보고자 한다.  


## 자주 사용되는 Array 객체의 대표적인 메소드를 알아보자

### 1. map과 forEach
- **map**: 기존의 배열을 이용해 **새로운 배열을 생성할 때** 사용된다. 
콜백 함수를 인자로 받아, 각 요소에 대한 해당 함수를 실행하고 결과값을 반환한다.  
메모리를 할당하고 새로운 값을 만들어서 return할 수 있고, return 된 값 자체(새로운 배열)를 반환한다.  
나는 리액트에서 컴포넌트에 값을 동적으로 할당해주고 화면에 렌더하도록 구현할 때 map을 주로 사용한다.
```javascript
const arr = [1, 2, 3, 4, 5]
const arrPlusOne = arr.map((item) => {
    return item + 1
})
console.log(arr) // [1, 2, 3, 4, 5]
console.log(arrPlusOne) // [2, 3, 4, 5, 6]

// react에서 이렇게!
{
    arr.map((item) => {
        return <span>{item}</span>
    })
}
```
- **forEach**: 단순히 배열을 순환하는 메소드이다. Map과는 다르게 forEach는 아무것도 리턴하지 않으므로 구문 밖으로 return 값을 받지 못한다.  
return값을 반환하지 않다보니, 컴포넌트를 렌더할때는 사용할 수 없다.
```javascript
const arr = [1, 2, 3, 4, 5]
arr.forEach((item) => {
    console.log(item) // 1 2 3 4 5
})
```

### 2.  sort 잘 쓰는 법
- sort는 파라미터로 **비교함수**를 넘겨받아 배열 요소들을 sorting한다. sort와 비교함수는 아래와 같이 쓰인다.  
*** sort는 새로운 배열을 만들지 않고, 원본 배열 자체의 값을 변경시키기 때문에 주의해야함!
```javascript
const arr = [2, 0, 4, 3, 1]
arr.sort((a, b) => {
    return a - b
})
console.log(arr) // [0, 1, 2, 3, 4] 오름차순 정렬

const arr = [2, 0, 4, 3, 1]
arr.sort((a, b) => {
    return b - a
})
console.log(arr) // [4, 3, 2, 1, 0] 내림차순 정렬
```
나는 이 오름차순/내림차순 한글뜻이랑 a-b, b-a가 매치가 안돼서 맨날 헷갈린다 ㅠㅠ  

그렇다면 number타입의 요소를 가질 때 말고, string값을 요소로 가질때는 어떻게 정렬해야할까?
간단하다. 오름차순은 sort(), 내림차순은 reverse()를 이용하면 된다.
```javascript
const arr = ['d', 'a', 'e', 'c', 'b']
arr.sort()
console.log(arr) // ['a', 'b', 'c', 'd', 'e'] 오름차순 정렬

arr.reverse()
console.log(arr) // ['e', 'd', 'c', 'b', 'a'] 내림차순 정렬
```
엥 그렇다면 object는??


### 3. find, filter와 some
세 메소드 모두 배열 내부를 순환하며 콜백 함수를 실행하며 그 결과를 반환하는 하는 함수이다.
- **find**: 주어진 조건을 만족하는 **제일 첫 번째 요소**를 반환하는 함수  
- **filter**: 주어진 조건을 만족하는 요소를 담은 **새로운 배열**을 반환하는 함수
- **some**: 주어진 조건을 만족하는 배열의 요소가 있으면 *true*, 없으면 *false*를 반환하는 함수
```javascript
const members = [
    {name: '주원', age: 28},
    {name: '민수', age: 28},
    {name: '광후', age: 29},
    {name: '재은', age: 26},
    {name: '소연', age: 27}
]

const theYoungest = members.find((item) => item.age === 26)
console.log(theYoungest) // {name: '재은', age: 26}

const sameAgeFriends = members.filter((item) => {
    if (item.age === 28) {
        return item.name
    }
})
console.log(sameAgeFriends) // ['주원', '민수']

const isTwenty = members.filter((item) => item.age === 20)
console.log('우리중에 스무살인 멤버가 있다? :', isTwenty) // '스무살인 멤버가 있다?' false
```

### 4. reduce
### 5. entries