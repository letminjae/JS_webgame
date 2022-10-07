# Let's Get it Javascript!
### `ZeroCho`님의 `Let's Get it Javascript` 책을 통해 자바스크립트를 이용한 웹게임을 만든 레포지토리 입니다.

<br />

### 단순히 프로그래밍 언어만 익히는 것이 아닌, `프로그래밍 사고력`을 길러주기 위한 목표를 향해 배웁니다. 

## 쿵쿵따
### 자바스크립트의 기본을 알고, 순서도를 이용해 로직 구현

<img src='https://user-images.githubusercontent.com/96935557/193987113-559bd241-cc7f-488f-836c-a50feb5ae7da.gif'>

- `document.querySelector()` - HTML 태그를 `선택` 하는 자바스크립트 함수
- `document.querySelectorAll()` - HTML 태그 모두를 선택하는 자바스크립트 함수
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
    - *`클로져`* - 함수와 함수 바깥에 있는 변수와의 관계. var와 비동기가 만나면 클로져 문제가 발생한다. 함수 밖의 var로 선언한 i와 setTimeout 안에있는 함수안의 i가 서로 다르기 때문에, 함수로 한번 감싸 j라는 변수를 만들어주며 클로져 문제를 해결함.

## 가위바위보
### 객체의 사용법을 익히고, 타이머를 멈췄다가 다시 재개하는 방법을 통해 로직 구현

<img src='https://user-images.githubusercontent.com/96935557/194025618-ec921ef2-a63c-48bd-a58e-d0854c7b7c56.gif'>

- 이미지 스트라이프로 인해 이미지의 px값을 다 다르게 설정해 컴퓨터가 어떤 값을 뱉어야하는지 객체로 좌표값을 나타냄.
```js
    const rspX = {
      scissors: '0',
      rock: '-220px',
      paper: '-440px'
    };

    let computerChoice = 'scissors';
    const changeComputerHand = () => {
      if (computerChoice === 'scissors') {
        computerChoice = 'rock';
      } else if (computerChoice === 'rock') {
        computerChoice = 'paper';
      } else if (computerChoice === 'paper') {
        computerChoice = 'scissors';
      }
      $computer.style.background = `url(${IMG_URL}) ${rspX[computerChoice]} 0`;
      $computer.style.backgroundSize = 'auto 200px';
    }

    let intervalId = setInterval(changeComputerHand, 50);
```
- `setInterval(콜백함수, 밀리초)` - 밀리초 간격으로 콜백함수를 계속 실행 해주는 함수.
- setInterval을 setTimeout으로 바꿀려면?
```js
// 재귀형식으로 함수를 계속 호출
// 만약 콘솔같은 간단한게 아니라 엄청 복잡한 코드면 1초마다 호출 안될수도 있음.
// 코드가 복잡해도 1초를 최대한 맞추고싶다. setInterval
// 코드 복잡해서 1초 근방만 맞추면 된다. setTimeout 재귀형태
funtion hello() {
    console.log("hello"); // 헬로 콘솔을 찍고,
    setTimeout(hello, 1000); // hello 다시 호출.
}
setTimeout(hello, 1000);
```

- `clearInterval()` - setInterval을 취소할 수 있는 함수. 단, setTimeout에 부른 콜백함수가 실행되기 전에 clearInterval 호출을 해야함. 이미 실행된 함수를 없던 일로 할 수 없음.

- 트러블 슈팅 - 버튼을 클릭할때마다 점점 엄청 interval이 빨리 돌아감. clickButton 이벤트를 5번 연속 호출하면 인터벌 1번부터 5번까지 다 호출되나, clearInterval은 5번만 취소가 됨. 나머지 1~4번 인터벌은 중복되어 interval이 빨리 돌아감. <br />
    1. clearInterval을 setTimeout에 한번 더 넣어준다. <br />
    2. 비동기되는 1초동안 clickButton을 비활성화 시킨다. - removeEventListener() 
    - *`removeEventListener 사용할 때 주의점`* - addEventListener와 removeEventListener의 콜백함수는 서로 같아야 적용이된다. 그런데?
    ```js
        // 객체, 함수, 배열은 모두 서로가 다르다. 원시값, 참조값을 배웠으면 알것.
        {} === {} // false
        [] === [] // false
        하여, 변수에 참조값을 넣어 같게 만들어야함.
    ``` 
    3. clickable 이라는 변수를 만들어 clickable이 true 일때만 setTimeout 활성화 - `Flag 변수`

- 가위바위보 로직 
    - 내 선택에 따라서 간단한 로직코드 작성
    ```js
    const scoreTable = { // event.target.id, 또는 textContent가 바위 => rock이면 0,
      rock: 0,
      scissors: 1,
      paper: -1
    }
    ```
    - 1, 0, -1 로 계산을해보니 myscore - computerScore을 대입하면, -2와 1이 나오면 내가 진것. 2와 -1이 나오면 내가 이긴 것이라는 로직이 나옴.
    - 조금이나마 if문을 줄일 수 있는 로직이 나옴.

