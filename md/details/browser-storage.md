# 브라우저 저장소의 종류와 용도

브라우저 저장소(Web Storage)란 도메인 단위로 해당 도메인과 관련된 특정 데이터들을 클라이언트 웹브라우저에 저장하는 기능이다.  
데이터는 Key와 Value 쌍으로 저장되된다.  
브라우저 저장소의 종류는 영구적으로 저장되는 Local Storage와 임시 저장되는 Session Storage로 나뉘어져 있어,
데이터의 지속성을 구분하여 사용할 수 있다.

> **쿠키와 브라우저 저장소의 차이점**  
> 쿠키: 4kb 데이터 제한, 모든 HTTP 리퀘스트에 포함되어 매번 서버로 전송됨
> 브라우저 저장소: 용량 제한 X, 클라이언트에 존재할 뿐 서버로 전송되지 않음, 영구적인 데이터 저장 가능, object 저장 가능  
> <br/> \*\*\* Persistent Cookie 의 경우에는 지정된 기간동안, 혹은 장치에서 쿠키를 수동으로 삭제할 때 까지 남는다.

<br/>

## 브라우저 저장소의 용도

- 장바구니, 좋아한 상품 등의 저장
- 작성중인 글의 임시저장
- 방문자 이동경로의 저장
- 기타 굳이 서버사이드에 저장될 필요가 없는 데이터 등

## Local Storage와 Session Storage의 비교

두 저장소 모두 도메인마다 별도로 생성되며 `windows` 전역 객체의
`LocalStorage`, `Session Storage`라는 컬렉션을 통해 wirte & read가 이루어진다.
하지만 Local Storage는 웹 어플리케이션이 실행된 탭을 종료하거나 브라우저를 닫았다가 다시 열어도 저장된 데이터가 유지되는 반면,  
Session Storage 는 웹 페이지의 세션이 끝날 때, 즉 탭을 종료하거나 브라우저를 닫을때 저장되어 있던 데이터가 지워진다.  
같은 브라우저에서 여러개의 탭을 띄워 사용할때, 도메인이 같을 경우, 로컬 스토리지는 여러 탭 간에 데이터가 전역으로 공유된다.

## 기본 API

앞서 설명한 것 처럼 `windows`객체를 이용해 접근해야 하지만, 줄여서 `localStorage`, `sessionStorage`로도 접근 가능하다.  
기본적으로 사용되는 API는 아래와 같다.

```javascript
// write
localStorage.setItem('key', value)

// read
localStorage.getItem('key')

// delete
localStorage.removeItem('key')

// delete all
localStorage.clear()

// get length of saved key,value pair
localStorage.length
```

<br/>

**객체를 저장하거나 불러오고 싶을때는?**

브라우저 저장소의 value는 문자형 데이터 타입의 저장만 지원하므로, JSON을 스트링으로 파싱하거나,
스트링으로 파싱되어 저장된 JSON을 읽어와야하므로 `JSON.parse()` 및 `JSON.stringify()` 구문을 이용해 데이터를 저장하고 불러와야한다.  
<br/>
예)

```javascript
// write
sessionStorage.setItem('key', JSON.stringify({a: 1, b: 2}))

// read
JSON.parse(sessionStorage.getItem('key')
```
