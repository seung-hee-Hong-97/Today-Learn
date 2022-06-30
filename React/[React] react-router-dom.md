# [React] react-router-dom

2021년 11월에 `리액트 라우터`의 버전이 5에서 6으로 업데이트 되었습니다.

이 글을 작성하는 2022년 6월 기준으로 저는 V6를 다운받아서 사용하고 있습니다.

이 글에선 `react-router-dom`에 대한 설명과 이전 버전과 달라진 점에 대해서 다루도록 하겠습니다.



## 1. 설치 방법

Node.js 패키지 매니저인 `npm`을 사용하는 방법과 Javascript 패키지 매니저 중 하나인 `yarn`을 사용하는 방법이 있습니다.

저는 `npm`을 사용하여 설치하였고, 설치 방법은 아래와 같이 터미널에 명령어를 입력하면 됩니다.

``` bash
$ npm install react-router-dom
```

> 이 글을 작성하는 시점으로 가장 최신 버전인 6.3.0이 설치됩니다.



## 2. 사용방법

기본적으로 React 프로젝트에서 라우팅을 담당하는 곳은 `App.js`파일에서 이루어집니다

`react-router-dom`을 사용하기 위해선 파일 상단에 아래와 같이 모듈을 추가시켜줘야 합니다.

``` javascript
import { BrowserRouter, Routes, Route } from 'react-router-dom';
```



위와 같이 모듈을 추가시켜준 뒤에 다음과 같은 형식으로 작성합니다.

``` javascript
function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path='/' element={<Home />} />
      </Routes>
    </BrowserRouter>
  )
}

export default App;
```

> `Route` 
>
> - 브라우저 주소 경로에 따라 원하는 컴포넌트를 보여주는 기능을 하는 컴포넌트
> - 항상 `Routes` 컴포넌트 내부에서 사용해야 합니다.



## 3. Switch 대신 Routes 쓸 때의 변경 사항

- `Switch`의 네이밍이 `Routes`로 변경
- `exact`  옵션 삭제
- component 방식 변경 (`component={COM}` 및 `render={() => <h1> Test </h1>}` 삭제)
- path를 기존의 `path="web/:id"` 에서 `path=":id"`로, 상대 경로를 통해 지정



### 기존의 V5까지의 방식

``` javascript
import { BrowserRouter, Switch, Route } from 'react-router-dom';
import { Home } from './routes/Home';
import { Detail } from './routes/Detail';

function App() {
  return (
    <BrowserRouter>
      <Switch>
        <Route path="/" component={() => <Home /> } />
        <Route exact path="/Detail" component={() => <Write />} />
      </Switch>
    </BrowserRouter>
  )
}

export default App;
```

> - `Switch`를 사용합니다.
> - `exact`를 사용하여 복수의 라우팅을 막습니다.
> - `component={ }` 내부에 화살표 함수를 사용하여, component를 전달합니다.



### V6 방식

``` javascript
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import { Home } from './routes/Home';
import { Detail } from './routes/Detail';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/Detail/*" element={<Detail />} />
      </Routes>
    </BrowserRouter>
  )
}

export default App;
```

> - `exact`를 더 이상 사용하지 않고, 여러 라우팅을 매칭하고 싶을 경우 URL 뒤에 `*`를 사용합니다.
> - `component` 대신 `element`를 사용하여 바로 Component를 전달합니다.



## 4. Link 컴포넌트를 활용해 다른 페이지로 이동

웹 페이지에서 기본적으로 링크를 보여주고, 링크를 타서 이동할 때 사용하는 것은 `<a>` 태그 입니다.

React Router에서는 `<a>` 태그를 사용하지 않고, `<Link>` 컴포넌트를 사용합니다. 왜냐하면 `<a>` 태그를 사용하여 페이지 이동을 하게 되면 브라우제서 페이지를 새로 불러와 새로고침이 되기 때문에 SPA의 취지에 어긋나게 됩니다

`<Link>` 컴포넌트는 `<a>` 태그를 내장하고 있지만, 페이지를 새로 불러오는 것을 막고 History API를 통해 브라우저 주소의 경로만 바꾸는 기능이 내장되어 있습니다.



### 사용 예시

`Home` 화면에서 `About` 화면으로 이동하는 기능을 `Link` 컴포넌트를 사용해서 구현해보겠습니다.

``` javascript
import { Link } from 'react-router-dom';

function Home() {
 return (
   <div>
     <h1>Home</h1>
     <p>Home 화면입니다.</p>
     <Link to="/about">About 페이지로 이동!</Link>
   </div>
 );
}

export default Home;
```

> `<Link to="경로">` 를 통해 원하는 화면으로 이동할 수 있습니다.



## 5. URL 파라미터 

React에서 URL 파라미터는 `useParams` 라는 Hook을 사용해 객체 형태로 조회할 수 있습니다.

URL 파라미터의 이름으로는 Route를 설정할 때 `Route` 컴포넌트의 `path` props를 통하여 설정합니다.



### URL 파라미터 설정

