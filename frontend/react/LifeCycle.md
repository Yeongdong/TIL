# 1. 리액트 컴포넌트와 라이프사이클

## 마운트(Mount)

: 컴포넌트를 페이지에 처음 렌더링할 때<br>
ex. 특정 동작을 하도록 만들 수 있음

## 업데이트(Update)

: State나 Props의 값이 바뀌거나 부모 컴포넌트가 리렌더해 자신도 리렌더될 때<br>
ex. 업데이트할 때 적절한지 검사할 수 있음

## 언마운트(Unmount)

: 더 이상 페이지에 컴포넌트를 렌더링하지 않을 때<br>
ex. 메모리를 정리할 수 있음

> 리액트 훅의 하나인 함수 **useEffect**를 이용하면 이 사이클을 쉽게 제어할 수 있음

# 2. useEffect

: 어떤 값이 변경될 때마다 특정 코드를 실행하는 리액트 훅<br>
=> "특정 값을 검사한다"라고 표현함<br>
ex. 컴포넌트의 State값이 바뀔 때마다 변경된 값을 콘솔에 출력하게 할 수 있음

```javascript
import { useState, useEffect } from "react"; ①
(...)

function App() {
  const [count, setCount] = useState(0);
  const handleSetCount = (value) => {
    setCount(count + value);
  };

  useEffect(() => { ②
    console.log("count 업데이트: ", count);
  }, [count]);

  return (
    <div className="App">
      <h1>Simple Counter</h1>
      <section>
        <Viewer count={count} />
      </section>
      <section>
        <Controller handleSetCount={handleSetCount} />
      </section>
    </div>
  );
}
export default App;
```

> ② useEffect를 호출하고 두 개의 인수를 전달합니다. 첫 번째 인수로 콜백 함수를, 두 번째 인수로 배열을 전달합니다

```javascript
useEffect(callbacok, [deps]);
```

## 의존성 배열

- 두 번째 인수로 전달한 배열을 의존성 배열(Dependency Array, deps)이라고 함
- useEffect는 이 배열 요소의 값이 **변경**되면 첫 번째 인수로 전달한 콜백함수를 실행
- 배열 요소에는 여러 개를 넣어 여러 개의 값을 검사할 수 있음
- 배열 요소에 아무 것도 전달하지 않으면, 컴포넌트를 렌더링할 때마다 콜백 함수를 실행

## useEffect로 라이프 사이클 제어하기

### 업데이트(Update) 발생시 특정 코드 실행

#### 기존

```javascript
(...)
function App() {
  const [count, setCount] = useState(0);

  const handleSetCount = (value) => {
    setCount(count + value);
  };

  useEffect(() => { ①
    console.log("컴포넌트 업데이트");
  });
  (..)
}
export default App;
```

- 배열에 아무것도 전달하지 않으면, useEffect는 컴포넌트를 렌더링할 때마다 콜백 함수를 실행

#### 업데이트 발생시에만 실행

```javascript
import { useRef, useState, useEffect } from "react";
(...)
function App() {
  (...)
  const didMountRef = useRef(false); ②

  useEffect(() => { ③
    if (!didMountRef.current) {
      didMountRef.current = true;
      return;
    } else {
      console.log("컴포넌트 업데이트!");
    }
  });
  (...)
}
export default App;
```

- 현재 App 컴포넌트를 페이지에 마운트했는지 판단하는 변수 didMountRef를 Ref 객체로 생성합니다. 초깃값으로 false를 설정합니다. Ref 객체는 돔 요소를 참조하는 것뿐만 아니라 컴포넌트의 변수로도 자주 활용됩니다.
- 컴포넌트 마운트 시점에는 콘솔에 ‘컴포넌트 업데이트’ 문자열을 출력하지 않도록 조건문을 추가 합니다.

#### 정리

- useEffect에서 의존성 배열을 인수로 전달하지 않으면 마운트, 업데이트 시점 모두 콜백 함수를 호출
- 그러나 콜백 함수 내부에서 조건문과 Ref 객체로 특정 시점에만 코드를 실행하게 만들 수 있음
- 즉, 마운트 시점(didMountRef=false)에 호출하면 아무것도 출력하지 않고 함수를 종료하고, 업데이트 시점(didMountRef=true)에 호출하면 문자열을 콘솔에 출력할 수 있음

### 마운트(Mount) 제어하기

```javascript
(...)
function App() {
  (...)
  const didMountRef = useRef(false);

  useEffect(() => {
    if (!didMountRef.current) {
      didMountRef.current = true;
      return;
    } else {
      console.log("컴포넌트 업데이트!");
    }
  });

  useEffect(() => { ①
    console.log("컴포넌트 마운트");
  }, []);

	return (
	  (...)
	);
}
export default App;
```

- 의존성 배열에는 빈 배열을 전달
- useEffect에서 빈 배열을 전달하면 컴포넌트의 마운트 시점에만 콜백 함수를 실행
