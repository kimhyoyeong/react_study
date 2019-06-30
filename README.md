# 4. props와 state

### 1) props

> props 는 부모 컴포넌트가 자식 컴포넌트에게 주는 값입니다. 
>
> 자식 컴포넌트에서는 props 를 받아오기만하고, 받아온 props 를 직접 수정 할 수 는 없습니다.

이미지 필요

```react
<Child Value="Value" />
```

[예시](https://codesandbox.io/s/react-basics-42y0x)



Myname 컴포넌트 생성

```react
import React, { Component } from 'react';

class Myname extends Component {
  render() {
    return (
      <div>
        안녕하세요! 제 이름은 <b>{this.props.name}</b> 입니다.
      </div>
    );
  }
}

export default Myname;
```

**this.props.name 으로 조회하여 사용할수있다.**



App.js 에서 Myname컴포넌트 사용하기

```react
import React, { Component } from 'react';
import Myname from './Myname';

class App extends Component {
  render() {
    return(
      <Myname name="리액트" />
    )
  }
}

export default App;
```



#### **defalutProps**

props의 기본값 

(최신 방법)

```react
import React, { Component } from 'react';

class MyName extends Component {
  static defaultProps = {
    name: '기본이름'
  }
  render() {
    return (
      <div>
        안녕하세요! 제 이름은 <b>{this.props.name}</b> 입니다.
      </div>
    );
  }
}

export default MyName;
```



알아만두기 -  최신 바벨이 아래 처럼 자동으로 변경해준다.

```react
import React, { Component } from 'react';

class MyName extends Component {
  render() {
    return (
      <div>
        안녕하세요! 제 이름은 <b>{this.props.name}</b> 입니다.
      </div>
    );
  }
}

MyName.defaultProps = {
  name: '기본이름'
};

export default MyName;
```



#### 함수형 컴포넌트

> 이렇게 단순히 props 만 받아와서 보여주기만 하는 컴포넌트의 경우엔 더 간편한 문법으로 작성할 수 있는 방법
>
> 함수형 컴포넌트와 클래스형 컴포넌트의 주요 차이점은, **우리가 조만간 배우게 될 state 와 LifeCycle 이 빠져있다는 점입니다**. 그래서, 컴포넌트 초기 마운트가 아주 미세하게 빠르고, 메모리 자원을 덜 사용합니다. 미세한 차이이니, 컴포넌트를 무수히 많이 렌더링 하게 되는게 아니라면 성능적으로 큰 차이는 없습니다. 

```react
//import React, { Component } from 'react';
import React from 'react';

const MyName = ({ name }) => {
  return (
    <div>
      안녕하세요! 제 이름은 {name} 입니다.
    </div>
  );
};

//defaultProps 사용
MyName.defaultProps = {
  name: '기본이름'
};

export default MyName;
```

[비구조화 할당 문법](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)



### 2) state

> 동적인 데이터를 다룰 때 사용
>
> 컴포넌트 내부에서 선언하며 내부에서 값을 변경 할 수 있습니다. 
>
> 변경 할때는 언제나 setState() [컴포넌트 내장함수]라는 함수를 사용한다.

이미지 필요

#### - state 정의

```react
import React, { Component } from 'react';

class Counter extends Component {
    /*
    constructor(props) {
    super(props);
    this.state = {
      number: 0
    }
    */
    
    state = {
    	number: 0
 	};
}
```



#### - 메소드 작성

> setState()를 사용하여 업데이트해줌

```react
 handleIncrease = () => {
     this.setState({
         number: this.state.number + 1
     });
 }

 handleDecrease = () => {
     this.setState({
         number: this.state.number - 1
     });
 }   
```



**화살표 함수를 사용하지 않을경우**

(이렇게 하면, 나중에 버튼에서 클릭이벤트가 발생 했을 때, this 가 undefined 로 나타나서 제대로 처리되지 않게 됩니다. 이는 함수가 버튼의 클릭이벤트로 전달이 되는 과정에서 "this" 와의 연결이 끊겨버리기 때문인데요, 이를 고쳐주려면 constructor 에서 설정을 다시 해줘야함)

```react
handleIncrease() {
    this.setState({
        number: this.state.number + 1
    });
}

handleDecrease() {
    this.setState({
        number: this.state.number - 1
    });
}

constructor(props) {
    super(props);
    this.handleIncrease = this.handleIncrease.bind(this);
    this.handleDecrease = this.handleDecrease.bind(this);
}
```

#### - setState

> 자 이제 각 메소드에 들어있는 `this.setState` 에 대해서 알아봅시다. state 에 있는 값을 바꾸기 위해서는, this.setState 를 무조건 거쳐야합니다. 리액트에서는, 이 함수가 호출되면 컴포넌트가 리렌더링 되도록 설계되어있습니다.
>
> setState 는, 객체로 전달되는 값만 업데이트를 해줍니다.



#### 이벤트 설정

```react
render() {
    return (
        <div>
            <h1>카운터</h1>
            <div>값 : {this.state.number}</div>
            <button onClick={this.handleIncrease}>+</button>
            <button onClick={this.handleDecrease}>-</button>
        </div>
    );
}
```

기존에 자바스크립트에서 사용하던 코드

```html
<button onclick="alert('hello');">Click Me</button>
```

여기서 정말로 **주의**해주셔야 하는데요, 리액트에서 이벤트 함수를 설정할때 html 과 다음과 같은 사항이 다릅니다.

- 이벤트이름을 설정 할 때 **camelCase** 로 설정해주어야 합니다. onclick 은 **onClick**, onmousedown 은 **onMouseDown**, onchange 는 **onChange** 이런식으로 말이죠.
- 이벤트에 전달해주는 값은 **함수** 여야 합니다. 만약에 `onClick={this.handleIncrease()}` 이런식으로 하게 된다면, 렌더링을 할 때 마다 해당 함수가 호출이됩니다. 그렇게 되면 정말 큰 일이 발생합니다. 렌더링 -> 함수 호출 -> setState -> 렌더링 -> 함수 호출 -> 무한반복.. 이렇게 되버리는 것이죠!

그러니까 꼭 주의하셔야 합니다. 렌더링 함수에서 이벤트를 설정 할 때 여러분이 만든 메소드를 호출하지 마세요! 



counter컴퍼넌트 생성

```react
import React, { Component } from 'react';

class counter extends Component {
  state = {
    number: 0
  };

  handleIncrease = () => {
    this.setState({
      number: this.state.number + 1
    });
  };

  handleDecrease = () => {
    this.setState({
      number: this.state.number - 1
    });
  };

  render() {
    return (
      <div>
        <h1>카운터</h1>
        <div>값 : {this.state.number}</div>
        <button onClick={this.handleIncrease}>+</button>
        <button onClick={this.handleDecrease}>-</button>
      </div>
    );
  }
}

export default counter;
```



### 3) Props VS State

|          |                      props                      |                         state                         |
| :------: | :---------------------------------------------: | :---------------------------------------------------: |
|   정의   | 부모 컴포넌트가 <br />자식 컴포넌트에게 주는 값 |                자기 자신이 들고있는값                 |
| 변경유무 |                    읽기전용                     |                       변경가능                        |
| 변경할때 |                        -                        | setState() 사용하여 <br />업데이트 해야 rerendering됨 |



