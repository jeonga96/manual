# 리액트

## node.js 설치

- 리액트를 운영하기 위한 Javascript 기반 런타임 환경.
- 공식사이트에서 운영체제 알맞은 버전을 다운로드 받아 설치한다.
- 윈도우즈 설치진행시 추가 도구설치를 자동으로 하도록 체크한다.
  - chocolatley : 윈도우상 패키지 관리자.
  - python : 파이선 3.x 버전 
  - visual studio tool : c/c++ buildtool 및 도구들.
  - 등이 각종 윈도우즈 업데이트와 함께 설치된다. (시간 소요)
  - 자동설치는 아니지만, mac의 경우 유사한 homebrew등을 사용할 수 있다.
- 설치 완료후 node, npm, npx를 사용할 수 있다.



## React boiler plate 사용.

- 리액트 프로젝트 구성을 위하여 create-react-app 툴체인을 사용한다.

  ``` sh
  npx create-react-app envtest
  ```

- 프로젝트를 빌드하고 실행한다.

  ``` sh
  cd envtest
  npm start
  ```

- 개발툴 (ex: visual studio code) 등에서 create-react-app으로 생성된 디렉토리를 열고, 구성을 확인한다.



## React Router

- react router는 spa(single page application)인 프로젝트를 브라우저의 location, history 등의 api와 연계하여 마치 url이 부여된 여러 페이지로 동작하게 해주는 third party 라이브러리 임.

- 샘플이 아닌 프로젝트에서는 규모에 상관없이 거의 쓰이게 되는 라이브러리.

- 설치

  ``` sh
  npm install react-router-dom@6
  ```

- 적용

  - index.js 파일에 BrowserRouter를 적용한다.

    ``` js
    import React from 'react';
    import ReactDOM from 'react-dom/client';
    // router 라이브러리를 추가한다.
    import { BrowserRouter } from "react-router-dom";
    import './index.css';
    import App from './App';
    import reportWebVitals from './reportWebVitals';
    
    const root = ReactDOM.createRoot(document.getElementById('root'));
    root.render(
      <React.StrictMode>
        // <App /> 컴포넌트를 BrowserRouter로 감싸 적용한다.
        <BrowserRouter>
          <App />
        </BrowserRouter>
      </React.StrictMode>
    );
    
    reportWebVitals();
    ```

  - screens 디렉토리에 router로 분리 되어 표시될 테스트 페이지를 home.js, contents.js 만든다.

    ``` js
    import React from "react"
    
    const Home = () => {
        return(
            <div>
                home
            </div>
        );
    }
    
    export default Home;
    ```

    ``` js
    import React from "react"
    
    const Contents = () => {
        return(
            <div>
                contents
            </div>
        );
    }
    
    export default Contents;
    ```

  - 어플리케이션 시작점인 app.js에 Route 들을 (Routes) 설정한다.

    ``` js
    import * as React from "react"
    import {Routes,Route} from "react-router-dom"
    import './App.css';
    
    import Home from "./screens/Home"
    import Contents from "./screens/Contents"
    
    function App() {
      return (
        <div className="App">
          <Routes>
            <Route path="/" element={<Home />} />
            <Route path="/contents" element={<Contents />} />
          </Routes>
        </div>
      );
    }
    
    export default App;
    ```



## 함수형 컴포넌트

- 기존의 많은 프로젝트들이 class 형식의 문법을 통하여 각 화면 안에 존재하는 컴포넌트를 구현하여 사용하였다.

- 근래의 프로젝트들은 함수형 컴포넌트를 사용함.

- prop 과 state의 범위가 보다 명확하게 보인다.

- 로그인 화면의 폼을 구현한 함수형 컴포넌트의 기본 구조는 아래와 같다.

  ``` javascript
  import React from "react"
  
  const LoginForm = () => {
      return(
          <div>
              <div>login form</div>
              <div>
                  <label>id</label>
                  <input type="text"></input>
              </div>
              <div>
                  <label>pw</label>
                  <input type="password"></input>
              </div>
              <div><button>login</button></div>
          </div>
      );
  }
  
  export default LoginForm;
  ```

- 함수형 컴포넌트에서 버튼의 이벤트를 처리하기 위하여 아래와 같이 구성한다.

  ``` javascript
  import React from "react"
  
  const LoginForm = () => {
      
      const onBtnLogin = () => {
          alert("login btn clicked");
      }
  
      return(
          <div>
              <div>login form</div>
              <div>
                  <label>id</label>
                  <input type="text"></input>
              </div>
              <div>
                  <label>pw</label>
                  <input type="password"></input>
              </div>
              <div><button onClick={onBtnLogin}>login</button></div>
          </div>
      );
  }
  
  export default LoginForm;
  ```
  