## 반응속도 체크하기
### 시간을 측정해야하므로 시간과 관련된 메서드를 제공하는 Date 객체를 사용하고 공부함.

<img src='https://user-images.githubusercontent.com/96935557/194033922-89cd6740-8be4-4d16-8447-9aec2d5f1d72.gif'>

- 태그에 해당클래스가 있는지 아는 방법 : `태그.classList.contains('클래스')` - 클래스가 있다면 true. 없다면 false.

- 태그.classList.add('클래스') - 클래스 추가 
- 태그.classList.replace('기존클래스', '수정클래스') - 클래스 수정
- 태그.classList.remove('클래스') - 클래스 삭제 

- 종료시간 - 시작시간 = 반응속도 : new Date() : 현재의 시각을 알 수 있는 메서드 
    - new Date(2022, 9, 5, 20, 55, 5) => Wed Oct 5 2022 20:55:05 GMT+0900 (대한민국 표준시), 특이하게 월만 0부터 시작.

- 평균 반응속도 : arr.reduce() 사용
- arr.reduce((acc, cur) => {}, 초기값) : acc는 누적값, cur은 현재값.
```js
[1,2,3,4].reduce((acc,cur) => a + c, 0); // 10;
// acc: 0, cur: 1
// acc: 1, cur: 2
// acc: 3, cur: 3
// acc: 6, cur: 4

["가", "나", "다", "라"].reduce((acc, cur, i) => { a[i] = c; return a }, {})
// acc: {}, cur: "가", i: 0
// acc: {0: '가'}, cur: "나", i: 1
// acc: {0: '가', 1: '나'}, cur: "다", i: 2
// acc: {0: '가', 1: '나', 2: '다'}, cur: "라", i: 3
// {0: '가', 1: '나', 2: '다', 3: '라'}
```


## 틱택토
### 2차원 배열을 다루어 데이터를 관리하고, 배열 데이터를 HTML 화면에 표시하는 작업을 통해 삼목 게임을 만들었습니다.

<img src='https://user-images.githubusercontent.com/96935557/194033937-73529d6b-8f63-470e-be8b-5ac6edbfa32e.gif'>

- 틱택토 배열의 형식은 [[], [], []] 형태
- HTML table의 구조, tr은 행, td는 열
```html
  <table>
    <tr> 
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </table>
```
```js
    //js로 html 삼목을 구현하려면?
    const rows = []; // 행 데이터
    const $table = document.createElement('table');
    let turn = 'O';

    for(let i = 0; i < 3; i++){
        const $tr = document.createElement('tr');
        const cells = []; // 열 데이터
        for(let j = 0; j < 3; i++){
            const $td = document.createElement('td');
            cells.push($td);
            $tr.append($td);
        }
        $table.append($tr);
    }
    document.body.append($table);

    //간단하게만 보자면,
    const arr = [];
    for (let i = 0; i < 3; i++){
        const cells = [];
        for (let j = 0; j < 3; j++){
            cells.push(1);
        }
        arr.push(cells);
    }
    // [[1,1,1],[1,1,1],[1,1,1]]
```
- `구조 분해 할당` : ES6 최신문법. 어떤 객체의 속성과 속성을 변수에 담는 변수명이 같을때 사용. 코드가 여러줄로 늘어나면 훨씬 편하다.
```js
    const { body } = document;
    // ===
    const body = document.body;

    const arr = [1, 2, 3, 4, 5];
    const one = arr[0];
    const three = arr[2];
    const five = arr[4];
    // ==
    const [one, , three, , five] = arr;

    const obj = {
        a: 'hello',
        b: {
            c: 'hi',
            d: {
                e: 'wow',
            },
        },
    };
    // ==
    const { a, b : { c, d: { e } } } = obj;
```

- *`이벤트 버블링, 캡쳐링`*
    - 이벤트는 table에 달았는데, 클릭은 td에서되고 동작은된다? => 이벤트 버블링 때문.
    - `이벤트 버블링` : 이벤트가 부모 요소를 따라서 방울방울 올라간다. 상위 태그의 이벤트 리스너가 있지만, 하위 태그를 클릭시 상위 태그의 이벤트 리스너가 동작한다.
    - `이벤트 캡쳐링` : 하위 태그에 이벤트 리스너가 있지만, 상위 태그 클릭시 하위 태그의 이벤트 리스너가 동작한다.
    - 나는 꼭 table에서 이벤트 실행했으면 좋겠다. `event.currentTarget` 활용.
    - 나는 이벤트 버블링을 막고싶다. `event.stopPropagation();`
    - `이벤트 위임` : td에 이벤트 리스너를 달지 않고 상위 태그인 table에 이벤트 리스너를 다는것
    - `장점` : 만약 td에 이벤트를 달면 3*3, 9개의 이벤트 리스너를 달아야하는데, 이벤트 버블링으로 인해 table에 한개만 달면 되고, 이벤트를 멈추고 싶을때도 removeEventLister를 한번만 쓰면됨.

