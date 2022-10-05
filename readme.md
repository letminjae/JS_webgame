# Let's Get it Javascript!
### `ZeroCho`님의 `Let's Get it Javascript` 책을 통해 자바스크립트를 이용한 웹게임을 만든 레포지토리 입니다.

<br />

### 단순히 프로그래밍 언어만 익히는 것이 아닌, `프로그래밍 사고력`을 길러주기 위한 목표를 향해 배웁니다. 

## 쿵쿵따
### 자바스크립트의 기본을 알고, 순서도를 이용해 로직 구현

<img src='https://user-images.githubusercontent.com/96935557/193987113-559bd241-cc7f-488f-836c-a50feb5ae7da.gif'>

- `document.querySelector()` - HTML 태그를 `선택` 하는 자바스크립트 문법
- `document.querySelectorAll()` - HTML 태그 모두를 선택하는 자바스크립트 문법
- `id와 class의 차이` - id는 요소 하나를 (상위) 지정하고 싶을때 (한번만 가지는 고유한 값), class는 여러 요소를 (하위) 지정하고 싶을 때 사용 (class 반에는 여러명이 있다고 생각)
- id는 선택할때 `'#id'`, class는 선택할때 `'.class'`로 작성
- `addEventListener('태그', 콜백함수)` : 파라미터 태그에 이벤트 리스너 생성. 이벤트 발생 시 실행코드 호출.
```js
    const $button = document.querySelector('#button');
    $button.addEventListener('click', function() {...});
```
- `event.target.value` - 파라미터인 event 객체에 target 객체의 value 값을 가져온다.
- `textContent` - HTML 태그에 text값을 넣어주는 문법

## 계산기
### 함수를 집중적으로 사용하는 계산기 로직 구현

<img src='https://user-images.githubusercontent.com/96935557/193988778-99e1b514-6e67-4a2d-adc9-399263342f15.gif'>

- 처음 숫자버튼을 클릭할 때 - operator(연산자) 변수가 비어있는가? 에서 조건문 출발
```js
  const onClickNumber = (event) => {
        if(!operator) { 
            numOne += event.target.textContent;
            $result.value += event.target.textContent;
            return;
        }
        if (!numTwo){
            $result.value = '';
        }
        numTwo += event.target.textContent;
        $result.value += event.target.textContent;
    }
```
- `고차 함수 (High Order Function)` - 함수가 함수를 리턴하는 것, 리턴된 함수는 다른 변수에 저장할 수도 있고, 변수에 저장된 함수를 다시 호출할 수도 있음. => `함수간의 중복을 제거하기 위해서 사용`
```js
  const onClickOperator = (op) => () => {
    if (numOne) {
        operator = op;
        $operator.value = op;
    } else {
        alert('숫자를 먼저 입력하세요')
    }
  }

  === 

  const onClickOperator = (op) => {
    return () => {
        if (numOne) {
            operator = op;
            $operator.value = op;
        } else {
            alert('숫자를 먼저 입력하세요')
        }
    }
  }
``` 
- if문의 중첩을 제거하는 방법
    1. 공통된 절차를 각 분기점 내부에 넣는다.
    2. 분기점에서 짧은 절차부터 실행하게 if문을 작성한다.
    3. 짧은 절차가 끝나면 return(함수 내부일때)이나 break(for 문 내부일때)로 중단한다.
    4. else를 제거한다.


## 숫자야구
### 기본적인 배열 메서드와 반복문을 이용하여 숫자야구 로직 구현

<img src='https://user-images.githubusercontent.com/96935557/193992443-36ec6c35-6723-4970-9636-2fb224624341.gif'>

- 무작위로 숫자 뽑기 - 정답을 만들기 위해서는 네자리의 무작위 숫자를 뽑아야하는 과정을 거쳐야함. => Math.random() 메서드로 구현가능.
```js
    const numbers = []; // 9개의 숫자를 배열로 생성
    for (let n = 0; n < 9; n += 1) {
      numbers.push(n + 1);
    }

    const answer = [];
    for (let n = 0; n < 4; n += 1) {
      const index = Math.floor(Math.random() * numbers.length) // 랜덤 숫자를 뽑는 4번 반복문 실행
      answer.push(numbers[index]) // answer 배열에 랜덤으로 뽑은 값을 넣어주고
      numbers.splice(index, 1) // 넣은 값은 numbers 배열에서 빼줌
    }
```

