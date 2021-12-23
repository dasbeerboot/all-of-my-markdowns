# 좋은 컴포넌트란?

좋은 리액트 코드를 작성하기 위해서는 신경써야 될 게 한두가지가 아니다. 아키텍쳐부터 상태관리,
디렉토리 설계 및 코드 포매팅까지 염두에 둬야 할 것들이 많다.  
그렇지만 결론적으로, 리액트 어플리케이션은 결국 팀원들과 협업하여 작성하고 번들링이 되어 나온 컴포넌트들의 묶음이다.
그러므로 좋은 리액트 코드를 작성하기 위해서는, 클린하고 간결한 컴포넌트를 작성해야한다.  
물론, 아키텍쳐를 신경쓰지 않고 컴포넌트만 클린하고 간결하게 작성한다고 좋은 어플리케이션이 탄생하는 건 아니지만,
컴포넌트를 neat하게 작성하면, 좋은 아키텍트를 만들기 더 쉬워진다.  
그러므로 이제부터 좋은 컴포넌트를 작성하는 법을 알아보자.

<br/>

## 1. 함수형 컴포넌트를 작성하라

**함수형 컴포넌트**란 말 그대로 함수를 기반으로 작성하는, 함수형 프로그래밍을 가능하게 하는 컴포넌트이다.

> 함수형 프로그래밍: 순수한 함수들을 조합하여 소프트웨어를 만드는 개발 방법론

컴포넌트가 가진 가장 중요한 역할을 바로 세그먼트를 화면에 렌더하는 render method에 있지만,
함수형이 아닌 클래스형으로 컴포넌트를 작성하게 되면, 우리는 종종 이 사실을 잊게되고,
컴포넌트 하나가 가진 기능이 너무 많고 복잡해지면 사용성이 떨어지고 유지보수 또한 힘들어진다.  
그 결과로 너무 많은 props를 내려받으며 너무 크고 유추하기 어려운 컴포넌트가 탄생하게된다.  
그러므로 컴포넌트를 함수형으로 작성하는 것 이 좋다.

<br/>

## 2. 좋은 함수를 작성하는 방법

함수형 컴포넌트를 작성하기로 마음 먹었으면, 이제는 좋은 함수를 작성해야한다.  
Rober Martin의 *Clean Code*를 다섯줄로 요약해 대입해보면, 좋은 컴포넌트란

1. 작고,
2. 오직 한 가지 기능만 하며,
3. 오직 하나의 추상화만을 가지며,
4. 세 개를 넘지 않는 인수(argument)를 갖고있는,
5. 직관적인 이름을 가진

함수라고 볼 수 있다.

> 인수(argument)와 매개변수(parameter) 의 차이: 인수는 함수를 **호출할 때** 사용하게 되는 일련의 값들을 말하며,
> 매개변수는 함수를 **정의할 때** 외부로부터 받아들이는 임의의 값을 말한다.  
> 매개변수:
>
> ```typescript
> const example = (x: number, y: string) => {
>   return x, y
> }
> ```
>
> 위 코드에서 매개변수는 x,y이며  
> <br/>
>
> ```typescript
> example(1, '2')
> ```
>
> 위 코드에서 인수는 1과 '2'이다.

## 3. 컴포넌트는 작아야한다.

작은 컴포넌트는 읽기 용이하다. 500줄 짜리 캄포넌트의 코드를 읽고, 이해하고, 사용하는 것 보다
50줄 내외의 컴포넌트 이해하는 편이 더 빠르고 효율적일 것이다.  
함수형 컴포넌트를 작성할 때 100줄 이내, 아무리 길어도 250줄 이내의 코드를 작성하도록 노력해보자.

<br/>

## 4. 컴포넌트는 한 가지 일만 해야한다.

아래 여러개의 추상화를 가진 psuedo code 예제를 보자.

```javascript
const loadThings = async () => {
  setIsLoading(true)
  const response = await fetchThings()
  setIsLoading(false)
  const { error, data } = response
  if (error) {
    if (error.status === 404) {
      redirectTo('/404')
    } else if (error.status === 500) {
      redirectTo('/error')
    }
  } else {
    const thingsToUpdate = data.ids.reduce((map, id) => {
      map[id] = data.things[id]
      return map
    }, {})
    updateThingsInState(thingsToUpdate)
  }
}
```

위 코드의 `loadThings`함수의 `setIsLoading()`함수는 다른 함수에의 의존성을 가지고 있다.  
(로딩 상태를 관리하고 서버의 응답값을 fetching하는 기능) 하지만 위 함수의 다른 요소들은 그렇지 않다.  
이 상태에서 더 클린한 코드를 짜려고 하면 아래와 같이 작성할 수 있다.

```javascript
const handleResponse = (response) => {
  const { error, data } = response
  if (error) {
    handleError(error)
  } else {
    updateThingsInState(data)
  }
}
const loadThings = async () => {
  setIsLoading(true)
  const response = await fetchThings()
  setIsLoading(false)
  handleResponse(response)
}
```

<br/>

## 5. 순수함수(순수 컴포넌트)를 사용하자.

리액트에서 순수 컴포넌트는 `shouldComponentUpdate`를 내부에서 다뤄준다.
`prop`혹은 `state`가 변경될 때만 얕은 비교를 수행해 리렌더 한다는 얘기다.  
반면 일반 컴포넌트는 바뀔 값과 현재값을 비교해주지 않으므로 매번 컴포넌트가 변경될때마다 불필요한 리렌더링을 수행하게 된다.

> **순수함수란?** 리턴상태로 결과값을 만드는 것 이외에 외부의 상태에 영향을 미치지 않는,
> 다시말해 부수효과가 없고, 동일한 인자를 주면 평가 시점에 상관없이 항상 동일한 결과를 리턴하는 함수.
> 이는 객체 메소드가 객체의 상태(객체 멤버)와 상호작용하도록 설계되는 OOP와 대비되며,  
> 외부 상태가 함수 내에서 조작되는 경우가 많은 절차적 스타일 코드와도 대비된다.  
> 그러나 리액트(React)의 useEffect 후크에서 볼 수 있듯이 실제 환경에서 함수는 더 넓은 컨텍스트와 상호작용해야 하는 경우가 많다.

순수함수에서 인자로 넘겨받은 object의 값을 변경해야 할 때는, 원래 있던 값은 그대로 두고,
새로운 객체를 깊은 복사 한 후 원하는 부분의 값이 변형된 새로은 값으로 리턴하는 식으로 함수를 만들어야 한다.

순수함수는 평가시점이 중요하지 않다. 어디서 어떻게 불러도, 같은 인자를 넘기면 같은 값이 나오는 함수이기 때문이다.  
때문에 평가시점을 개발자가 자유롭게 다룰 수 있고 조합성이 강조된다.  
함수형 프로그래밍의 컨셉이 순수함수를 통해 조합성을 강조하는 프로그래밍 패러다임이기에 되도록 순수함수를 작성하여 사용하도록 하자.

<br/>

## 6. 일급함수를 사용하자.

> **일급함수란?** 함수를 변수에 담아 값으로 다루거나, 다른 함수에 파라미터로 전달하고 반환받는 것. 아래와 같이 사용될 수 있다.
>
> ```javascript
> let count = function (x) {
>   return x++
> }
> ```