- rowIndex와 cellIndex : td는 자체적으로 cellIndex 메서드가 있고, tr에는 parentNode에 rowIndex 메서드가 있어 textContent가 몇번째줄 몇번째 칸인지 쉽게 찾을 수 있음

- parentNode : 요소의 부모 태그를 가져옴. $table.parentNode = body
- children : 요소의 자식 태그를 가져옴. body.children = HTMLCollection(3) [script, table, div]
- 태그.children 은 `배열처럼 생긴 객체` 이다. children[0], children[1] 처럼 사용할 수 있어서 배열처럼 보이지만, `유사 배열 객체`라고 표현한다.
- 태그.children에 indexOf 같은 배열 메서드 사용하고 싶다면 Array.from(태그.children).indexOf('무언가'); 사용하면됨

- every와 some, flat
    - flat : n차원 배열을 n-1차원으로 만들어주는 메서드. (3차원 => 1차원 일때는 `arr.flat().flat()`으로 1차원 만들수 있음)
    - every : 1차원 배열만 가능. 모든 것이 충족되면 true를 반환, 한개라도 충족되지 않은 값이 있으면 false 반환
    - some : 1차원 배열만 가능. 하나라도 충족되면 true를 반환, 모든 것이 충족되면 false 반환
    ```js
    rows = [[1,1,1],[1,1,1],[1,1,1]];
    rows.flat() // [1,1,1,1,1,1,1,1,1]
    rows.flat().every((td) => td.textContent) // 다 차있다면 (무승부) true, 한칸이라도 비워져있다면 false
    rows.flat().some((td) => td.textContent) // 다 차있다면 (무승부) false, 한칸이라도 비워져있다면 true
    ```

## 텍스트 RPG 게임
### 클래스 문법을 배우며, 자바스크립트로 RPG 게임을 만들었습니다.

<img src='https://user-images.githubusercontent.com/96935557/194033928-b72db3c7-7cb3-4a7f-a9db-98a8b4f7481d.gif'>


- *`깊은 복사와 얕은 복사`*
    - 객체안의 정보만 필요하기 때문에 참조가 아닌 복사를 해야할 때가 있음.
    - 참조로 한다면 객체안의 정보가 변환될 가능성이 있기 때문.
    - 얕은 복사 : 겉 껍데기만 복사가 되는 형태, 내부객체는 참조형태라 값이 바뀔수 있음.
    - 깊은 복사 : 겉과 내부 모두 복사가 되는 형태. 원본 객체 항상 유지.
    ```js
    const monsterList = [
        { name: '슬라임', hp: 25, att: 10, xp: 10 },
        { name: '스켈레톤', hp: 50, att: 15, xp: 20 },
        { name: '마왕', hp: 150, att: 35, xp: 50 },
    ];

    //얕은 복사
    const monster1 = { ...monsterList[0] };
    // 배열 얕은복사
    const arr = [...arr];
    //깊은 복사
    const monster2 = JSON.parse(JSON.stringify(monsterList[0]));
    ```

    ```js
    // 좀 더 딥하게
    const a = [5]; //배열
    const b = 'hello' //문자열
    const c = {}; //객체 리터럴
    const arr = [a, b, c];

    const arr1 = arr; // 변수에 배열을 넣는 형태. => 참조 형태
    arr1[1] = 'hi' // 'hi' , 참조형태라서 arr[1] 값 자체가 hello 에서 hi로 바뀌었다.

    // 값자체를 바꾸지 않고 싶을때 흔하게 얕은복사를 많이 쓴다.
    const arr2 = [...arr]; //얕은 복사
    const arr2 = arr.slice() //얕은 복사 2
    arr2[1] = 'minjae'
    arr[1] // 'hello', 얕은복사 사용시 본 배열의 값은 바뀌지않고 새로운 배열이 복사가된다.

    //얕은 복사의 문제점
    arr2[0].push(1)
    arr[0] // [5, 1], 내부의 요소는 참조형태이기 때문에 내부요소를 건드리면 변화가능. => 겉껍데기 배열은 복사가 된 상태이나, 내부 요소(객체)는 복사가 안되어있음.

    //그래서 깊은복사를 하여 내부의 요소도 복사함
    const arr3 = JSON.parse(JSON.stringfy(arr)); // 가장 간단한 깊은복사법, JSON 파일로 바꾸고, JSON 파일을 다시 파싱해서 객체로 만드는 과정.
    arr3[0].push(2);
    arr[0] // [5, 1], 2는 push되지않음.
    ```

