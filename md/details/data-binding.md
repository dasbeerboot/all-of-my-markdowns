# ES6의 향상된 객체 문법과 더 효율적인 데이터핸들링

### 객체 문법, 뭐가 추가된거지?

1. 단축 속성명(Shorthand Property Name)

   - 객체 속성값으로 사용될 변수가 미리 정의되어 있을 때, 해당 변수명으로 속성의 키와 값을 한 번에 정의할 수 있다.

   ```javascript
   //before
   const member = {
     name: "juwon",
     age: 28,
     address: "seoul",
   };

   // after
   const name = "juwon";
   const member = {
     name,
     age: 28,
     address: "seoul",
   };

   console.log(member); //{name: "juwon", age: 28, address: "seoul"}
   ```

2. 계산된 속성명(Computed Property Name)

   - 대괄호[] 안에 변수, 혹은 식을 대입하여 **동적**으로 객체 속성을 생성 및 할당할 수 있다.
   - class값을 동적을 쓰고싶을때 쓸 수 있음!!

   ```javascript
   const name = "juwon"
   const city = "seoul"
   const member = {
     [name]: 28,
     [city]: "sadang",
   }

    console.log(member) //{juwon:28, seoul: 'sadang'}

    const foo = true
    const bar = true
    const example =  <div class={
     foo bar
    }></div>
   ```

3. 비구조화 할당 (Destructuring)

   - 배열 비구조화
     > 배열 비구조화는 index값을 기본으로 한다.
     > promise all 할때 많이 쓰임!

   ```javascript
   const arr = [1, 2];
   const [a, b] = arr;
   console.log(a); // 1
   console.log(b); // 2

   // 요소 건너뛰기
   const arr = [1, 2, 3];
   const [a, , c] = arr;
   console.log(a); // 1
   console.log(c); // 3

   //Spread 연산자를 이용한 나머지배열 생성
   const arr = [1, 2, 3];
   const [a, ...rest] = arr;
   console.log(a); // 1
   console.log(rest); // [2, 3]
   ```

   - 객체 비구조화

   ```javascript
   const member = { age: 28, name: "juwon" };
   const { age, name } = member;
   console.log(age); // 28
   console.log(name); // juwon

   // 별칭 사용 + API 호출시 예제
   const { result: myData, refetch: refetchMydata } = useCreatorQuery({
     fetchPolicy: "network-only",
   });
   console.log(myData); // useCreatorQuery 의 result 값
   console.log(result); // undefined
   ```

<br/>