- 입력값 검증, 유효성 검사 - 이미 시도한 값이거나, 4자리가 아니거나, 숫자가 중복 입력 되었을 때 에러 메시지 뱉어내야함.
```js
function checkInput(input) {
      if (input.length !== 4) {
        return alert("4자리 숫자를 입력해주세요")
      }
      if (new Set(input).size !== 4) {
        return alert("숫자를 중복되지 않게 입력해주세요")
      }
      //Set - 중복을 제거한 배열, 각자 고유의 독립된 값만 있는 배열이라 생각.
      //Set은 arr같이 length가 아닌 size가 개수를 알아내는 코드
      if (tries.includes(input)) {
        return alert("이미 시도한 값입니다")
      }
      return true;
    };
```
- arr.join('') - arr에 있는 배열 요소들을 문자열로 합쳐줌.
- string.split('') - string 요소들을 배열로 쪼개서 여러개의 요소로 만듬.
- `createTextNode`, `createElement` - document에 새로운 text 노드('1'), element('div')들을 만듬
- `.append` - createTextNode나 createElement 같은 document에서 만든 노드들을 추가해줌.
- `.innerHTML` - append와 비슷하나 HTML 태그로 추가한다는 점에서 다름.
- `.appendChild` - 노드의 자식 요소들도 실행.
- arr.indexOf('arr의 찾을 요소') - arr에서 요소가 몇번째 index에 있는지 찾아줌. 없다면 -1 return 함.
- arr.forEach(el, index){} - 배열에서 반복문 돌릴 때 편한 메서드. 인수로 함수를 받고, 배열 요소 하나하나에 인수로 받은 함수를 각각 적용.
```js
    //arr.forEach()를 for문으로?
    for(let i = 0; i < arr.length; i++){
        //...
    }
```
- arr.map((el, index) => {}) - 배열 요소 전체에 반복해서 무언가 변화를 주고싶을때 사용. `기존 배열은 바뀌지 않고 새로운 배열이 생겨 불변성 유지에 좋다!`
```js
    //arr.map((el) => { return el * 2})을 for문으로 바꾸기
    const arr = [1, 2, 3, 4];
    const result = [];
    for (let i = 0 ; i < arr.length; i++){
        result.push(arr[i] * 2);
    }
    console.log(result);
```
- Array(number).fill('무언가') - number를 length로 가진 Array에 '무언가' 요소를 채우는 메서드
```js
    // 숫자야구에서 numbers 만든 반복문을 fill과 map을 이용
    const numbers = [];
    for (let n = 0; n < 9; n += 1) {
      numbers.push(n + 1);
    }
    // =>
    Array(9).fill(0).map((el, index) => {
        return index + 1;
    })
```

## 로또 추첨기
### 타이머를 사용하고 비동기의 개념을 익히는 로직 구현

<img src='https://user-images.githubusercontent.com/96935557/194005632-e45ea798-8435-4b95-b442-604784a7f6c5.gif'>

- `비동기?` - 동기의 반댓말, 실제로 코딩한 순서와 다르게 동작하는 코드를 비동기 코드라고한다. 이벤트 리스너가 대표적인 비동기 코드이다. 타이머 또한 비동기이다.

- `arr.slice(시작 (<=, 포함), 끝(<, 포함X))` - 배열의 index를 바탕으로 배열을 자를 때 사용하는 메서드. 원본이 변하지 않아 불변성 유지에 좋다.
    - 뒤에서 자르고 싶으면 -인덱스 사용가능.
- `arr.sort((a, b) => a - b)` - 랜덤으로 뽑은 공을 오름차순대로 정렬.
    - sort 메소드, 정렬은 원본을 아예 바꿔버리기 때문에, `arr.slice().sort((a,b) => a-b)`로 사용하면 원본을 바꾸지않고 정렬할 수 있다.
    - a - b는 오름차순, b - a 는 내림차순
    - 문자열 sort => arr.slice().sort((a, b) => a[0].charCodeAt() - b[0].charCodeAt());

