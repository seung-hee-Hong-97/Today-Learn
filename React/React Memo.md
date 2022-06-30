# [React] Memo

React는 먼저 컴포넌트를 렌더링 한 후에 이전 렌더링된 결고와 비교하여 DOM 업데이트를 결정합니다.

만약 렌더링된 결과가 이전과 다르다면, DOM을 업데이트합니다.

컴포넌트가 `React.memo()` 로 래핑되면 컴포넌트를 렌더링하고 그 결과를 기억합니다. 그리고 다음 렌더링이 일어날 때 `props` 가 같다면, 재 렌더링을 하지 않고 기억해뒀던 내용을 재 사용합니다.

### 사용 예시

``` javascript
import React from 'react';

export function Movie({title, releaseDate}) {
  return (
    <>
      <div>Movie Title: {title}</div>
      <div>Release Date:  {releaseDate}</div>
    </>
  );
}

export const MemoizedMovie = React.memo(Movie);

/*
React.memo(movie)는 새롭게 메모이징된 컴퍼넌트인 memoizedMovie를 반환합니다.
```

> 만약 `title` 혹은 `releaseDate` 와 같은 props가 변경되지 않는다면 다음 렌더링 때 재 렌디렁되는 것이 아닌 메모이징(기억)된 내용을 그대로 사용하게 됩니다.

이런 식으로 메모이징한 결과를 재사용하게 됨으로써 React에서 Virtual DOM에서 달라진 부분을 확인하지 않기 때문에 성능상 이점을 가질 수 있습니다.



## React.Memo() 사용하기 좋은 경우

기본적으로 React.Memo( )는 함수형 컴포넌트에 적용되어 같은 props에 같은 렌더링 결과를 제공합니다.

Memo를 사용하기 가장 좋은 경우는 함수형 컴포넌트가 같은 props로 자주 렌더링 되는 경우에 사용하면 좋습니다.

``` javascript
// 위에 작성된 Movie 컴포넌트를 사용하여 부모 컴포넌트인 MovieViewsRealTime 컴포넌트 작성

function MovieViewsRealTime({title, releaseDate, views}) {
  return (
    <div>
      <Movie title={title} releaseDate={releaseDate} />
      Movie views: {views}
    </div>  
  )
}
```

> 이 컴포넌트 코드는 주기적으로 서버에서 데이터를 폴링해서 `MovieViewsRealtime` 컴포넌트의 `views` 를 업데이트 합니다.

```javascript
// 초기 렌더링
<MovieViewsRealTime views={0} title="Hello, World" releaseDate="2022.06.24" />

// 1초 뒤, views: 10
<MovieViewsRealTime views={10} title="Hello, World" releaseDate="2022.06.24" />

// 2초 뒤, views: 25
<MovieViewsRealTime views={25} title="Hello, World" releaseDate="2022.06.24" />
```

> `views`가 새로운 값으로 업데이트 될 때 마다 `MovieViewsRealTime` 컴포넌트 또한 re-render 됩니다.
>
> 이 때 `Movie`컴포넌트 또한 title 혹은 releaesDate가 같음에도 불구하고 re-render되는데 이 때 `Movie`컴포넌트에 메모이제이션을 적용하면 불필요한 re-render를 하지 않게 됩니다.

### 메모이제이션 적용 코드

``` javascript
import React from 'react';

function MovieViewsRealTime({title, releaseDate, views}) {
  return (
    <div>
      <MemoizedMovie title={title} releaseDate={releaseDate} />
      Movie views: {views}
    </div>
  )
}
```

> title 혹은 releaseDate props가 같다면, MemoizedMovie를 re-render하지 않고 기억해뒀던 내용을 재사용합니다.



## React.Memo()를 사용하기 안좋은 경우

이전 렌더링 결과와 비교하였을 때 props가 동일하지 않아 새로운 값으로 렌더링 되는 상황이라면 `React.Memo()` 를 사용할 필요가 없습니다.

오히려 성능 관련 변경이 잘못 적용된다면 오히려 성능적으로 악화될 가능성이 높습니다.

또한 기술적으로 불가능한 것은 아니지만 클래스 기반 컴포넌트에 `React.Memo()` 로 래핑하는 것은 적절하지 않습니다. 만일 클래스 기반의 컴포넌트에서 메모이제이션이 필요하다면 `PureComponent`를 확장하여 사용하거나 `shouldComponentUpdate()` 메서드를 구현하는 것이 더 바람직합니다.

> props가 자주 변하는 컴포넌트, 즉 메모이제이션을 적용할 필요가 없는 컴포넌트일 경우 `React.Memo()`로 래핑할지라도, React는 2가지의 작업을 re-render 할 때 마다 수행합니다.
>
> 1. 이전 props와 다음 props의 동등 비교를 위해 비교 함수를 실행
> 2. 비교 함수는 거의 항상 false를 반환할 것 이기 때문에(= 이전 props와 다음 props의 값이 다르므로) React는 이전 렌더링 내용과 다음 렌더링 내용을 비교할 것 입니다.