- `this?`
    - 객체 안의 this는 자기 자신을 나타낸다. 하지만, 화살표 함수일때는 아니니, function의 this가 자기 자신이라고 생각하자.
    - 화살표 함수일때 this는 바깥의 this가 되어버림. hero 객체에서는 `window`가 되어버린다. document의 부모. 브라우저 전체를 담당하는 객체.
    ```js
    const hero = {
        name: '',
        lev: 1,
        maxHp: 100,
        hp: 100,
        xp: 0,
        att: 10,
        attack: function(monster) {
        monster.hp -= this.att; // 여기서 this는 hero이다. hero.att 과 같다.
        this.hp -= monster.att; // 여기서 this는 hero이다. hero.hp 와 같다.
        },
        // 화살표 함수일 때 this는 window
        heal : (monster) => { 
        this.hp += 20;
        this.hp -= monster.att;
        },
    };
    ```

    ```js
    // 좀 더 this가 자기자신이라는 것에 엄밀히 말하면,
    // 객체.메소드를 해야 this가 그 객체가 된다.
    const b = {
        name: '차민재',
        sayName() {
            console.log(this === b)
        }
    }
    b.sayName(); // true, 객체.메서드를 해야 this가 자기자신이 된다.

    const bf = b.sayName;
    bf(); // false, 변수 선언 후 호출하면 this는 window가 된다.
    ```

- `클래스(Class)가 필요한 이유` : 이 게임을 발전시키고 싶음. 예를들어 몬스터가 여러마리 나온다거나, 동료 시스템을 업데이트 한다거나.. 코드가 굉장히 복잡해짐. 변수가 많아짐에 따라 서로 상호작용하는 객체들을 잘 정리할 수 있는 무언가가 필요해짐. => Class의 탄생. 객체지향 프로그래밍의 시초.

- `클래스` - 객체를 생성하기 위한 템플릿 (서식)
    - 클래스가 있기 이전에는 함수로 객체를 만듬. => `팩토리 함수`
    ```js
    function createMonster(name, hp, att, xp) {
        return { name, hp, att, xp }
    }
    const monster1 = createMonster('슬라임', 25, 10, 10);
    const monster2 = createMonster('스켈레톤', 50, 20, 30);
    const monster3 = createMonster('마왕, 150, 40, 100);
    ```
    - 또한 생성자 함수를 통한 객체를 만드는 경우도 많았다.
    - new 생성자를 사용하면 Monster 함수의 this가 window가 아닌 자기자신이 되는 신기한 현상이 일어남. 하지만, new를 사람들이 많이 빼먹고 window가 this가 되어 많은 에러가 발생. 하여 class 문법 탄생.
    ```js
    function Monster(name, hp, att, xp){
        this.name = name;
        this.hp = hp;
        this.att = att;
        this.xp = xp;
    }
    const monster1 = new Monster('슬라임', 25, 10, 10);
    const monster2 = new Monster('스켈레톤', 50, 20, 30);
    ```
    
    - Class 사용시 constructor를 꼭 사용해줘야한다. 생성자라는 의미이다. 
    - 이전 생성자 함수를 사용할 때 new를 빼먹으면 this가 window로 되었으나, Class에선 new를 빼먹으면 에러가 발생한다.
     ```js
    Class Monster {
        constructor(name, hp, att, xp){
            this.name = name;
            this.hp = hp;
            this.att = att;
            this.xp = xp;
        }
    }
    const monster1 = new Monster('슬라임', 25, 10, 10);
    const monster2 = new Monster('스켈레톤', 50, 20, 30);
    ```

- `화살표 함수와 this의 관계`
    - ❗ this는 항상 먼저 결정되는 것이 아닌, 함수가 호출될때 결정이된다! (항상 자기자신이 아니고, window가 아니고 상황에 따라 달라진다.)
    - this 문제를 해결하기 위해 화살표함수가 출시 되었다.
    - addEventListener의 경우, 직전 쿼리가 this가 되므로, addEventListener의 콜백함수는 화살표함수로 만들어줘야 정상적인 동작을 한다.
    ```js
    // Class Game 에 있는 onGameMenuInput 함수 : 화살표 함수를 사용하였다.
    class Game {
        constructor(name) {
            this.monster = null;
            this.hero = null;
            this.monsterList = [
                { name: '슬라임', hp: 25, att: 10, xp: 10 },
                { name: '스켈레톤', hp: 50, att: 15, xp: 20 },
                { name: '마왕', hp: 150, att: 35, xp: 50 },
            ];
            this.start(name);
        }
        start(name) {
            console.log(this);
            $gameMenu.addEventListener('submit', this.onGameMenuInput);
            $battleMenu.addEventListener('submit', this.onBattleMenuInput);
            this.changeScreen('game');
            this.hero = new Hero(this, name);
            this.updateHeroStat();
        }
        //만약 onGameMenuInput을 일반 function으로 만들었다면, 함수 내 this는
        //$gameMenu.addEventListener('submit', this.onGameMenuInput);의 $gameMenu가 this가 되므로 정확한 동작이 일어나지 않는다.
        //하여 화살표 함수로 만들어주며, Class Game을 this로 바라보게 만든다.
        onGameMenuInput = (event) => {
            event.preventDefault();
            const input = event.target['menu-input'].value;
            if (input === '1') { // 모험
                this.changeScreen('battle');
                const randomIndex = Math.floor(Math.random() * this.monsterList.length);
                const randomMonster = this.monsterList[randomIndex];
                this.monster = new Monster(
                this,
                randomMonster.name,
                randomMonster.hp,
                randomMonster.att,
                randomMonster.xp,
                );
                this.updateMonsterStat();
                this.showMessage(`몬스터와 마주쳤다. ${this.monster.name}인 것 같다!`);
            } else if (input === '2') { // 휴식
                this.hero.hp = this.hero.maxHp;
                this.updateHeroStat();
                this.showMessage('충분한 휴식을 취했다.');
            } else if (input === '3') { // 종료
                this.showMessage(' ');
                this.quit();
            }
        }
        //...
    }
    ```