- 함수형 컴포넌트의 stete와 input box 이벤트 처리.

  ``` javascript
  import React, {useState} from "react"
  
  const LoginForm = () => {
  
      // state 정보 및 useState 함수를 이용한 내부 상태 변수 관리.
      const [formValues, setFormValues] = useState({
          userId: "",
          userPw: ""
      });
  
      const onBtnLogin = () => {
          console.log(formValues);
          alert("login btn clicked");
      }
      
      // form 구성요소의 변경시 이벤트 헨들러.
      const onInputId = (e) => { 
          setFormValues({userId:e.target.value,userPw:formValues.userPw});
      }
      
      const onInputPw = (e) => {
          setFormValues({userId:formValues.userId,userPw:e.target.value});
      }
  
      return(
          <div>
              <div className="loginFormTitle">login form</div>
              <div>
                  <label>id</label>
        					<!-- 입력 요소의 name, value, onChage 구성요소 사용 -->
                  <input type="text" name="userId" value={formValues.userId} onChange={onInputId}></input>
              </div>
              <div>
                  <label>pw</label>
                  <input type="password" name="userPw" value={formValues.userPw} onChange={onInputPw}></input>
              </div>
              <div><button onClick={onBtnLogin}>login</button></div>
          </div>
      );
  }
  
  export default LoginForm;
  ```
  
  

## 외부 CSS 파일사용

- react는 jsx 내에서 또는 각 컴포넌트 별 css를 사용하는 경우가 많음.

- 컴포넌트별 css를 사용하는 경우 build후 수정시, 소스코드를 다시 빌드해야 하므로, css 파일을 js 외부에서 사용하도록 함.

- public 디렉토리에 css 디렉토리를 생성하고, css 파일을 생성한다.

- public 디렉토리의 index.html 파일의 <head> 태그 안쪽에 작성한다.

  ``` html
  <link rel="stylesheet" href="%PUBLIC_URL%/css/global.css" />
  ```

- 내부 react.js 의 jsx 에서는 class 가 아닌 className 키워드를 이용하여 css 적용한다.

  ``` javascript
  <div className="loginFormTitle">login form</div>
  ```



## 백엔드 서버간 Restful 통신을 위한 Axios

- npm을 통한 라이브러리 설치.

  ``` sh
  npm install axios --save
  ```

- 서비스 추가 

  ``` javascript
  import axios from 'axios';
  
  const urlPrefix = "http://localhost:8080";
  
  const urlLogin = urlPrefix+"/pub/login";
  
  export function login(data) {
      return new Promise((resolve,reject)=>{
          axios({method:'POST',url:urlLogin,data:data})
          .then((response)=>{
              if(response.data.status === "fail") {
                  resolve(false);
              }
              resolve(response.data);
          })
          .catch((error) =>{
              reject(error);
          });
      })
  }
  ```

- 컴포넌트 소스에서 서비스 사용

  ``` javascript
  import React, {useState} from "react"
  // service 추가.
  import * as userService from './../services/UserService'
  
  const LoginForm = () => {
  
      const [formValues, setFormValues] = useState({
          userid: "",
          passwd: ""
      });
  
      const onInputId = (e) => { 
          setFormValues({userid:e.target.value,passwd:formValues.passwd});
      }
      
      const onInputPw = (e) => {
          setFormValues({userid:formValues.userid,passwd:e.target.value});
      }
  
      const onFormSubmit = (e) => {
          e.preventDefault();
          let data = formValues;
          // 서비스 호출 및 서비스 응답 처리
          userService.login(data)
          .then((data)=>{
              console.log(data);
              console.log("response : "+data);
          })
          .catch((error)=>{
              console.log("error : "+error);
          });
      }
  
      const onFormReset = (e) => {
          setFormValues({userid:"",passwd:""});
      }
  
      return(
          <div>
              <div className="loginFormTitle">login form</div>
              <form onSubmit={onFormSubmit}>
                  <div>
                      <label>id</label>
                      <input type="text" name="userid" value={formValues.userid} onChange={onInputId}></input>
                  </div>
                  <div>
                      <label>pw</label>
                      <input type="password" name="passwd" value={formValues.passwd} onChange={onInputPw}></input>
                  </div>
                  <div>
                      <button type="submit">login</button>
                      <button type="reset" onClick={onFormReset}>cancel</button>
                  </div>
              </form>
          </div>
      );
  }
  
  export default LoginForm;
  ```



## Redux 활용

- 리덕스를 이용하여, state 구조를 전체로 관리 한다.

- props를 통하여 전달구조를 설계하는데 실수를 줄일 수 있다.