- `setTimeout(() => {내용}, 밀리초)` - 밀리초 뒤에 함수가 호출되는 이벤트
- `setTimeout의 타이머 시간은 정확할까?` - 자바스크립트는 기본적으로 싱글 쓰레드, 동기적 언어이기 때문에, 한번에 한가지 일만 수행 할 수 있다. 이미 많은 작업이 수행중인 상태라면, 설정한 시간이 되어도 setTimeout에 지정된 작업이 수행하지 않을 수도 있다.

- *`let과 var의 차이`*
    - 변수는 스코프(scope, 범위)라는 것을 가진다. var는 `함수 스코프를 가지고`, let은 `블록 스코프`를 가진다.
    - 함수 스코프는 함수를 경계로 접근 여부가 달라지는 것
    - 블록 스코프는 블록(중괄호)을 경계로 접근 여부가 달라지는 것
    ```js
    funtion b() {
        var a = 1;
    }
    console.log(a); // Uncaught ReferenceError: a is not defined, var a는 함수안에서 선언한 변수라 함수 바깥에서는 알수가 없음.

    funtion b() {
        let a = 1;
    }
    console.log(a); // Uncaught ReferenceError: a is not defined, let a는 블록안에서 선언한 변수라 블록 바깥에서는 알수가 없음.

    if (true) {
        var a = 1;
    }
    console.log(a) // 1, 함수가 아니고 if문이기 때문에 스코프를 벗어나지않음.

    if (true) {
        let a = 1;
    }
    console.log(a) // Uncaught ReferenceError: a is not defined, let a는 블록안에서 선언한 변수라 블록 바깥에서는 알수가 없음.

    for(var i = 0; i < 5; i++){}
    console.log(i) // 5, for문이 끝났을때 마지막으로 i++을 하고 i가 5가 되어있기 때문

    for(let i = 0; i < 5; i++){}
    console.log(i) // Uncaught ReferenceError: i is not defined, let i는 블록안에서 선언한 변수라 블록 바깥에서는 알수가 없음.
    ``` 

- *`클로져와 스코프의 혼합 문제`*
    ```js
    // var 일 때
    for (var i = 0; i < 6; i++) { // for은 동기, 빠르게 실행함. setTimeout은 비동기. var로 i 선언을 하면, i는 setTimeout에서 걸린 타이머 1000ms가 되기전에 for문을 다돌아버리고 바로 6이 되어버림
      setTimeout(() => {
        showBall(winBalls[i], $result) // i가 6이되면 winBalls[6]은 undefined가 되기에 공이 출력안됨.
      }, (i + 1) * 1000);
    }

    // 그럼 var로 선언한 사람들은 저 코드를 못쓰는걸까? => 쓸 수 있게 고쳐보기! (클로져 사용해서 문제해결)
    // 함수 스코프이니 함수로 감싸주자
    for (var i = 0; i < 6; i++) {
        (function(j){
            setTimeout(() => { // 함수 코드라 j 사용
                showBall(winBalls[j], $result)
            }, (i + 1) * 1000); // 함수코드 벗어났으니 i 사용
        })(i)
    }

    // let 일 때
    for (let i = 0; i < 6; i++) { // let은 블록스코프. 블록안이 아닌 반대의경우로 생각해보자면 블록 바깥에서 선언한 let이기에 하나의 반복문 블록마다 i값이 고정되기에 고정된 i값에 setTimeout 타이머에 있는 i도 같은 값으로 호출 => 블록별로 i 선언이 반복된다
      setTimeout(() => {
        showBall(winBalls[i], $result)
      }, (i + 1) * 1000);
    }
    ```
    - *`클로져`* - 함수와 함수 바깥에 있는 변수와의 관계. var와 비동기가 만나면 클로져 문제가 발생한다. 함수 밖의 var로 선언한 i와 setTimeout안에있는 함수안의 i가 서로 다르기 때문에, 함수로 한번 감싸 j라는 변수를 만들어주며 클로져 문제를 해결함.

## 가위바위보
### 



## 반응속도 체크하기
### 



## 틱택토
### 



## 텍스트 RPG 게임
### 



## 카드 짝맞추기 게임
### 



## 지뢰찾기
### 



## 틱택토 게임
### 



## 두더지 잡기 게임
### 

