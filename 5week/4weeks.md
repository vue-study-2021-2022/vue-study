# 4주차

📜  부모 컴포넌트는 **Props**를 통해 자식 컴포넌트에게 data를 전달하고,

자식 컴포넌트는 **events**를 통해 부모 컴포넌트에게 메세지를 보냄

# 1. Props

- 부모 컴포넌트의 data를 자식 컴포넌트에게 전달할때 사용 되는 단방향 데이터 전달 방식
- 부모 컴포넌트의 data가 변경되면 자식 컴포넌트에게 전달되지만, 반대 반향으로는 전달되지 않는다.
- data는 **Props**옵션을 사용하여 하위 컴포넌트에 전달 될 수 있다.
- **Props**로 지정 된 값을 자식 컴포넌트에서 직접 변경할 경우 에러 발생
- **Props** 이름을 calmelCase로 정한 경우, template안에서는 kebab-case로 사용
    
    ❗HTML 속성을 대소문자를 구분하지 않기 때문
    

```jsx
<!-- 부모 컴포넌트 -->
<child-components :message-text="messageText" />
export default {
	data(){
		return {
			messageText: 'Yi ji Hyeon'
		}
	}
}

<!-- 자식 컴포넌트 -->
<template>
	<div>{{ messageText }}</div> // Yi ji Hyeon
</template>
export default {
	props:['messageText']
}
```

## 유효성 검사

- props는 타입과 기본값, 필수값을 지정할 수 있음
    - type : data의 타입을 정할 수 있다.
        - String
        - Number
        - Boolean
        - Array
        - Object
        - Date
        - Function
        - Symbol
    - 기본값(default) : 부모에서 값을 전달해주지 않았을 경우의 기본값 설정
    - 필수값(required) : 값이 true일 때 해당 **Props**를 부모에서 전달해주지 않을경우 에러 발생
    

```jsx
Vue.component('my-component', {
  props: {
    // 기본 타입 체크 (`Null`이나 `undefinded`는 모든 타입을 허용합니다.)
    propA: Number,

    // 여러 타입 허용
    propB: [String, Number],

    // 필수 문자열
    propC: {
      type: String,
      required: true
    },

    // 기본값이 있는 숫자
    propD: {
      type: Number,
      default: 100
    },

    // 기본값이 있는 오브젝트
    propE: {
      type: Object,
      // 오브젝트나 배열은 꼭 기본값을 반환하는
      // 팩토리 함수의 형태로 사용되어야 합니다. 
      default: function () {
        return { message: 'hello' }
      }
    },
    // 커스텀 유효성 검사 함수
    propF: {
      validator: function (value) {
        // 값이 항상 아래 세 개의 문자열 중 하나여야 합니다. 
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
})
```

- 비부모-자식간의 통신은 이벤트 버스를 사용
    
    ❗자주 사용할 결우 이벤트 추적 관리가 힘들어짐
    
- 여러 컴포넌트에 분산되어있는 상태와, 그 상호작용으로 인해 관리가 복잡해질 경우에는 Vuex(상태관리패턴)를 사용

# 2. 커스텀 이벤트 - $emit

- **Props**로 받은 data를 자식 컴포넌트에서 직접적으로 변경할 수 없음
- 따라서 자식 컴포넌트에서 변경된 값을 부모 컴포넌트로 업데이트 및 전달해야 함

```jsx
// 자식 컴포넌트에서 이벤트를 선언
this.$emit('eventName');
 
// 부모 컴포넌트
<childComponents @eventName='testEvent'></childComponents>

methods : {
    testEvent(){
     colsole.log('vue js')
    }
}
```

# 3. 슬롯(Sots)

- 컴포넌트의 재사용성을 높여주는 기능
- 특정 컴포넌트에 등록된 하위 컴포넌트의 마크업을 확장하거나 재정의

### Named Slot

- slot은 name 속성을 지정하여 여러개 사용 가능
- <template> 엘리먼트 사용 가능

```jsx
<!-- ButtonTab.vue -->
<template>
  <div class="tab panel">
    <!-- 탭 헤더 -->
    <slot name="header"></slot>
    <!-- 탭 본문 -->
    <slot name="content"></slot>
  </div>
</template>

<!-- TabContainer.vue -->
<template>
  <button-tab>
    <!-- slot 영역 -->
    <h1 slot="header">First Header</h1>
    <div slot="content" class="content">Tab Contents #1</div>
  </button-tab>
</template>
```

### Scoped Slot

- 자식 컴포넌트에서만 접근 할 수 있는 데이터에서 slot에 필요한 경우 사용

### v-slot

- 간단하게 named-slot + slot-scope
- slot 디렉티브와는 다르게 반드시 <template>태그에 추가
- v-slot:name은 #name으로 축약어를 사용할수 있음
    
    ```jsx
    <template v-slot:header>
    	<h1>Here might be a page title</h1>
    </template>
    
    <template v-slot:content>
    	<p>Here's some contact info</p>
    </template>
    ```
    
    ❗`slot`, `slot-scope`는 이후 업데이트 될 Vue 3에서는 공식적으로 삭제된다고 하니 Vue에서 공식적으로 지원 할 `v-slot`만 사용
    

---