- bind : this를 바꿔줄 수 있는 메서드. 하지만 화살표 함수는 bind 하지 못해 this가 바뀌지않아 그대로 window 출력
```js
function a() {
    console.log(this);
}
a(); // window
a.bind(document)(); // document

const b = () => {
    console.log(this)
}
b.bind(document)(); // window
```

- Class의 `상속` : 덮어씌운다. Class 마다 공통된 부분들이 여러개 있다면 상속을 통해서 코드를 간결하게 만들수 있다. => extends 사용!
- `super()` : constructor 내에서 사용. 부모 클래스의 생성자를 호출
```js
class Hero {
    constructor(game, name) {} // 중복
    attack(target){ ... } // 중복
    heal(monster){ ... }
    getXp(xp) { ... }
}

class Monster {
    constructor(game, name, hp, att, xp) {} // 중복
    attack(target){ ... } // 중복
}
class Boss {
    constructor(game, name, hp, att, xp) {} // 중복
    attack(target){ ... } // 중복
    skill(target){ ... }
}

// 상속을 해서 간단히 만들어보자
// 공통 클래스인 Unit 생성
class Unit {
  constructor(game, name, hp, att, xp) {
    this.game = game;
    this.name = name;
    this.maxHp = hp;
    this.hp = hp;
    this.xp = xp;
    this.att = att;
  }
  attack(target) {
    target.hp -= this.att;
  }
}

class Hero extends Unit {
  constructor(game, name) {
    super(game, name, 100, 10, 0);
    this.lev = 1;
  }
  attack(target) {
    super.attack(target); // 부모 클래스의 attack을 super.attack()으로 호출 가능
    // 부모 클래스 attack 외의 동작 코드 작성
  }
  heal(monster) {
    this.hp += 20;
    this.hp -= monster.att;
  }
  getXp(xp) {
    this.xp += xp;
    if (this.xp >= this.lev * 15) { // 경험치를 다 채우면 레벨업하는 상황
      this.xp -= this.lev * 15; // xp: 5,  lev: 2, maxXp: 15
      this.lev += 1;
      this.maxHp += 5;
      this.att += 5;
      this.hp = this.maxHp;
      this.game.showMessage(`레벨업! 레벨 ${this.lev}`);
    }
  }
}

class Monster extends Unit {
  constructor(game, name, hp, att, xp) {
    super(game, name, hp, att, xp);
  }
}
```


## 카드 짝맞추기 게임
### 비동기의 깊은 이해와 이벤트 루프에 대해서 공부하고 로직 구현

<img src='https://user-images.githubusercontent.com/96935557/194034459-be33b8e1-1a31-4158-8ada-5fa2c121b4f4.gif'>

- 두 카드가 짝이 다를 때 아예 뒤집히지 않는 경우
    - 두 번째 카드의 앞면을 보이기도 전에 flipped 클래스가 제거되어 오류가 생김.
    - 앞면을 보일 수 있는 시간을 setTimeout으로 확보
    ```js
    setTimeout(() => {
        clicked[0].classList.remove('flipped');
        clicked[1].classList.remove('flipped');
        clicked = [];
        clickable = true;
      }, 500);
    ```
- 모든 짝을 맞추었을 때 마지막 카드가 열리지 않고 alert가 뜨는 경우
    - 마지막 카드의 앞면을 보이기도 전에 alert가 뜨는 경우.
    - 마지막 카드의 색깔을 볼 수 있는 시간을 setTimeout으로 확보
    ```js
    setTimeout(() => {
      alert(`축하합니다! ${(endTime - startTime) / 1000}초 걸렸습니다.`);
      resetGame();
    }, 1000);
    ```

