 # props와 state

### 1) props

> props 는 부모 컴포넌트가 자식 컴포넌트에게 주는 값입니다. 
>
> 자식 컴포넌트에서는 props 를 받아오기만하고, 받아온 props 를 직접 수정 할 수 는 없습니다.

<img src="https://user-images.githubusercontent.com/36122040/60410872-4bae6280-9c05-11e9-9453-e0c1d9cbef46.jpg" alt="">

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



# LifeCycle API

> 컴포넌트가 브라우저에서 나타날때, 사라질때, 그리고 업데이트 될 때, 호출되는 API 



## 컴포넌트 초기 생성 (Mounting)

>  컴포넌트가 브라우저에 나타날때 호출되는 API

### constructor

> 컴포넌트 생성자 함수
>
> 컴포넌트가 새로 만들어질 때마다 이 함수가 호출
>
> 컴포넌트가 만들어 지는 과정에서 미리 설정해야 하는 작업이 있을때

```react
constructor(props) {
  super(props);
}
```



### ~~componentWillMount~~

> 이 API 는 컴포넌트가 여러분의 화면에 나가나기 직전에 호출되는 API 인데요, 이 API 에 대해선 별로 신경쓰지 않아도 됩니다. 

```react
componentWillMount() {

}
```



### componentDidMount

> 컴포넌트가 화면에 나타나게 됐을 때 호출
>
> 여기선 주로 D3, masonry 처럼 DOM 을 사용해야하는 외부 라이브러리 연동을 하거나, 해당 컴포넌트에서 필요로하는 데이터를 요청하기 위해 axios, fetch 등을 통하여 ajax 요청을 하거나, DOM 의 속성을 읽거나 직접 변경하는 작업을 진행합니다. 

```react
componentDidMount() {
  // 외부 라이브러리 연동: D3, masonry, etc
  // 컴포넌트에서 필요한 데이터 요청: Ajax, GraphQL, etc
  // DOM 에 관련된 작업: 스크롤 설정, 크기 읽어오기 등
}
```



## 컴포넌트 업데이트 (Updating)

> 컴포넌트가 업데이트는 props 의 변화, 그리고 state 의 변화에 따라 결정됩니다. 
>
> 업데이트가 되기 전과 그리고 된 후에 호출되는 API



### static getDerivedStateFromProps()

> 이 함수는, v16.3 이후에 만들어진 라이프사이클 API 인데요, 이 API 는 props 로 받아온 값을 state 로 동기화 하는 작업을 해줘야 하는 경우에 사용됩니다. 

```react
static getDerivedStateFromProps(nextProps, prevState) {
  // 여기서는 setState 를 하는 것이 아니라
  // 특정 props 가 바뀔 때 설정하고 설정하고 싶은 state 값을 리턴하는 형태로
  // 사용됩니다.
  /*
  if (nextProps.value !== prevState.value) {
    return { value: nextProps.value };
  }
  return null; // null 을 리턴하면 따로 업데이트 할 것은 없다라는 의미
  */
}
```



### shouldComponentUpdate ** 중요

> 이 API 는 컴포넌트를 최적화하는 작업에서 매우 유용하게 사용됩니다. 우리가 저번에 배웠을 떄, 리액트에서는 변화가 발생하는 부분만 업데이트를 해줘서 성능이 꽤 잘 나온다고 했었지요? 하지만, 변화가 발생한 부분만 감지해내기 위해서는 Virtual DOM 에 한번 그려줘야합니다.
>
> 즉, 현재 컴포넌트의 상태가 업데이트되지 않아도, 부모 컴포넌트가 리렌더링되면, 자식 컴포넌트들도 렌더링 됩니다. 여기서 "렌더링" 된다는건, render() 함수가 호출된다는 의미입니다.
>
> 변화가 없으면 물론 DOM 조작은 하지 않게 됩니다. 그저 Virutal DOM 에만 렌더링 할 뿐이죠. 이 작업은 그렇게 부하가 많은 작업은 아니지만, 컴포넌트가 무수히 많이 렌더링된다면 얘기가 조금 달라집니다. CPU 자원을 어느정도 사용하고 있는것은 사실이니까요.
>
> 쓸대없이 낭비되고 있는 이 CPU 처리량을 줄여주기 위해서 우리는 **Virtual DOM 에 리렌더링 하는것도,불필요할경우엔 방지하기 위해서** shouldComponentUpdate 를 작성합니다.
>
> 이 함수는 기본적으로 true 를 반환합니다. 우리가 따로 작성을 해주어서 조건에 따라 false 를 반환하면 해당 조건에는 render 함수를 호출하지 않습니다.

