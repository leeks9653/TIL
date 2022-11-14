# 화살표 함수

- es6에 새로 추가된 문법.
- this나 super에 대한 바인딩이 없고, methods 로 사용될 수 없습니다.
- new.target키워드가 없습니다.(함수 또는 생성자가 new 연산자를 사용하여 호출됐는지를 감지할 수 있습니다)
- 일반적으로 스코프를 지정할 때 사용하는 call, apply, bind methods를 이용할 수 없습니다.
- 생성자(Constructor)로 사용할 수 없습니다.
- yield를 화살표 함수 내부에서 사용할 수 없습니다.
- 렉시컬 스코프 안의 다른 this를 가르킨다.  
  [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/Arrow_functions](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

## this 바인딩 안됨

```javascript
const obj1 = {
  name: "kyeong_soo",
  call: () => {
    console.log(this.name);
  },
};

const ojb2 = {
  name: "soo_kyeong",
  call() {
    console.log(this.name);
  },
};

obj1.call();
ojb2.call();

// 결과값
#1 undefined
#2 soo_kyeong
```

## 생성자로 사용 안됨

```javascript
function test1(firstname, lastname) {
  this.firstname = firstname;
  this.lastname = lastname;
}

const test2 = (firstname, lastname) => {
  this.firstname = firstname;
  this.lastname = lastname;
};

const test11 = new test1("lee", "kyeong_soo");
const test22 = new test2("lee", "kyeong_soo");

// 결과값

#1 ""
#2
const test22 = new test2("lee", "kyeong_soo");
               ^

TypeError: test2 is not a constructor
```
