# React PropTypes

React는 기본적으로 Javascript를 사용하는데, 이는 유연한 특성 때문에 작성이 편하지만, 파일이 많아지면 생산성이 떨어지고 타입에 대한 엄격함이 떨어져 버그를 잡기 힘든 점이 있습니다.

Typescript를 사용하면 이 문제를 겪지 않아도 되지만, 만약 꼭 Javascript를 통해 개발을 해야하는 상황이라면 `PropTypes`를 활용하는 것을 권장합니다.

`PropTypes` 는 부모로부터 전달받은 prop의 데이터 타입을 검사합니다. 자식 컴포넌트에서 명시해 놓은 데이터 타입과 부모로부터 넘겨받은 데이터 타입이 일치하지 하지 않으면 콘솔에 에러 경고문을 띄웁니다.

## 사용 예시

```javascript
import PropTypes from 'prop-types';

Testing.propTypes = {
  name : PropTypes.string
};

const Testing = ({name}) => {
  return (
    <h1>Hello, {name}</h1>
  );
}
```

> 위의 코드는 부모에게서 받아온 `name` 이라는 props가 string 타입이 아닌 숫자, 함수 또는 객체 등 다른 형태면 콘솔에 에러 메시지가 출력됩니다. (개발 모드에서만 출력된 것 확인 가능)
>
> 이렇게 콘솔에 에러 메시지가 출력되는 이유는 React가 렌더링하는 과정에서 잘못된 속성 값 타입을 검사해주기 때문입니다.



Props를 사용함으로써 생기는 또 하나의 장점은 타입 정의만으로도 가독성이 좋아지고 좋은 문서가 될 수 있습니다.

타입을 정의하지 않은 코드와 `propTypes` 를 이용해 속성 값의 타입 정보를 정의한 코드를 비교해보겠습니다.



## 타입 정의하지 않은 코드

``` javascript
const User = ({type, age, male, onChangeName, onChangeTitle}) => {
  const onClick1 = () => {
    const msg = `type: ${type}, age: ${age ? age: 'unknown'}`
  };
  
  const onClick2 = () => {
    if (onChangeName) {
      onChangeName(name);
    }
    
    onChangeTitle(title);
  };
};
```

> 코드를 얼핏봐도 가독성이 좋지 않고 타입이 뭔지 확인하기 위해 일일이 코드를 뜯어서 봐야 합니다.



## PropTypes 이용한 코드

``` javascript
/* 
1. isRequired가 붙으면 필수값이라는 의미입니다. 
2. oneOf는 인자로 들어가는 배열에 포함된 값 중에서 하나를 만족해야한다는 의미입니다.
*/

User.propTypes = {
  male: PropTypes.boolean.isRequired,  // 필수값
  age: PropTypes.number,
  type: PropTypes.oneOf(['gold', 'silver', 'bronze']),
  onChangeName: PropTypes.func,
  onChangeTitle: PropTypes.func.isRequired  // 필수값
}

const User = ({type, age, male, onChangeName, onChangeTitle}) => {
  // Code...
}
```

> 이렇게 타입 지정을 해놓으면 필수값을 보내지 않거나, 타입이 다른 값을 보내면 에러가 발생합니다.



## 기본으로 제공되는 PropTypes로 정의하는 타입들

``` javascript
MyComponent.propTypes = {
  // React 요소
  menu: PropTypes.element,
  
  // 컴포넌트 함수가 반환할 수 있는 모든 것 (권장 X)
  description: PropTypes.node,
  
  // Message 클래스로 생성된 모든 객체
  message: PropTypes.instanceOf(Message),
  
  // 배열에 포함된 값 중에서 하나를 만족
  name: PropTypes.oneOf(['kim', 'lee', 'park']),
  
  // 배열에 포함된 타입 중에서 하나를 만족
  width: PropTypes.oneOfType([PropTypes.number, PropTypes.string]),
  
  // 특정 타입만 포함하는 배열
  ages: PropTypes.arrayOf(PropTypes.number),  // 타입이 숫자인 값만 배열에 포함되어야 함
  
  // 객체의 속성값 타임을 정의
  info: PropTypes.shpae({
    color: PropTypes.string,
    weight: PropTypes.number
  }),
  
  // 객체에서 모든 속성값의 타입이 같아야 함
  infos: PropTypes.objectOf(PropTypes.number)  // 객체의 속성 값의 타입이 모두 숫자여야 함
}
```

> 더 많은 PropTypes를 통해 정의하는 타입을 살펴보려면 아래의 공식문서 참조!
>
> [공식 문서](https://ko.reactjs.org/docs/typechecking-with-proptypes.html#proptypes)



## 커스텀 타입 함수

`PropTypes`가 제공하는 기본적인 타입 정의 함수를 이용하면 웬만한 타입 정보는 표현할 수 있습니다.

또한 사용자 정의 유효성 검사기를 지정할 수도 있습니다. 만일 검사 실패 시에는 Error 객체를 반환해야 합니다.

``` javascript
MyComponent.propTypes = {
  age: function (props, propName, componentName) {
    const value = prop[propName];
    
    if (value < 10 || value > 20) {
      return new Error(
        `Invalid prop ${propName} supplied to ${componentName}`;
      )
    }
  }
}