- 효과 발생 중 카드 클릭 막기 : 무언가 버그가 생긴다면 Flag 변수를 항상 선언.
    ```js
    let clickable = false;

    function onClickCard() {
    // clickable이 false이면 , 이미 완성된 카드는, 클릭한 카드는 연달아 클릭을 막아준다.
      if (!clickable || completed.includes(this) || clicked[0] === this) {
        return;
      }
      this.classList.toggle('flipped');
      clicked.push(this);
      // 여러개를 클릭했는데 열리지 않게 막아준다.
      if (clicked.length !== 2) {
        return;
      }
      const firstBackColor = clicked[0].querySelector('.card-back').style.backgroundColor;
      const secondBackColor = clicked[1].querySelector('.card-back').style.backgroundColor;
      if (firstBackColor === secondBackColor) { // 두 카드가 같은 카드면
        completed.push(clicked[0]);
        completed.push(clicked[1]);
        clicked = [];
        if (completed.length !== total) {
          return;
        }
        const endTime = new Date();
        setTimeout(() => {
          alert(`축하합니다! ${(endTime - startTime) / 1000}초 걸렸습니다.`);
          resetGame();
        }, 1000);
        return;
      }
      clickable = false;
      setTimeout(() => {
        clicked[0].classList.remove('flipped');
        clicked[1].classList.remove('flipped');
        clicked = [];
        clickable = true;
      }, 500);
    }
    ```

- *`호출 스택과 이벤트 루프`*
    - 호출스택, 이벤트루프, 백그라운드, 태스크 큐
    - `호출스택` : 함수들이 실행되는 공간.
    - `백그라운드` : 타이머(setTimeout, setInterval)와 이벤트리스너들이 저장되는 공간.
    - `태스크 큐` : 타이머와 이벤트리스너에 있는 콜백함수들이 줄을 서서(queue의 의미) 대기하고, 실행되는 공간
    - `이벤트 루프` : 태스크 큐에서 호출 스택으로 함수를 이동시키는 존재. 호출 스택이 비어있으면 이벤트 루프는 태스크 큐에서 함수를 하나씩 꺼내 호출스택으로 옮김.
    - *`startGame()을 호출 했을때의 이벤트 루프`*
        - anonymous => startGame => createcard => forEach로 flip total번 => setTimeout 함수니까 호출스택에 있음. 타이머만 백그라운드로 빠짐
        - 이 과정에서 있는 타이머들과 클릭 이벤트 리스너는 모두 백그라운드로 빠지고 차례차례 위에서부터 스택 소화.
        <img src = 'https://user-images.githubusercontent.com/96935557/194289586-2ba8cd3d-66ad-4a5f-9847-bae36e314f76.PNG'>
    - *`startGame()이 모두 끝나고 나서의 백그라운드와 태스크 큐 동작`*
        - 클릭이벤트는 대기 상태. 호출스택 비어있으니 1초 타이머 부터 동작시작.
        - 1초 타이머에 있는 콜백함수인 `card.classList.add('flipped');` 태스크 큐 이동
        - `card.classList.add('flipped');` 호출스택으로 이동되어 실행.
        <img src = 'https://user-images.githubusercontent.com/96935557/194291501-dc60491b-aa7f-4547-be75-9996bfd92335.PNG'>
    - *`현재 오류가나는 2, 5, 8, 9번 카드를 연달아 클릭했을때 8,9번이 안뒤집히는 상태`*
        - 9번 카드의 click 콜백함수가 호출스택에서 실행되고 호출 스택이 비어져 fliped가 remove되어야하는 0.5초 타이머가 실행되어야함.
        - 0.5초 안에 click 8,9 번 콜백함수가 실행되어버려 0.5초 settimeout의 콜백함수보다 실행이 빨라지면 뒤집히지 않는 문제 발생. ( => 큐에 8,9번 콜백이 settimeout 0.5초 콜백보다 먼저 등록되어 문제)
        - 실행 자체를 막을 수는 없기에, 카드가 2장이 될때 clickable을 false로 만들어 세 번째 카드부터는 클릭을 못하게 만들어야함.

