# admin manual

- service, string에서 export하고 있는 함수 및 변수들을 설명하고, 그에 따른 사용방법을 작성하였다.
- 전역적으로 사용하는 reducer의 데이터를 설명하고, 그에 따른 사용방법을 작성하였다.

## importData

기능이 들어간 함수는 다른 파일에서 import하여 사용한다.

위치 : `service/importData.js`

### 작성예시 - Storage에 자료 넣기, 찾기, 제거하기

```
export function setStorage(name, data) {
  return sessionStorage.setItem(name, data);
}

export function getStorage(name) {
  return sessionStorage.getItem(name);
}

export function removeStorage() {
  return sessionStorage.removeItem(ISLOGIN);
}
```

### 사용예시

```
setStorage("KeyName", "value");

getStorage("keyName");

removeStorage("keyName");
```

### 작성예시 - axios를 사용하여 data 핸들링

**데이터 요청하기**

```
export function axiosGetData(url, getData) {
  return axios(url, {
    method: "GET",
    data: getData,
  })
    .then((res) => res.data)
    .catch((error) => console.log(error));
}

export function axiosPostData(url, postData) {
  return axios(url, {
    method: "POST",
    headers: {
      "content-type": "application/json",
    },
    data: postData,
  })
    .then((res) => {
      console.log("importData.axiosSetData", res);
      return res.data;
    })
    .catch((error) => console.log(error));
}
```

**header에 token을 입력하여 요청하기**

```
export function axiosGetToken(url, getData, token) {
  return axios(url, {
    method: "GET",
    data: getData,
    headers: {
      Authorization: `Bearer ${token}`,
      "Content-Type": "application/json",
    },
  })
    .then((res) => res.data)
    .catch((error) => console.log("에러가 났어욥", error));
}

export function axiosPostToken(url, postData, token) {
  return axios({
    baseURL: url,
    method: "POST",
    headers: {
      Authorization: `Bearer ${token}`,
      "Content-Type": "application/json",
    },
    withCredentials: false,
    data: postData,
  })
    .then((res) => {
      console.log("importData.axiosSetData", res);
      return res.data;
    })
    .catch((error) => console.log(error));
}

```

### 사용 예시

메뉴얼을 확인하여 데이터의 키를 파악한 후 요청을 한다.

```
axiosGetData("http://", data);

axiosPostData("http://", data);

axiosGetToken("http://", data, token)

axiosPostToken("http://", data, token)
```

## string.js

service 혹은 프로젝트에 필요한 url 및 string 타입 데이터는 const에 할당하여 한 파일에 작성한다.

위치 : `/service/string.js`

### 작성예시

```
export const loginUrl = "http://devback.gongsacok.com:8080/pub/login";

export const ISLOGIN = "is_login";
```

### 사용예시

```
import { loginUrl, ISLOGIN } from "../service/string";

console.log(loginUrl, ISLOGIN); // http://devback.gongsacok.com:8080/pub/login, is_login
```

## reducer

전역적으로 필요한 데이터는 redux를 사용하여 전역적으로 사용할 수 있도록 한다.

위치 : `./reducer.js`

### 작성예시

```
const initialState = {
  login: { userid: "", passwd: "" },
};

const reducer = (state = initialState, action) => {
  const newState = { ...state };
  // initialState를 직접적으로 변경하는 것이 불가능하기 때문에 스프레드 연산자를 통하여 깊은 복사하여 진행한다.

  switch (action.type) {
    case "loginEvent":
    // action이 loginEvent일 때 실행된다.

      console.log("loginEvent가 발생했습니다!");
    break;

    case "userInfoInputChange":
    // action이 userInfoInputChange 때 실행된다.

      newState.login = action.payload;
      // userInfoInputChange의 payload 값을 newState의 userInfo에게 할당된다.
    break;

  }
  return newState;
};

export default reducer;

```

### 사용예시

```
import { useSelector, useDispatch } from "react-redux";

function Login() {
  const login = useSelector((state) => state.login);
  const dispatch = useDispatch();

  function onChange(e) {
    dispatch({
      type: "userInfoInputChange",
      payload: { ...login, [e.target.id]: [e.target.value] },
    });
  }

  const fnLogin = (e) => {
    dispatch({
      type: "loginEvent",
    });
  };

  return (
        <form onSubmit={fnLogin}>
          <input
            type="text"
            id="userid"
            onChange={onChange}
          />
          <label htmlFor="userid">
            아이디를 입력해 주세요.
          </label>
        </form>
  );
}
```
