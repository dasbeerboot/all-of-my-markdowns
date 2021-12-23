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
   }

   // after
   const name = "juwon"
   const member = {
     name,
     age: 28,
     address: "seoul",
   }

   console.log(member) //{name: "juwon", age: 28, address: "seoul"}
   ```

2. 계산된 속성명(Computed Property Name)

   - 대괄호[] 안에 변수, 혹은 식을 대입하여 **동적**으로 객체 속성을 생성 및 할당할 수 있다.

   ```javascript
   const name = "juwon"
   const city = "seoul"
   const member = {
     [name]: 28,
     [city]: "sadang",
   };

   console.log(member) //{juwon:28, seoul: 'sadang'}
   ```

3. 비구조화 할당 (Destructuring)
   - 배열 비구조화
   > 배열 비구조화는 index값을 기본으로 한다.
    ```javascript
    const arr = [1, 2]
    const [a,b] = arr
    console.log(a) // 1
    console.log(b) // 2

    // 요소 건너뛰기
    const arr = [1, 2, 3]
    const [a, ,c] = arr;
    console.log(a) // 1
    console.log(c) // 3

    //Spread 연산자를 이용한 나머지배열 생성
    const arr = [1, 2, 3]
    const [a, ...rest] = arr
    console.log(a) // 1
    console.log(rest) // [2, 3]
    ```
   - 객체 비구조화
   ```javascript
    const member = {age: 28, name:'juwon'}
    const {age, name} = member;
    console.log(age) // 28
    console.log(name) // juwon

    // 별칭 사용 + API 호출시 예제
    const {result: myData, refetch: refetchMydata} = useCreatorQuery({fetchPolicy: 'network-only'})
    console.log(myData) // useCreatorQuery 의 result 값
    console.log(result) // undefined
    ```

<br/>

### 그렇다면 실제 프로젝트에서 데이터바인딩 할때, 어떻게 효율적으로 쓸 수 있을까? (오답노트)
 우선 GraphQL 뮤테이션을 보낼때 내가 한 실수를 알아보자.
 ```javascript
 const {mutate: createCreator, error: createCreatorError} = useCreateCreatorMutation({})

 const firstStepRef = ref({
  marketing: false,
  precaution: true,
  regulation: true,
  terms: true,
})

const secondStepRef = ref({
  introduction: '',
  nickname: '',
})

const thirdStepRef = ref([
  {address: '', type: CreatorSnsType.Twitter},
  {address: '', type: CreatorSnsType.Instagram},
  {address: '', type: CreatorSnsType.Facebook},
])

const fifthStepRef = ref<Array<CreatorAccessChannelCreateInput>>([])

const onCreateCreator = () => {
  createCreator({
    input: {
      agreeItemMarketingFlag: firstStepRef.value.marketing,
      agreePrecautionsFlag: firstStepRef.value.precaution,
      agreeRegulationFlag: firstStepRef.value.regulation,
      agreeTermsFlag: firstStepRef.value.terms,
      creatorAccessChannels: fifthStepRef.value,
      creatorSns: thirdStepRef.value,
      introduction: secondStepRef.value.introduction,
      nickname: secondStepRef.value.nickname,
    },
  }).then(() => {
    isFinalAlertOpen.value = true
  })
}
 ```
 ~~... 그만 알아보자~~  
 크리에이터 생성의 각 단계별로 컴포넌트와 ref를 나눠 해당 컴포넌트에 props로 ref를 넘겨주는 방식으로 객체의 값을 관리하도록 했다.  
 위 코드에서 GraphQL 뮤테이션을 보낼때, 단축속성명을 익숙하게 사용했다면 더 간단하게 작성 가능했을 코드가 몇 부분(~~단 두줄~~) 보인다.  
 바로 `creatorAcessChannels`와 `creatorSNS`다.  
 단축속성명을 사용한다면 아래와 같이 바꿀 수 있겠다.  
 ```javascript
 const {mutate: createCreator, error: createCreatorError} = useCreateCreatorMutation({})

 const firstStepRef = ref({
  marketing: false,
  precaution: true,
  regulation: true,
  terms: true,
})

const secondStepRef = ref({
  introduction: '',
  nickname: '',
})

// 요기!
const creatorSns = ref([
  {address: '', type: CreatorSnsType.Twitter},
  {address: '', type: CreatorSnsType.Instagram},
  {address: '', type: CreatorSnsType.Facebook},
])

// 요기!
const creatorAccessChannels = ref<Array<CreatorAccessChannelCreateInput>>([])

const onCreateCreator = () => {
  createCreator({
    input: {
      agreeItemMarketingFlag: firstStepRef.value.marketing,
      agreePrecautionsFlag: firstStepRef.value.precaution,
      agreeRegulationFlag: firstStepRef.value.regulation,
      agreeTermsFlag: firstStepRef.value.terms,
      // 요기!
      creatorAccessChannels,
      creatorSns,
      introduction: secondStepRef.value.introduction,
      nickname: secondStepRef.value.nickname,
    },
  }).then(() => {
    isFinalAlertOpen.value = true
  })
}
 ```
 