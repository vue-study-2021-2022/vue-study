# 4์ฃผ์ฐจ

๐  ๋ถ๋ชจ ์ปดํฌ๋ํธ๋ **Props**๋ฅผ ํตํด ์์ ์ปดํฌ๋ํธ์๊ฒ data๋ฅผ ์ ๋ฌํ๊ณ ,

์์ ์ปดํฌ๋ํธ๋ **events**๋ฅผ ํตํด ๋ถ๋ชจ ์ปดํฌ๋ํธ์๊ฒ ๋ฉ์ธ์ง๋ฅผ ๋ณด๋

# 1. Props

- ๋ถ๋ชจ ์ปดํฌ๋ํธ์ data๋ฅผ ์์ ์ปดํฌ๋ํธ์๊ฒ ์ ๋ฌํ ๋ ์ฌ์ฉ ๋๋ ๋จ๋ฐฉํฅ ๋ฐ์ดํฐ ์ ๋ฌ ๋ฐฉ์
- ๋ถ๋ชจ ์ปดํฌ๋ํธ์ data๊ฐ ๋ณ๊ฒฝ๋๋ฉด ์์ ์ปดํฌ๋ํธ์๊ฒ ์ ๋ฌ๋์ง๋ง, ๋ฐ๋ ๋ฐํฅ์ผ๋ก๋ ์ ๋ฌ๋์ง ์๋๋ค.
- data๋ **Props**์ต์์ ์ฌ์ฉํ์ฌ ํ์ ์ปดํฌ๋ํธ์ ์ ๋ฌ ๋  ์ ์๋ค.
- **Props**๋ก ์ง์  ๋ ๊ฐ์ ์์ ์ปดํฌ๋ํธ์์ ์ง์  ๋ณ๊ฒฝํ  ๊ฒฝ์ฐ ์๋ฌ ๋ฐ์
- **Props** ์ด๋ฆ์ calmelCase๋ก ์ ํ ๊ฒฝ์ฐ, template์์์๋ kebab-case๋ก ์ฌ์ฉ
    
    โHTML ์์ฑ์ ๋์๋ฌธ์๋ฅผ ๊ตฌ๋ถํ์ง ์๊ธฐ ๋๋ฌธ
    

```jsx
<!-- ๋ถ๋ชจ ์ปดํฌ๋ํธ -->
<child-components :message-text="messageText" />
export default {
	data(){
		return {
			messageText: 'Yi ji Hyeon'
		}
	}
}

<!-- ์์ ์ปดํฌ๋ํธ -->
<template>
	<div>{{ messageText }}</div> // Yi ji Hyeon
</template>
export default {
	props:['messageText']
}
```

## ์ ํจ์ฑ ๊ฒ์ฌ

- props๋ ํ์๊ณผ ๊ธฐ๋ณธ๊ฐ, ํ์๊ฐ์ ์ง์ ํ  ์ ์์
    - type : data์ ํ์์ ์ ํ  ์ ์๋ค.
        - String
        - Number
        - Boolean
        - Array
        - Object
        - Date
        - Function
        - Symbol
    - ๊ธฐ๋ณธ๊ฐ(default) : ๋ถ๋ชจ์์ ๊ฐ์ ์ ๋ฌํด์ฃผ์ง ์์์ ๊ฒฝ์ฐ์ ๊ธฐ๋ณธ๊ฐ ์ค์ 
    - ํ์๊ฐ(required) : ๊ฐ์ด true์ผ ๋ ํด๋น **Props**๋ฅผ ๋ถ๋ชจ์์ ์ ๋ฌํด์ฃผ์ง ์์๊ฒฝ์ฐ ์๋ฌ ๋ฐ์
    

```jsx
Vue.component('my-component', {
  props: {
    // ๊ธฐ๋ณธ ํ์ ์ฒดํฌ (`Null`์ด๋ `undefinded`๋ ๋ชจ๋  ํ์์ ํ์ฉํฉ๋๋ค.)
    propA: Number,

    // ์ฌ๋ฌ ํ์ ํ์ฉ
    propB: [String, Number],

    // ํ์ ๋ฌธ์์ด
    propC: {
      type: String,
      required: true
    },

    // ๊ธฐ๋ณธ๊ฐ์ด ์๋ ์ซ์
    propD: {
      type: Number,
      default: 100
    },

    // ๊ธฐ๋ณธ๊ฐ์ด ์๋ ์ค๋ธ์ ํธ
    propE: {
      type: Object,
      // ์ค๋ธ์ ํธ๋ ๋ฐฐ์ด์ ๊ผญ ๊ธฐ๋ณธ๊ฐ์ ๋ฐํํ๋
      // ํฉํ ๋ฆฌ ํจ์์ ํํ๋ก ์ฌ์ฉ๋์ด์ผ ํฉ๋๋ค. 
      default: function () {
        return { message: 'hello' }
      }
    },
    // ์ปค์คํ ์ ํจ์ฑ ๊ฒ์ฌ ํจ์
    propF: {
      validator: function (value) {
        // ๊ฐ์ด ํญ์ ์๋ ์ธ ๊ฐ์ ๋ฌธ์์ด ์ค ํ๋์ฌ์ผ ํฉ๋๋ค. 
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
})
```