- 이벤트 루프 시각화 영상: (https://www.youtube.com/watch?v=8aGhZQkoFbQ)

## 지뢰찾기
### 이때 까지 배운 개념을 심화하고, 재귀 함수를 이용해서 구현했습니다.

<img src='https://user-images.githubusercontent.com/96935557/194033745-2c529406-fdf8-4ad3-85ba-5f84a914c232.gif'>

- 간단하게 배열 만드는 방법 : Array(요소 개수).fill().map((arr, i) => {return i});

- 이차원 배열 만드는 법 : 이중 for문 사용
```js
  const data = [];
  for (let i = 0; i < row; i++) {
    const rowData = [];
    data.push(rowData);
    for (let j = 0; j < cell; j++) {
      rowData.push(CODE.NORMAL);
    }
  }
```

- 이차원 배열에 지뢰심기 : 지뢰의 행렬을 알아야하는데, 행은 지뢰숫자 / 열, 열은 지뢰숫자 / 열의 나머지로 로직을 짠다.
```js
  for (let k = 0; k < shuffle.length; k++) {
    const ver = Math.floor(shuffle[k] / cell); // 7번째 줄
    const hor = shuffle[k] % cell; // 1번째 칸
    data[ver][hor] = CODE.MINE;
  }
```

- 우클릭으로 깃발 꽂기 : 우클릭은 contextmenu 이벤트가 따로 존재, 메뉴가 뜨는 기본동작이 있기에 e.preventDefault() 해주기.
  - 우클릭은 총 3가지로 존재한다. 물음표거나, 깃발이거나, 빈칸이거나.
  - 하지만 그 칸이 지뢰라면 지뢰일때 물음표거나, 깃발이거나, 빈칸이거나.
  - 총 6개의 조건문으로 표현을해야한다.
  ```js
  function onRightClick(event) {
    event.preventDefault();
    const target = event.target;
    const rowIndex = target.parentNode.rowIndex;
    const cellIndex = target.cellIndex;
    const cellData = data[rowIndex][cellIndex];
    if (cellData === CODE.MINE) { // 지뢰면
      data[rowIndex][cellIndex] = CODE.QUESTION_MINE; // 물음표 지뢰로
      target.className = 'question';
      target.textContent = '?';
    } else if (cellData === CODE.QUESTION_MINE) { // 물음표 지뢰면
      data[rowIndex][cellIndex] = CODE.FLAG_MINE; // 깃발 지뢰로
      target.className = 'flag';
      target.textContent = '!';
    } else if (cellData === CODE.FLAG_MINE) { // 깃발 지뢰면
      data[rowIndex][cellIndex] = CODE.MINE; // 지뢰로
      target.className = '';
      target.textContent = '';
    } else if (cellData === CODE.NORMAL) { // 닫힌 칸이면
      data[rowIndex][cellIndex] = CODE.QUESTION; // 물음표로
      target.className = 'question';
      target.textContent = '?';
    } else if (cellData === CODE.QUESTION) { // 물음표면
      data[rowIndex][cellIndex] = CODE.FLAG; // 깃발으로
      target.className = 'flag';
      target.textContent = '!';
    } else if (cellData === CODE.FLAG) { // 깃발이면
      data[rowIndex][cellIndex] = CODE.NORMAL; // 닫힌 칸으로
      target.className = '';
      target.textContent = '';
    }
  }
  ```

- 주변 지뢰 개수 세기 : 좌클릭 이벤트를 구현해야하는 경우.
  - 지뢰가 있을때 : 지뢰를 표시하면서 게임이 터지고 재시작.
  - 지뢰가 없을때 : 주변에 지뢰가 몇개 있는지를 표시함.
    - 지뢰의 경우는 : CODE.MINE 뿐만 아니라  CODE.QUESTION_MINE, CODE.FLAG_MINE 포함
    - 모서리 또는 가장 바깥쪽 칸을 확인할때는 주변칸이 undefined이 뜨니 옵셔널체이닝 사용.
    ```js
    function countMine(rowIndex, cellIndex) {
      const mines = [CODE.MINE, CODE.QUESTION_MINE, CODE.FLAG_MINE];
      let i = 0;
      mines.includes(data[rowIndex - 1]?.[cellIndex - 1]) && i++; // 1번칸
      mines.includes(data[rowIndex - 1]?.[cellIndex]) && i++; // 2번칸
      mines.includes(data[rowIndex - 1]?.[cellIndex + 1]) && i++; // 3번칸
      mines.includes(data[rowIndex][cellIndex - 1]) && i++; // 4번칸 (5번칸 칸 나자신)
      mines.includes(data[rowIndex][cellIndex + 1]) && i++; // 6번칸
      mines.includes(data[rowIndex + 1]?.[cellIndex - 1]) && i++; // 7번칸
      mines.includes(data[rowIndex + 1]?.[cellIndex]) && i++; // 8번칸
      mines.includes(data[rowIndex + 1]?.[cellIndex + 1]) && i++; // 9번칸
      return i;
    }
    // 논리 연산자 && : 앞에 코드가 true라면 뒤의 코드를 실행하라. and라고 생각할 수도 있지만 논리연산자는 굉장히 많이쓰이는 코드이다.
    // 논리 연산자 || : 앞에 코드가 false라면 뒤의 코드를 실행하라. or이라고 생각할 수도 있지만, 적절할때 사용!
    ```
- *`nullish coalescing : ?? 연산자`* : null과 undefined이면 앞의 코드. 아니라면 뒤의 코드 사용.

- 재귀? : 주변에 빈칸을 한번에 열고싶은 로직 작성.
  - 함수가 나 자신의 함수를 호출하는 것.
  - 처음엔 open() 함수로 클릭한 주변 9칸만 여는 코드를 openAround()로 작성했지만, 재귀를 사용하여 주변에 빈칸이 있다면 그주변 9칸도 여는 로직작성.
  - 허나 `Maximum call stack size exceeded` 발생.
  - 호출스택을 생각해보면, 기존 openAround() 함수가 끝나지도 않았는데 또 openAround()가 호출되는 상황이 생김. => 호출스택 터짐
  - => 그렇다면, 호출스택이 바쁘니 백그라운드와 태스크 큐를 좀 써보면 되지않을까 라는 생각을 해야함.
  - setTimeout(재귀콜백함수, 0) 사용으로 해결!
  ```js
  function openAround(rI, cI) {
    setTimeout(() => {
      const count = open(rI, cI);
      if (count === 0) {
        openAround(rI - 1, cI - 1);
        openAround(rI - 1, cI);
        openAround(rI - 1, cI + 1);
        openAround(rI, cI - 1);
        openAround(rI, cI + 1);
        openAround(rI + 1, cI - 1);
        openAround(rI + 1, cI);
        openAround(rI + 1, cI + 1);
      }
    }, 0);
  }
  ```
  - => 허나 최적화가 안되어서 버벅거림 및 윈도우에 렉걸림 현상발생.
  - 어떤것이 최적화가 안되어있는가? 굳이 열려있는 빈칸을 계속 9칸씩 여니깐 최적화 문제가 생김. 열다보면 기준칸을 다시 열게됨. 그럼 또 기준칸을 재귀로 염. 무한반복..
  - 한번 연칸은 다시 열지 않는 방어 코드를 설정해야함
  ```js
  // 칸이 열렸던 칸이면 return
  // data[rowIndex]가 undefined가 될수 있기 때문에 (가장바깥쪽) ?. 옵셔널체이닝사용
  if (data[rowIndex]?.[cellIndex] >= CODE.OPENED) return;
  ```


## 2048
### 키보드, 마우스 드래그 이벤트 사용을 중점적으로 로직구현

<img src='https://user-images.githubusercontent.com/96935557/194492452-96802e4f-bdaf-4373-b3fb-b319f31c19e7.gif'>

- 이차원 배열 드로잉 ($table -> $fragment -> $tr -> $td) => 랜덤한 곳에 첫 숫자 2 배치 => 이벤트를 통해 유저 게임진행

- document.createDocumentFragment() : fragment를 넣어 성능 향상. tr이 백만개다? 백만번 for문 반복 돌면서 append를 해줘야하는데 드로잉 최적화가 안됨. `fragment는 메모리에만 저장이 되어 테이블에 백만번 계속 작업하는게 아닌 백만개의 fragment를 한번에 드로잉`

- 이동방향 판단하기
  - keydown, keyup 이벤트 : 키보드를 눌렀다(keydown) 떼는(keyup) 이벤트
  - mousedown, mouseup, mousemove 이벤트 : 마우스를 누르고(mousedown) 이동하고(mousemove) 떼는(mouseup) 이벤트
    - 마우스는 상하좌우 어디로 move 했냐를 알기 어렵기때문에, clientX, clientY를 통해 마우스를 누르고 뗐을때의 좌표값을 구해 일일이 상하좌우값을 이벤트리스너에 주어야한다.
    - Math.abs() : 해당 값의 절댓값을 구해서 반환한다.
    ```js
      let startCoord;
      window.addEventListener('mousedown', (event) => {
        startCoord = [event.clientX, event.clientY];
      });
      window.addEventListener('mouseup', (event) => {
        const endCoord = [event.clientX, event.clientY];
        const diffX = endCoord[0] - startCoord[0];
        const diffY = endCoord[1] - startCoord[1];
        if (diffX < 0 && Math.abs(diffX) > Math.abs(diffY)) {
          moveCells('left');
        } else if (diffX > 0 && Math.abs(diffX) > Math.abs(diffY)) {
          moveCells('right');
        } else if (diffY > 0 && Math.abs(diffX) <= Math.abs(diffY)) {
          moveCells('down');
        } else if (diffY < 0 && Math.abs(diffX) <= Math.abs(diffY)) {
          moveCells('up');
        } 
      });
    ```
  - switch문으로 case : 왼쪽, 오른쪽, 위, 아래로 구분
  ```js
  switch (direction) {
      case 'left': {
        const newData = [[], [], [], []];
        data.forEach((rowData, i) => {
          rowData.forEach((cellData, j) => {
            if (cellData) { // newData = [2(현재값), 2(이전값), 4] 라면 왼쪽으로 갔을때 [4, 4]가 되겠네?라고 예시를들어 로직구현
              const currentRow = newData[i] //지금 줄
              const prevData = currentRow[currentRow.length - 1]; // 이전 값
              if (prevData === cellData) { // 이전 값과 지금 값이 같으면 합친다
                const score = parseInt($score.textContent);
                $score.textContent = score + currentRow[currentRow.length - 1] * 2;
                currentRow[currentRow.length - 1] *= -2; // 왜 -2인가?
                // 한번에 합쳐지지 않게 하기 위해서 한번 [-4, 4]로 걸어주고 Math.abs로 [4,4]로 만들어준다.
              } else {
                newData[i].push(cellData); 
              }
            }
          });
        });
        console.log(newData);
        [1, 2, 3, 4].forEach((rowData, i) => {
          [1, 2, 3, 4].forEach((cellData, j) => {
            data[i][j] = Math.abs(newData[i][j]) || 0;
          });
        });
        break;
      }
      case 'right': //...
      //...
  }
  ```


- 숫자 합쳐 두배로 만들기

- 승리와 패배 구현


## 두더지 잡기 게임
### 

<img src='https://user-images.githubusercontent.com/96935557/194033760-a0145cbf-7598-429b-97b2-72f144c7af7c.gif'>

- 