- 리덕스 및 리덕스 관련 라이브러리를 설치한다. (redux-devtools-extension 과 redux-logger 는 개발에 도움을 주는 도구로 선택하여 설치할 수 있다.)

  ``` sh
  npm install redux react-redux redux-devtools-extension redux-logger --save
  ```

- 스토어로 사용할 리듀서(reducer.js) 생성하여 src 디렉토리에 위치시킨다. 이 파일은 src 디렉토리에 있는 index.js 에서 사용하게 된다.

- 스토어는 다양한 방식의 패턴을 통해 구현될 수 있으므로, 본 소스 코드는 기본적인 패턴으로 참고한다.

  ``` javascript
  // 초기 state 변수 및 값을 설정한다.
  const initialState = {
      testValue: 11
  };
  
  // 리듀서를 생성한다. state와 action을 가지는 함수를 parameter로 받는다.
  const reducer = (state = initialState, action) => {
      // 기존의 상태를 그대로 가져온다. (리듀서는 내부적으로 이전의 객체 상태를 바꾸어서는 안된다.)
      const newState = { ...state };
      // 액션에 따라서, 새로운 상태 변수에 변경을 가한다.
      switch(action.type) {
          case "add":
          		// 값을 1 증가.
              newState.testValue++;
              break;
          case "addSome":
          		// action의 payload 값으로 state를 설정.
              newState.testValue=action.payload;
              break;
          default:
              break;
      }
      return newState;
  };
  
  export default reducer;
  ```

- index.js 에 Provider를 이용하여 앱(<App />) 전체를 리덕스의 대상으로 설정한다.

  ``` javascript
  import React from 'react';
  import ReactDOM from 'react-dom/client';
  import { BrowserRouter } from "react-router-dom";
  import './index.css';
  import App from './App';
  import reportWebVitals from './reportWebVitals';
  // 리덕트 라이브러리를 추가한다.
  import { Provider } from 'react-redux';
  // 기존에 생성한 리듀서를 추가한다.
  import reducer from './reducer'
  // reducer 를 사용하는 스토어를 생성한다. (createStore는 deprecated 되었지만, 그대로 사용해도됨, 삭제 예정이 아님.)
  const store = createStore(reducer);
  const root = ReactDOM.createRoot(document.getElementById('root'));
  root.render(
    <React.StrictMode>
      <BrowserRouter>
        <!-- 프로바이더로 App을 지정하고, 파라미터로 스토어로 사용될 리듀서를 지정한다. -->
        <Provider store={store}>
          <App />
        </Provider>
      </BrowserRouter>
    </React.StrictMode>
  );
  
  reportWebVitals();
  ```

- 컴포넌트에서 useSelector, useDispatch 를 이용하여 만들어진 store를 이용할 수 있다.

  - useSelector는 스토어의 state를 받아서 표시하는데 사용한다.

  - useDispatch는 스토어에 선언된 액션을 수행하는데 사용하고, 키(payload) 를 이용해 state를 변경하기도 한다.

    ``` javascript
    import React, {useState} from "react"
    import * as userService from './../services/UserService'
    // useSelector, useDispatch 사용을 선언한다.
    import { useSelector,useDispatch } from "react-redux";
    
    const LoginForm = () => {
    
      	// store 로 부터 값을 가져온다.
        const testValue = useSelector((state) => state.testValue);
        // 스토어의 dispatch를 가져온다.
        const dispatch = useDispatch();
    
        const btnAddValue = (e) => {
          	// dispatch를 이용하여 action을 호출한다.
            dispatch({type:"add"});
        }
    
        const btnAddSomeValue = (e) => {
            // dispatch를 이용하여 데이터가 포함된 action을 호출한다.
            dispatch({type:"addSome",payload:33});
        }
    
        return(
            <div>
                <div className="loginFormTitle">login form</div>
          		  <!-- 현재 redux의 데이터 표출한다 -->
                <div>{testValue}</div>
                <div>
                    <button onClick={btnAddValue}>redux test add value.</button>
                </div>
                <div>
                    <button onClick={btnAddSomeValue}>redux test add some value.</button>
                </div>
            </div>
        );
    }
    
    export default LoginForm;
    ```

    


## 디렉토리 구조

- public
  - css

- src
  - components
  - screens
  - Services


## Trouble shooting

- 설치중 power shell 멈춤.
  - 다양한 이유로 설치중 멈춤 현상 발생.
  - 터미널 종료 후 재부팅(윈도우 업데이트의 영향을 반영하기 위하여.)하고, node.js 메뉴에서 install additional tools for node.js 를 선택하여. 재 설치 시작.