- ๋น๋ถ๋ชจ-์์๊ฐ์ ํต์ ์ ์ด๋ฒคํธ ๋ฒ์ค๋ฅผ ์ฌ์ฉ
    
    โ์์ฃผ ์ฌ์ฉํ  ๊ฒฐ์ฐ ์ด๋ฒคํธ ์ถ์  ๊ด๋ฆฌ๊ฐ ํ๋ค์ด์ง
    
- ์ฌ๋ฌ ์ปดํฌ๋ํธ์ ๋ถ์ฐ๋์ด์๋ ์ํ์, ๊ทธ ์ํธ์์ฉ์ผ๋ก ์ธํด ๊ด๋ฆฌ๊ฐ ๋ณต์กํด์ง ๊ฒฝ์ฐ์๋ Vuex(์ํ๊ด๋ฆฌํจํด)๋ฅผ ์ฌ์ฉ

# 2. ์ปค์คํ ์ด๋ฒคํธ - $emit

- **Props**๋ก ๋ฐ์ data๋ฅผ ์์ ์ปดํฌ๋ํธ์์ ์ง์ ์ ์ผ๋ก ๋ณ๊ฒฝํ  ์ ์์
- ๋ฐ๋ผ์ ์์ ์ปดํฌ๋ํธ์์ ๋ณ๊ฒฝ๋ ๊ฐ์ ๋ถ๋ชจ ์ปดํฌ๋ํธ๋ก ์๋ฐ์ดํธ ๋ฐ ์ ๋ฌํด์ผ ํจ

```jsx
// ์์ ์ปดํฌ๋ํธ์์ ์ด๋ฒคํธ๋ฅผ ์ ์ธ
this.$emit('eventName');
 
// ๋ถ๋ชจ ์ปดํฌ๋ํธ
<childComponents @eventName='testEvent'></childComponents>

methods : {
    testEvent(){
     colsole.log('vue js')
    }
}
```

# 3. ์ฌ๋กฏ(Sots)

- ์ปดํฌ๋ํธ์ ์ฌ์ฌ์ฉ์ฑ์ ๋์ฌ์ฃผ๋ ๊ธฐ๋ฅ
- ํน์  ์ปดํฌ๋ํธ์ ๋ฑ๋ก๋ ํ์ ์ปดํฌ๋ํธ์ ๋งํฌ์์ ํ์ฅํ๊ฑฐ๋ ์ฌ์ ์

### Named Slot

- slot์ name ์์ฑ์ ์ง์ ํ์ฌ ์ฌ๋ฌ๊ฐ ์ฌ์ฉ ๊ฐ๋ฅ
- <template> ์๋ฆฌ๋จผํธ ์ฌ์ฉ ๊ฐ๋ฅ

```jsx
<!-- ButtonTab.vue -->
<template>
  <div class="tab panel">
    <!-- ํญ ํค๋ -->
    <slot name="header"></slot>
    <!-- ํญ ๋ณธ๋ฌธ -->
    <slot name="content"></slot>
  </div>
</template>

<!-- TabContainer.vue -->
<template>
  <button-tab>
    <!-- slot ์์ญ -->
    <h1 slot="header">First Header</h1>
    <div slot="content" class="content">Tab Contents #1</div>
  </button-tab>
</template>
```

### Scoped Slot

- ์์ ์ปดํฌ๋ํธ์์๋ง ์ ๊ทผ ํ  ์ ์๋ ๋ฐ์ดํฐ์์ slot์ ํ์ํ ๊ฒฝ์ฐ ์ฌ์ฉ

### v-slot

- ๊ฐ๋จํ๊ฒ named-slot + slot-scope
- slot ๋๋ ํฐ๋ธ์๋ ๋ค๋ฅด๊ฒ ๋ฐ๋์ <template>ํ๊ทธ์ ์ถ๊ฐ
- v-slot:name์ #name์ผ๋ก ์ถ์ฝ์ด๋ฅผ ์ฌ์ฉํ ์ ์์
    
    ```jsx
    <template v-slot:header>
    	<h1>Here might be a page title</h1>
    </template>
    
    <template v-slot:content>
    	<p>Here's some contact info</p>
    </template>
    ```
    
    โ`slot`,ย `slot-scope`๋ ์ดํ ์๋ฐ์ดํธ ๋  Vue 3์์๋ ๊ณต์์ ์ผ๋ก ์ญ์ ๋๋ค๊ณ  ํ๋ Vue์์ ๊ณต์์ ์ผ๋ก ์ง์ ํ ย `v-slot`๋ง ์ฌ์ฉ
    

---