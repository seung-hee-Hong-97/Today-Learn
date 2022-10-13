# [React] Context API

컴포넌트들이 많아지면 여러 레벨을 거쳐 최상위 컴포넌트에서 `props`로 원하는 상태와 함수를 전달 하였지만, 이렇게 되면 `props` 가 필요하지 않은 컴포넌트들도 하위 컴포넌트에 값을 전달하기 위해 어쩔 수 없이 `props` 를 받아야 하는 경우가 생깁니다.

`Context API`를 사용하게 되면 하위 컴포넌트로 Props를 전달하지 않고, 전역적으로 값을 전달할 수 있게 됩니다.

`Context API` 뿐만 아니라, `Redux` 및 `Mobx`와 같은 상태 관리 라이브러리를 사용할 수도 있지만 복잡하기 때문에 학습 난이도가 높습니다.



## Context API 사용법 예시 

### `1. createContext() 함수를 사용하여 Context 생성`

- `src` 디렉토리 하위에 `contexts` 디렉토리를 생성하고 하위에 `UserContext.tsx` , `UserContextProvider.tsx` 파일 생성

``` typescript
/* UserContext.tsx */

import { createContext } from 'react';

const UserContext = createContext({
  loggedIn: false,
  setLoggedInHandler: (isLogin: boolean) => {}
});

export default UserContext;
```

```jsx
/* UserContextProvider.tsx */

import { useState } from 'react';
import UserContext from './UserContext';

const UserContextProvider = ({ children }: { children: React.ReactNode }) => {
  const [loggedIn, setLoggedIn] = useState<boolean>(false);
  const setLoggedInHandler = (isLogin: boolean) => setLoggedIn(isLogin);
  
  return (
    <UserContext.Provider value={{ loggedIn, setLoggedInHandler }}>
      {children} 
    </UserContext.Provider>
  );
};

export default UserContextProvider;
```



### `2. 최상위 컴포넌트에서 Provider로 래핑`

- `Context API`의 데이터에 접근해야 하는 하위 컴포넌트를 return 내부에서 Provider로 래핑
- 전달할 데이터는 `value = { }` 안에 겍체 형태로 넣음

``` jsx
/* App.tsx */

import { BrowserRouter, Route, Routes } from 'react-router-dom';
import Home from '@pages/Home';
import Login from '@pages/Login';

const App = () => {
  return (
    <Router>
      <Routes>
        <Route path="/" element={<Home/>}>
          <Route path="/login" element={<Login />} />
        </Route>
      </Routes>
    </Router>
  );
};

export default App;
```

``` jsx
/* Home.tsx */

import UserContextProvider from '@contexts/User/UserContextProvider';
import Main from '@pages/Main';

const Home = () => {
  return (
    <UserContextProvider>
      <Main/>
    </UserContextProvider>
  );
};

export default Home;
```



### `3. 하위 컴포넌트에서 context API 설정`

- 위에 설정했던 `UserContext`를 Import 하여 사용
- `useContext` hook을 사용하여 value 값을 가져옴

``` jsx
/* Login.tsx */

import React, { useContext, useState } from 'react';
import UserContext from '@contexts/User/UserContext';

const Login = () => {
  const [id, setId] = useState<string>('');
  const [pwd, setPwd] = useState<string>('');
  const {loggedIn, setLoggedInHandler } = useContext(UserContext);
  
  const onLogin = () => {
    // 로그인 성공 시
    setLoggedInHandler(true);
  };
  
  return (
    <>
      <form onSubmit={(e: React.FormEvent) => {
        e.preventDefault();
        onLogin();
        }}
      >
        <input type="text" value={id} onChange={(e: React.ChangeEvent<HTMLInputElement>) =>  
          setId(e.target.value)} />
        <input type="password" value={pwd} onChange={(e: React.ChangeEvent<HTMLInputElement) =>
         setPwd(e.target.value)} />
      
        <button>로그인</button>
      </form>
      
      {loggedIn && <div>로그인 완료</div>}
  );
};

export default Login;
```

> 로그인에 성공했을 시, `setLoggedInHandler`를 통해 `loggedIn` 값을 true로 설정하여, `로그인 완료` 라는 문구가 보여지도록 하는 코드입니다.



## 여러 Context를 병합하여 사용

React 프로젝트를 진행할 때, 전역적으로 값을 공유해야 하는게 많아질 경우 `Context`를 여러 개 등록해야 하는 경우가 생깁니다.

Context API는 Provider를 래핑하는 방식으로 구현되는데, 여러 Provider를 계속 래핑 하다 보면 코드의 가독성도 좋지 않게 되고 적용하는 위치도 애매해 질 수 있습니다.

따라서 적용되는 모든 Provider를 병합하여 하나의 Provider로 설정하게 되면 래필도 딱 한번만 해주면 되고 앞서 언급한 단점들을 상쇄시킬 수 있습니다.



### 적용 예시

1. `AppProviderProps` 인터페이스 생성

``` typescript
/* interfaces/common.ts */

export interface AppProviderProps {
  contexts: Array<React.JSXElementConstructor<React.PropsWithChildren<any>>>;
  children: React.ReactNode;
}
```

> - `contexts` : 병합할 Context들
> - `children` : 하위 컴포넌트



2. `AppProvider.tsx` 파일 생성

```jsx
/* AppProvider.tsx */

import { AppProviderProps } from '@interfaces/common';

const AppProvider = ({ contexts, children }: AppProviderProps) => {
  return (
    <>
      {contexts.reduce((prev, Context) => {
        return <Context>{prev}</Context>;
      }, children)}
    </>
  );
};

export default AppProvider;
```



3. 최상위 컴포넌트에 AppProvider.tsx 파일 래핑

``` jsx
/* Home.tsx */

import AppProvider from '@contexts/Appprovider';
import ModalProvider from '@contexts/Modal/ModalProvider';
import UserContextProvider from '@contexts/User/UserContextProvider';

import Main from './Main';

const Home = () => {
  return (
    <AppProvider
      contexts={[UserContextProvider, ModalProvider]}
    >
      <Main />
    </AppProvider>
  )
}
```

> 위의 코드처럼 Provider를 추가할 때 마다 래핑하지 않고,  `contexts`에 배열의 요소로 추가하여 사용할 수 있게 됐습니다.
