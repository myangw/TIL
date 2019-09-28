## 실전리액트프로그래밍
## 2장. ES6+ 자바스크립트

### 2-1. var → const, let

- **var의 문제점**
- var : 함수 스코프를 가진다. 함수를  벗어난 영역에서 사용하면 에러
- var는 hoisting된다 : 이 변수가 속한 스코프의 최상단으로 끌어올려진다. (다만 assign은 assign하는 코드 시점에 되고, define만 위로 올려짐)
- **const, let : 블록스코프.**
    - const, let도 hoisting되기는 하다. 그러나 정의된 위치보다 앞서서 변수를 사용하려고 하면 temporal dead zone에 있어서 에러가 발생함.(var는 undefined) - 그렇담 const, let을 호이스팅해서 다른게 무엇인가?
```javascript
        const foo = 1;
        {
        	console.log(foo); // 호이스팅이 없다면 여기서 값이1인 foo를 참조하게될 것이다.
        	// 그러나 hoisting.. 2인 foo를 참조하기 때문에 참조에러가 발생한다
        	const foo = 2;
        }
```
- **const**
    - 한번 할당해놓으면 새로운 값으로 재할당 안됨
    - const로 객체를 정의하면(Object, array)  그거의 내부 속성값은 수정이 가능하다
        - 이것도 재할당 못하게 만들려면 immutable.js, immer쓰기 (객체 수정시 기존 객체는 변경하지 않고 새로운 객체를 생성하게 된다)
        - 새로운 객체생성x, 수정못하게 하려면
            - Object.preventExtensions
            - Object.seal
            - Object.freeze

### 2-2. 객체와 배열의 사용성 개선

- **Shorthand property names (단축속성명)**
    - 객체 속성값 일부가 이미 변수로 존재한다면, 간단하게 변수 이름만 적어주면, 알아서 속성명이 변수 이름과 같게 들어간다.
``` javascript
    const name = 'mia';
    const obj = {
    	age: 21,
    	name,
    	getName() { return this.name; },
    };
    
    function makePerson(age, name) {
    	return { age: age, ....}
    }  // 대신에
    
    function makePerson(age, name) {
    	return { age, name }
    }  // 이걸로 끝
```
- **Computed property names**

``` javascript
    function makeObjectBefore(key, value) {
    	const obj = {};
    	obj[key] = value;
    	return obj;
    }
    function makeObjectWithES6(key, value) {
    	return { [key]: value };
    }
    // 리액트에서 적용
    class MyComponent extends React.Component {
    	state = {
    		count1: 0,
    		count2: 0,
    		count3: 0,
        };
        onClick = index => {
            const key = `count${index}`;
            const value = this.state[key];
            this.setState({[key] : value + 1});
        };
    }
```

  - **전개 연산자**
    - 배열 또는 객체의 모든 속성을 풀어놓을 때
    - 전개연산자로 쉽게 서로 다른 두 배열, 객체를 합칠 수 있음. 순서 유지!
      - 중복된 속성명이 있을 경우 마지막꺼를 적용함
    - 배열, 객체 **복사**할때도 유용.
  ``` javascript
    const panda1 = {
        age: 3,
        name: 'Jake'
    }
    const panda2 = {...panda1};
    panda2.age = 'Jay';   // this code doesn't affect panda1
  ```

 - Array destructuring(배열 비구조화)
``` javascript


```