```react
shouldComponentUpdate(nextProps, nextState) {
  // return false 하면 업데이트를 안함
  // return this.props.checked !== nextProps.checked
  return true;
}
```



### render

> 어떤 돔을 만들지 내부에 태그에 어떤 값을 전달할지

```react
 render() {
    return (
      <div>
        <h1>카운터</h1>
      </div>
    );
  }
```



### getSnapshotBeforeUpdate()

>  이 API 가 발생하는 시점은 다음과 같습니다.
>
> 1. render()
> 2. **getSnapshotBeforeUpdate()**
> 3. 실제 DOM 에 변화 발생
> 4. componentDidUpdate
>
> 이 API를 통해서, DOM 변화가 일어나기 직전의 DOM 상태를 가져오고, 여기서 리턴하는 값은 componentDidUpdate 에서 3번째 파라미터로 받아올 수 있게 됩니다.
>
> 브라우저상에 보이기전 스크롤 위치나 사이즈를 가져오고싶을때

```react
 getSnapshotBeforeUpdate(prevProps, prevState) {
    // DOM 업데이트가 일어나기 직전의 시점입니다.
    // 새 데이터가 상단에 추가되어도 스크롤바를 유지해보겠습니다.
    // scrollHeight 는 전 후를 비교해서 스크롤 위치를 설정하기 위함이고,
    // scrollTop 은, 이 기능이 크롬에 이미 구현이 되어있는데, 
    // 이미 구현이 되어있다면 처리하지 않도록 하기 위함입니다.
    if (prevState.array !== this.state.array) {
      const {
        scrollTop, scrollHeight
      } = this.list;

      // 여기서 반환 하는 값은 componentDidMount 에서 snapshot 값으로 받아올 수 있습니다.
      return {
        scrollTop, scrollHeight
      };
    }
  }

  componentDidUpdate(prevProps, prevState, snapshot) {
    if (snapshot) {
      const { scrollTop } = this.list;
      if (scrollTop !== snapshot.scrollTop) return; // 기능이 이미 구현되어있다면 처리하지 않습니다.
      const diff = this.list.scrollHeight - snapshot.scrollHeight;
      this.list.scrollTop += diff;
    }
  }
```

전체 코드 [<https://codesandbox.io/s/484zvr87ow> ](https://codesandbox.io/s/484zvr87ow )



### componentDidUpdate

> 이 API는 컴포넌트에서 render() 를 호출하고난 다음에 발생하게 됩니다. 이 시점에선 this.props 와 this.state 가 바뀌어있습니다. 그리고 파라미터를 통해 이전의 값인 prevProps 와 prevState 를 조회 할 수 있습니다. 그리고, getSnapshotBeforeUpdate 에서 반환한 snapshot 값은 세번째 값으로 받아옵니다.

```react
componentDidUpdate(prevProps, prevState, snapshot) {

}
```



## 컴포넌트 제거 (Unmounting)

> 컴포넌트가 더 이상 필요하지 않게 되면 단 하나의 API 가 호출



### componentWillUnmount

> 여기서는 주로 등록했었던 이벤트를 제거하고, 만약에 setTimeout 을 걸은것이 있다면 clearTimeout 을 통하여 제거를 합니다. 추가적으로, 외부 라이브러리를 사용한게 있고 해당 라이브러리에 dispose 기능이 있다면 여기서 호출해주시면 됩니다. 

```react
componentWillUnmount() {
  // 이벤트, setTimeout, 외부 라이브러리 인스턴스 제거
}
```

