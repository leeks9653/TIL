# this

this는 기본적으로 window, node에서는 global!  
이유는 런타임 환경이 서로 다르기 떄문

```javascript
function func1(firstName) {
  console.log("firstName = ", firstName, "name = ", this.name);
}
const func2 = (firstName) => {
  console.log("firstName = ", firstName, "name = ", this.name);
};
const obj1 = {
  name: "kyeong_soo",
  func3(firstName) {
    console.log("firstName = ", firstName, "name = ", this.name);
  },
};
const obj2 = {
  name: "kyeong_soo",
  func4: (firstName) => {
    console.log("firstName = ", firstName, "name = ", this.name);
  },
};

func1("lee");
func2("lee");
obj1.func3("lee");
obj2.func4("lee");

// 결과값

#1 firstName =  lee name =  undefined
#2 firstName =  lee name =  undefined
#3 firstName =  lee name =  kyeong_soo
#4 firstName =  lee name =  undefined
```

화살표 함수와 일반 함수의 결과값이 다른 이유?????

<br/>

<br/>

## apply, call , bind

this를 변경하고 싶을 때 사용하는 함수

- apply  
  func.apply(thisArg, [argsArray])

```javascript
const changeOBJ = {
  name: "soo_kyeong",
};

function func1(firstName) {
  console.log("firstName = ", firstName, "name = ", this.name);
}
const func2 = (firstName) => {
  console.log("firstName = ", firstName, "name = ", this.name);
};
const obj1 = {
  name: "kyeong_soo",
  func3(firstName) {
    console.log("firstName = ", firstName, "name = ", this.name);
  },
};
const obj2 = {
  name: "kyeong_soo",
  func4: (firstName) => {
    console.log("firstName = ", firstName, "name = ", this.name);
  },
};

func1.apply(changeOBJ, ["lee"]);
func2.apply(changeOBJ, ["lee"]);
obj1.func3.apply(changeOBJ, ["lee"]);
obj2.func4.apply(changeOBJ, ["lee"]);

// 결과값

#1 firstName =  lee name =  soo_kyeong
#2 firstName =  lee name =  undefined
#3 firstName =  lee name =  soo_kyeong
#4 firstName =  lee name =  undefined
```

- call  
  func.call(thisArg[, arg1[, arg2[, ...]]])

```javascript
const changeOBJ = {
  name: "soo_kyeong",
};

function func1(firstName) {
  console.log("firstName = ", firstName, "name = ", this.name);
}
const func2 = (firstName) => {
  console.log("firstName = ", firstName, "name = ", this.name);
};
const obj1 = {
  name: "kyeong_soo",
  func3(firstName) {
    console.log("firstName = ", firstName, "name = ", this.name);
  },
};
const obj2 = {
  name: "kyeong_soo",
  func4: (firstName) => {
    console.log("firstName = ", firstName, "name = ", this.name);
  },
};

func1.call(changeOBJ, "lee");
func2.call(changeOBJ, "lee");
obj1.func3.call(changeOBJ, "lee");
obj2.func4.call(changeOBJ, "lee");

// 결과값

#1 firstName =  lee name =  soo_kyeong
#2 firstName =  lee name =  undefined
#3 firstName =  lee name =  soo_kyeong
#4 firstName =  lee name =  undefined
```

- bind  
  func.bind(thisArg[, arg1[, arg2[, ...]]])

```javascript
const changeOBJ = {
  name: "soo_kyeong",
};

function func1(firstName) {
  console.log("firstName = ", firstName, "name = ", this.name);
}
const func2 = (firstName) => {
  console.log("firstName = ", firstName, "name = ", this.name);
};
const obj1 = {
  name: "kyeong_soo",
  func3(firstName) {
    console.log("firstName = ", firstName, "name = ", this.name);
  },
};
const obj2 = {
  name: "kyeong_soo",
  func4: (firstName) => {
    console.log("firstName = ", firstName, "name = ", this.name);
  },
};
const newFunc1 = func1.bind(changeOBJ, "lee");
const newFunc2 = func2.bind(changeOBJ, "lee");
const newFunc3 = obj1.func3.bind(changeOBJ, "lee");
const newFunc4 = obj2.func4.bind(changeOBJ, "lee");

newFunc1();
newFunc2();
newFunc3();
newFunc4();

// 결과값

#1 firstName =  lee name =  soo_kyeong
#2 firstName =  lee name =  undefined
#3 firstName =  lee name =  soo_kyeong
#4 firstName =  lee name =  undefined
```

# 정리

apply, call , bind 는 모두 첫번째 인자로 this가 될 값을 인자로 받는다.

apply 는 두번째 인자로 배열을 받는다.
call, bind 는 두번째부터 인자를 쉼표로 구분한다.

bind 는 변경된 this가 적용된 함수를 리턴한다.
