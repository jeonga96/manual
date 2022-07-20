# axios manual

service, string에서 export하고 있는 함수 및 변수들을 설명하고, 그에 따른 사용방법을 작성하였다.

## importData

기능이 들어간 함수는 다른 파일에서 import하여 사용하기 위해 `service/importData.js`에 작성한다.

### Storage에 자료 넣기, 찾기, 제거하기

- importData

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

- 사용예시

```
setStorage("KeyName", "value");

getStorage("keyName");

removeStorage("keyName");
```

### exios를 사용하여 서버에서 데이터 가져오기 & 등록하기

- importData

```
export function axiosGetData(url, getData) {
  return axios(url, {
    method: "GET",
    data: getData,
  })
    .then((res) => res.data)
    .catch((error) => console.log(error));
}

export function axiosSetData(url, postData) {
  return axios(url, {
    method: "POST",
    headers: {
      "content-type": "application/json",
    },
    data: postData,
  })
    .then((res) => res.data)
    .catch((error) => console.log(error));
}
```

- 사용 예시

```
axiosGetData("http://", data);

axiosSetData("http://", data);
```

### login 기능

- import Data

```
export function loginEvent(loginUrl, userData) {
  return axios({
    method: "POST",
    url: loginUrl,
    data: {
      userid: userData.userid[0],
      passwd: userData.passwd[0],
    },
    headers: {
      "Content-Type": "application/json",
    },
  })
    .then((res) => {
      if (res.data.status === "fail") {
        alert("회원가입을 먼저 진행해 주세요.");
        return;
      }
      if (res.data.status === "success") {
        const accessToken = res.data.data.jtoken;
        setStorage(ISLOGIN, `${accessToken}`);
        window.location.href = `${process.env.PUBLIC_URL}/`;
        return console.log(accessToken);
      }
    })
    .catch((error) => console.log(error.response));
}
```

- 사용예시

  - loginEvent 함수 사용 예시

  ```
  loginEvent(loginUrl, login);
  ```

  - loginEvent를 활용한 loginFn 함수 사용 예제

  ```
  const fnLogin = (e) => {
    e.preventDefault();
    if (login.userid === "" || login.passwd === "") {
      return alert("아이디와 비밀번호를 입력해 주세요!");
    }
    loginEvent(loginUrl, login);
  };
  ```

## string

service 혹은 프로젝트에 필요한 url 및 strin 변수는 `/service/string.js`에 작성한다.

```
export const loginUrl = "http://devback.gongsacok.com:8080/pub/login";

export const ISLOGIN = "is_login";
```
