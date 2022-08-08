# react 개발환경 구축

_create-react-app을 사용하여 react 개발환경 구축_

react는 개발 프레임워크가 아닌 라이브러리이기 때문에 필요한 기술이나 기능을 설치하여 사용해야 한다. (webpack, babel 등) 하지만 create-react-app을 사용하여 환경을 구축하면 따로 설치할 필요없이 필요한 기능들을 바로 사용할 수 있다.

---

## node.js 설치

create-react-app을 사용하기에 앞서 node.js(node, npm, npx)가 설치되어 있어야 한다.  
[node.js 설치하기](https://nodejs.org/en/)

- LTS는 안정적인 지원을, Current는 신기술 지원을 약속함.
- node.js를 설치하면 npm(node pakage manager)가 자동으로 설치되므로 따로 설치할 필요가 없다.
- npx는

```
node -v
npm -v
npx -v
```

설치를 진행한 후 `-v` 명령어를 통해 설치여부 및 버전을 확인할 수 있다.

## create-react-app을 통한 react 폴더 생성

[react 문서 확인하기](https://ko.reactjs.org/docs/create-a-new-react-app.html#create-react-app)

```
npx create-react-app [folder name]
```

proxy환경에서 node.js설정

create-react-app을 실행했을 때 아래와 같은 오류가 확인되었다.

```
npm ERR! code ERR_SOCKET_TIMEOUT
npm ERR! network Socket timeout
npm ERR! network This is a problem related to network connectivity.
npm ERR! network In most cases you are behind a proxy or have bad network settings.
npm ERR! network
npm ERR! network If you are behind a proxy, please make sure that the
npm ERR! network 'proxy' config is set properly.  See: 'npm help config'
npm ERR! A complete log of this run can be found in:
npm ERR!     /Users/jeonga/.npm/_logs/2022-06-27T04_25_22_210Z-debug-0.log
```

해결방법은 두가지가 있음.

**첫번째 방법**

```
npm config set proxy http://:
npm config set https-proxy http://:
```

**두번째 방법**

```
npm config set proxy false
npm cache clean --force
```

## 실행

```
cd [folder name] // 해당 폴더로 이동
npm start // 로컬 환경에서 개발모드를 확인하기 위해 실행하는 명령어
```

npm start를 사용하면 브라우저를 통해 `http://localhost:3000/` 가 열리고 코드 수정 시 바로 화면으로 확인할 수 있다.

## 폴더 생성 후 필요없는 코드 삭제 및 수정

- css, logo.svg 삭제
- title, favicon, mata 코드 수정

### file 용도

**App.test.js**  
**setupTests.js**  
create-react-app으로 react 프로젝트를 생성하면 react-testing-library도 함께 설치된다.  
해당 컴포넌트가 제대로 랜더링되는지 확인해준다.  
`npm test` 를 통해 실행할 수 있다.

**reportWebVitals.js**  
해당 코드에 console.log를 작성하면 앱의 퍼포먼스 시간을 분석하여 obj 형태로 보여준다.

## 라이브러리 설치

프로젝트에 필요한 라이브러리 설치

```
npm i [react-router-dom etc.]
```

- react-router-dom
- styled-components

---

## Redux

redux는 한 컴포넌트에서만 사용할 수 있는 useState의 상태값을 props의 행위 없이 전역적으로 사용할 때 사용하는 라이브러리이다.

redux를 사용하기에 앞서 설치를 진행해야 한다.

- 스토어로 사용할 reducer.js는 src에 작성한다.
  - 이 외의 다양한 방법이 있으나 현재는 기본적인 형태로 한다.

### redux 설치

```
npm install redux react-redux --save
```

- 기본 설치

```
npm install redux react-redux redux-devtools-extension redux-logger --save
```

- 선택 사항

### index.js

```
import React from "react";

import { Provider } from "react-redux";
import { createStore } from "redux";

import App from "./App";
import reducer from "./reducer";

const store = createStore(reducer);
const root = ReactDOM.createRoot(document.getElementById("root"));

root.render(
    <Provider store={store}>
      <App />
  </Provider>
);
```

- react-router-dom을 사용한다면 <router><Provider></router>의 방식으로 router로 Provider를 감싸서 사용하도록 한다.
- 스토어(reducer.js)를 전달하여 store를 생성하여 Provider에 전달한다.

### reducer.js

```
const initialState = {
  caseValue: {userid:"", userpw:""},
};

const reducer = (state = initialState, action) => {
  const newState = { ...state };

  switch (action.type) {
    case "case1":
    console.log("case1");
      break;

    case "case2":
      newState.caseValue = action.payload;
      break;

    default:
      break;
  }
  return newState;
};

export default reducer;
```

- reducer는 initalState의 값을 그대로 변경시킬 수 없기 때문에  깊은 복사를 통해 복사를 진행하여 새로운 변수를 생성 -> rerurn하여 사용
- 전역적으로 사용할 function, state value를 위와 같이 작성하였음.

### 사용예시 - useDispatch, useSelect

```
import { useSelector, useDispatch } from "react-redux";

function Login() {
  const caseVaule = useSelector((state) => state.caseVaule);
  const dispatch = useDispatch();

  function onChange(e) {
  // dispatch를 이용하여 데이터가 포함된 action을 호출
    dispatch({
      type: "case2",
      payload: { ...caseVaule, [e.target.id]: [e.target.value] },
    });
  }

  const fnLogin = (e) => {
    e.preventDefault();
    // dispatch를 이용하여 action을 호출한다.
    dispatch({
      type: "case1",
    });
  };

  return (
        <form onSubmit={fnLogin}>
          <input
            id="userid"
            onChange={onChange}
          />
          <input
            id="userpw"
            onChange={onChange}
          />
          <button type="submit" />
          <span>{caseVaule}</span> // caseValue의 값
        </form>
  );
}

export default Login;
```

- satae value를 사용할 때에는 useSelect를 사용한다.
- store에서 생성한 reducer를 불러와 satae value의 값을 변경시키는 행위를 하고자 할 때에는 useDispatch()를 사용한다.
