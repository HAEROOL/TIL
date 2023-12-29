# state 구조화 원칙

## 연관된 state는 그룹화 한다

두 개의 state 변수가 항상 함께 변경된다면, 단일 state로 통합하는 것을 고려해 보자.

## state의 모순을 피한다

모순이 생겨나는 경우를 피하도록 한다. 모순을 피하는 방법 중 하나는 상태 변수들을 미리 지정하여 이들을 사용하는 것

```jsx
const [status, setStatus] = useState('typing');

async function handleSubmit(e) {
    e.preventDefault();
    setStatus('sending');
    await sendMessage(text);
    setStatus('sent');
  }

const isSending = status === 'sending';
const isSent = status === 'sent';
```

## 불필요한 state를 사용하지 않는다

만약 한 state에서 계산할 수 있는 값이라면 state를 사용하지 않는다.

특히 props의 미러링을 주의할 것! props를 state의 초깃값으로 설정하게 되면, props에 변경이 일어나도 state가 업데이트 되지 않는다. 차라리 직접 props를 사용하자.

## state의 중복을 피한다

동일한 데이터를 중복해서 state에 저장하게 되면, 이후 값의 업데이트가 발생했을 때, 중복된 항목을 모두 업데이트 해주어야 하고, 이는 값의 추적이나, 오류를 찾는 것을 어렵게 만든다.

## 깊게 중첩된 state를 피한다

데이터가 많이 중첩되어 있다면, 이것을 업데이트 하는 건 많은 비용이 들게 된다. 그럴 땐 데이터를 평탄화(정규화)하는 것을 고려해보자.