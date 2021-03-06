

### Mixins

- 이름처럼 컴포넌트에 무언가를 섞어 원하는 것을 기능을 구현
- 특정 기능을 나타내는 파일을 캡슐화 할 수 있음
- 여러 컴포넌트에서 동일한 기능이 있을 경우 mixins 사용
- 컴포넌트와 동일한 라이프사이클을 가지며 모든 훅을 사용할 수 있음
- 믹스인 파일은 순수 자바스크립트로 작성된 객체 (html/css 믹스인 하지 않음)
- 

<img src="../image/sample/20211208_6weeks_yijiheion_img_03.png"> 

⭐ **장점**

- 단순히 코드를 줄이는 것 뿐만 아니라 유지보수가 용이
- 코드의 재사용성

❗**단점**

- 해당 data나 methods가 어느 믹스인에 정의되어 있는지 파악하기 어려움

---

### 옵션 병합

- 컴포넌트와 믹스인은 상하관계를 가지게 되어 컴포넌트에서 mixin의 data prop을 사용할 수 있지만 
mixin에서 컴포넌트의 data prop 사용은 불가
- 믹스인에서 선언한 속성을 컴포넌트에서도 다시 선언할 수 있음
    - 해당 경우에는 컴포넌트에 선언된 값을 우선하여 병합

```jsx
// 즉, 아래와 같이 mixin.js와 component.vue에 같은 속성이 중복 선언된 경우에
// mixin.js
...
data {
  return {
    age: 20
  }
}
...

// component.vue
  ...
data {
  return {
    age: 26
  }
}
...

// 컴포넌트의 선언값이 살아남습니다.
// result
  <div>{{ age }}</div>
// => <div>26</div>
```

- 같은 이름의 함수가 병합 될 경우 모든 함수가 호출
    - mixin 훅은 컴포넌트 자체의 훅 이전에 호출

```jsx
var mixin = {
  created: function () {
    console.log('mixin hook called')
  }
}

new Vue({
  mixins: [mixin],
  created: function () {
    console.log('component hook called')
  }
})

// => "mixin hook called"
// => "component hook called"
```

---

### 전역 Mixin

- 전역으로도 사용할 수 있음
    
    ❗다만, 믹스인은 전역으로 사용하면 모든 vue 인스턴스에 영항을 미치므로 해당 방법은 지양
    

```jsx
Vue.mixin({
  mounted() {
    console.log('hello from mixin!')
  }
})

new Vue({
  ...
})
```

<img src="../image/sample/20211208_6weeks_yijiheion_img_02.png"> 

---

### 사용자 정의 옵션 병합 전략

- 사용자 지정 옵션을 병합할 때 기본 옵션을 사용하면 기존 값을 덮어씀
- 커스텀 로직을 사용해 커스텀 오셥을 병함하려면 Vue.config.optionMergeStrategies에 함수 추가
    - optionMergeStrategies 객체
    
<img src="../image/sample/20211208_6weeks_yijiheion_img_01.png"> 
