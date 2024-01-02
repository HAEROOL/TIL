# state는 렌더트리의 위치에 ‘연결’되는 것

React는 컴포넌트 구조로 렌더 트리를 생성하는데, 여기서 우리는 state가 컴포넌트에 유지된다고 생각할 수 있다. 하지만 사실 React가 트리의 위치를 이용해 각 state를 알맞은 컴포넌트와 연결하는 것.

> 즉, 컴포넌트에 state가 있는 게 아니라 React가 알맞은 위치에 가져다 주는 것이다.
> 

# React는 동일한 컴포넌트가 그 자리에 렌더링 되는 한 state를 유지한다.

```tsx
export default function App() {
  const [isFancy, setIsFancy] = useState(false);
  return (
    <div>
      {isFancy ? (
        <Counter isFancy={true} /> 
      ) : (
        <Counter isFancy={false} /> 
      )}
      <label>
        <input
          type="checkbox"
          checked={isFancy}
          onChange={e => {
            setIsFancy(e.target.checked)
          }}
        />
        Use fancy styling
      </label>
    </div>
  );
}
```

예를 들어 위와 같이 `Counter` 라는 컴포넌트가 있을 때, React는 `isFancy` 가 어떤 값이건 `Counter` 를 동일한 컴포넌트로 간주한다.

왜냐? `App` 컴포넌트가 반환한 `div`의 첫번째 자식은 항상 `Counter` 일 것이기 때문이다~

그렇다면 `isFancy` 가 거짓일 경우에 `<p>` 태그로 감싸서 반환을 한다면 어떻게 될까?

```tsx
...
{isFancy ? (
        <Counter isFancy={true} /> 
      ) : (
				<p>
	        <Counter isFancy={false} /> 
	      </p>
			)}
...
```

React는 다른 이 둘을 다른 컴포넌트로 간주해 state를 초기화 시킬 것이다. **트리의 구조가 다르기 때문!**

# 같은 위치에서 state를 초기화 하려면??

## 다른 위치에 컴포넌트 렌더링하기

말 그래도 다른 위치에 렌더링 시킴으로써 React가 해당 컴포넌트를 다른 컴포넌트로 간주하게 하는 것이다. 하지만 이 방법은 많은 양의 컴포넌트를 관리할 때 힘들 수 있을 것.

## key를 이용해 초기화 하기

배열을 렌더링할 때처럼 컴포넌트에 key값을 주는 방법이다.

```tsx
{isPlayerA ? (
  <Counter key="Taylor" person="Taylor" />
) : (
  <Counter key="Sarah" person="Sarah" />
)}
```

이렇게 하면, 동일한 위치에 생성되어도 key값이 다르기 때문에 React는 다른 컴포넌트로 간주해 state를 초기화 할 것이다.

# 그러니까 컴포넌트는 최상위 범위에서 정의해 주세요!

```tsx
import { useState } from 'react';

export default function MyComponent() {
  const [counter, setCounter] = useState(0);

  function MyTextField() {
    const [text, setText] = useState('');

    return (
      <input
        value={text}
        onChange={e => setText(e.target.value)}
      />
    );
  }

  return (
    <>
      <MyTextField />
      <button onClick={() => {
        setCounter(counter + 1)
      }}>Clicked {counter} times</button>
    </>
  );
}
```

중첩해서 컴포넌트를 생성해 버리면, React가 `MyComponent` 를 생성할 때마다 하위 함수를 만들 것이고, 같은 함수에서 다른 컴포넌트를 렌더링 할 대마다 모든 state를 초기화 할 거임

### 참조

https://ko.react.dev/learn/preserving-and-resetting-state