``` javascript
import { Link } from 'react-router-dom';

function Home() {
  // 위의 코드와 동일
  <Link to="/about/:id"> About 페이지로 이동! </Link>
}

export default Home;
```

> URL 파라미터는 `/about/:id` 와 같이 경로에 `:(콜론)` 을 사용하여 설정합니다.
>
> 만약 URL 파라미터가 여러개인 경우엔 `/about/:id/:name` 과 같은 형태로 설정할 수 있습니다. 



### URL 파라미터 받기

``` javascript
import { useParams } from 'react-router-dom';

function About() {
  const { id } = useParams();
  
  return (
    <div>
      <h1>About</h1>
      <p> About 페이지입니다.</p>
      <p>페이지 Id는 {id}입니다. </p>
    </div>
  );
}

export default About;
```



## 6. 쿼리 스트링

쿼리스트링을 사용할 때는 URL 파라미터와 다르게 `Route` 컴포넌트를 사용할 때 별도로 설정해야 되는 것은 없습니다.

``` javascript
import { useLocation } from 'react-router-dom';

function About() {
  const location = useLocation();
  
  return (
    <div>
      <h1> About </h1>
      <p> About 페이지입니다. </p>
      <p> 쿼리 스트링 예제: {location.search}</p>
    </div>
  );
}

export default About;
```



위 예제에서 `useLocation` 이라는 Hook을 사용했늗네, 이 Hook은 `location` 객체를 반환하며 이 객체는 현재 사용자가 보고 있는 페이지의 정보를 담고 있습니다.

이 객체에는 아래와 같은 값들이 있습니다.

- `pathname` : 현재 주소의 경로 (쿼리 스트링 제외)
- `search` : 맨 앞의 `?` 문자를 포함한 쿼리스트링 값
- `hash` : 주소의 `#` 문자열 뒤의 값
- `state` : 페이지로 이동할 때 임의로 넣을 수 있는 값
- `key` : `location` 객체의 고유 값, 초기에는 `default` 이며 페이지가 변경될 때 마다 고유의 값이 생성됨



쿼리 스트링은 `location.search` 를 통해서 조회할 수 있습니다. 주소창에 아래와 같은 URL을 입력해봅니다.

``` bash
http://localhost:3000/about?detail=true&mode=1
```

> 출력된 값은 `?detail=true&mode=1`이 됩니다.

이렇게 되면 `?` 와 `&` 을 기준으로 문자열을 분리한 뒤 key와 value를 파싱하는 작업을 해야하는데, React Router V6부터는 `useSearchParams` 라는 Hook을 이용하여 쉽게 파싱할 수 있습니다.

``` javascript
import { useSearchParams } from 'react-router-dom';

function About() {
  const [searchParams, setSearchParams] = useSearchParams();
  const detail = searchParams.get('detail');
  const mode = searchParams.get('mode');
  
  return (
    <div>
      <h1>About</h1>
      <p>detail: {detail}</p>
      <p>mode: {mode}</p>
    </div>
  );
}

export default About;
```

`useSearchParams`는 배열 타입의 값을 반환하며, 첫번째 원소는 Query Parameter를 조회하거나 수정하는 메서드들이 담긴 객체를 반환합니다.

`get` 메서드를 통해 특정 Query Parameter를 조회할 수 있고, `set` 메서드를 통해 특정 Query Parameter를 업데이트 할 수 있습니다 

만일 조회 시에 Query Parameter가 존재하지 않는다면 `null`로 조회됩니다.

두번째 원소로는 Query Parameter를 객체 형태로 업데이트 할 수 있는 함수를 반환합니다.



### 주의할 점

- Query Parameter를 조회할 때 값은 무조건 문자열 타입
- `true` 혹은 `false` 와 같은 Boolean 타입의 값을 넣어 비교할 때 꼭 따옴표로 감싸서 비교
- 숫자를 다루게 된다면 `parseInt` 를 사용하여 숫자 타입으로 변환 후 다뤄야 함



## 7. useNavigate

`useNavigate` Hook은 `Link` 컴포넌트를 사용하지 않고, 다른 페이지로 이동해야 할 때 사용합니다.

``` javascript
import { useNavigate } from 'react-router-dom';

function Layout() {
  const navigate = useNavigate();
  
  const goBack = () => {
    navigate(-1); // 이전 페이지로 이동
  }
  
  const goArticle = () => {
    navigate('/article'); // article 경로로 이동
  }
  
  return (
    <div>
      <h1> Layout page </h1>
      <p> 아래의 버튼을 눌러보세요.</p>
      <button onClick={goBack}>뒤로가기 버튼</button>
      <button onClick={goArticle}>Article 페이지로 이동</button>
    </div>
  );
}

export default Layout;
```



다른 페이지로 이동할 때 `replace` 라는 옵션이 있는데, 이 옵션을 사용하게 되면 페이지를 이동할 때 현재 페이지를 기록에 남기지 않습니다. 위의 `goArticle` 함수에 `replace` 옵션을 추가해보겠습니다. 

``` javascript
const goArticle = () => {
  navigate('/article', {replace: true});
}
```

