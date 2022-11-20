# useCallback

내가 만둘고 싶었던 Dropdown 컴포넌트는

- focus가 됐을 때 선택할 수 있는 옵션리스트가 나온다.
- 윗방향키, 아랫방향키를 눌렀을 때 값을 선택할 수 있다.
- 스크린리더 대응

이런 기능을 가진 컴포넌트이다.

하지만 위 기능 중에 2번째 기능을 구현하던 도중 이상한 상황이 생겼다.

에러 내용은 윗방향키, 아랫방향키이벤트가 focus out 됐을 때 사라져야 하지만 사라지지 않고 남아있던 것이다.

그러니까 input에 포커스가 됐을 때 option list가 나오면서 윗방향키를 누르면 "위로" , 아랫방향키를 누르면 "아래로" 라는 콘솔이 찍히는데  
이게 focus out이 됐을 때도 자꾸 찍히는 것이었다.

```javascript
import { useEffect, useState } from "react";
import "./App.css";
import styled from "styled-components";

const keyCode = {
  ARROW_UP: "ArrowUp",
  ARROW_DOWN: "ArrowDown",
};

function App() {
  const [open, setOpen] = useState(false);
  // 이 부분
  const keyboardEvent = (e: KeyboardEvent) => {
    if (e.key === keyCode.ARROW_UP) {
      // 리스트 한칸 위로
      console.log("위로");
    }
    if (e.key === keyCode.ARROW_DOWN) {
      // 리스트 한칸 아래로
      console.log("아래로");
    }
  };

  const focusDropdown = () => {
    setOpen(true);
  };
  const blurDropdown = () => {
    setOpen(false);
  };

  useEffect(() => {
    if (open) {
      window.addEventListener("keydown", keyboardEvent);
    } else {
      window.removeEventListener("keydown", keyboardEvent);
    }
  }, [open]);

  return (
    <Position>
      <SelectWrap>
        <SelectedValue onFocus={focusDropdown} onBlur={blurDropdown} readOnly />
        {open && (
          <SelectOptionList tabIndex={3}>
            <SelectOption>가</SelectOption>
            <SelectOption>나</SelectOption>
            <SelectOption>다</SelectOption>
            <SelectOption>라</SelectOption>
          </SelectOptionList>
        )}
      </SelectWrap>
    </Position>
  );
}

export default App;
```

이유가 뭘까 고민하던 도중 keyboardEvent 함수가 re render 될때마다 새롭게 다시 생성될 것 같다는 생각이 들었다.  
그래서 함수가 딱 한번만 생성되고 더 이상 생성되지 않게 하기 위해 useCallback을 함수에 씌워보았다.
간단하게 해결 되었다.

```javascript
const keyboardEvent = useCallback((e: KeyboardEvent) => {
  if (e.key === keyCode.ARROW_UP) {
    // 리스트 한칸 위로
    console.log("위로");
  }
  if (e.key === keyCode.ARROW_DOWN) {
    // 리스트 한칸 아래로
    console.log("아래로");
  }
}, []);
```

굳이 useCallback을 쓰지 않으려면

```javascript
useEffect(() => {
  const keyboardEvent = (e: KeyboardEvent) => {
    if (e.key === keyCode.ARROW_UP) {
      // 리스트 한칸 위로
      console.log("위로");
    }
    if (e.key === keyCode.ARROW_DOWN) {
      // 리스트 한칸 아래로
      console.log("아래로");
    }
  };
  if (open) {
    window.addEventListener("keydown", keyboardEvent);
  }
  return () => {
    window.removeEventListener("keydown", keyboardEvent);
  };
}, [open]);
```

이렇게 useEffect 안에서 함수를 생성하고 clean up 함수로 이벤트를 제거해주면 될 것 같다.

## 결론!

useCallback을 깊게 잘 알고 있지는 않지만 useCallback을 써서 함수를 한번만 생성하게 하는 것이  
useEffect안에서 함수를 여러번 생성하는 것보다 더 나을 것 같다고 생각한다.

useCallback을 더 공부해서 필요할 때 안필요할 때를 확실하게 구분해서 사용할 수 있었으면 좋겠다.
