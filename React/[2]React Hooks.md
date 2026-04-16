# React Hooks

### 함수형 컴포넌트에서 상태와 생명주기 기능을 사용할 수 있도록 도와주는 기능 ( v16.8부터 사용가능 )

> - 기존에는 클래스형 컴포넌트에서만 상태와 생명주기 메서드를 사용가능 했음
> - 컴포넌트가 처음 나타날 때 ( 마운트 ) -> componentDidMount
> - 컴포넌트가 업데이트될 때 ( 리렌더링 ) -> componentDidUpdate
> - 컴포넌트가 사라질 때 ( 언마운트 ) -> componentWillUnmount
> - 클래스형 컴포넌트에서는 이러한 과정들을 각각의 메서드로 관리했지만 함수형 컴포넌트에서는 useEffect같은 Hook을 사용

1. useState ( 상태 관리 )
   - 컴포넌트의 상태값을 선언하고 업데이트
   - 반환값 : [현재값,업데이트 함수]

```jsx
const [count, setCount] = useState(0);
return <button onClick={() => setCount(count + 1)}>{count}</button>;
```

2. useEffect ( 부수 효과 처리 )
   - 데이터 Fetching , 구독 , DOM 조작 등에 사용
   - 두번째 인자(의존성 배열)로 실행 시점을 제어
     - [] -> 최초 마운트시 1회 작동
     - [값] -> 해당 값이 바뀔 때 마다 실행
     - 생략 -> 매 렌더링마다 실행

```jsx
useEffect(() => {
  document.title = `Count: ${count}`;
  return () => {
    /* cleanup */
  };
}, [count]); // count가 바뀔 때마다 실행
```

3. useRef ( DOM 요소 접근 및 렌더링과 무관한 값 저장 )

```jsx
// 1. useRef를 사용하여 input 요소에 접근할 ref 생성
const inputRef = useRef(null);

useEffect(() => {
  // 2. 컴포넌트가 마운트되면 inputRef.current.focus() 실행
  inputRef.current.focus();
}, []); // 빈 배열 → 최초 렌더링 시 한 번 실행

return <input ref={inputRef} type="text" placeholder="자동 포커스" />;
```

4. useReducer ( 복잡한 상태 관리 )
5. useContext ( 전역 상태 관리 )
6. useMemo ( 성능 최적화를 위한 값 메모이제이션 )
7. useCallBack ( 성능 최적화를 위한 함수 메모이제이션 )
