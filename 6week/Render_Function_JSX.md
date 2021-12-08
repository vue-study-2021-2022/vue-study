# Render Function & JSX

Created: December 8, 2021
Created by: Anonymous
Tags: vue

### render 함수

템플릿에 더 가까운 render 함수 사용 가능 

---

→ 반복되는 내용 

```html
<script type="text/x-template" id="anchored-heading-template">
  <h1 v-if="level === 1">
    <slot></slot>
  </h1>
  <h2 v-else-if="level === 2">
    <slot></slot>
  </h2>
  <h3 v-else-if="level === 3">
    <slot></slot>
  </h3>
  <h4 v-else-if="level === 4">
    <slot></slot>
  </h4>
  <h5 v-else-if="level === 5">
    <slot></slot>
  </h5>
  <h6 v-else-if="level === 6">
    <slot></slot>
  </h6>
</script>
```

```jsx
	Vue.component('anchored-heading', {
	  template: '#anchored-heading-template',
	  props: {
	    level: {
	      type: Number,
	      required: true
	    }
	  }
	})
```

→ render 함수로 변환

```jsx
Vue.component('anchored-heading', {
  render: function (createElement) {
    return createElement(
      'h' + this.level,   // 태그 이름
      this.$slots.default // 자식의 배열
    )
  },
  props: {
    level: {
      type: Number,
      required: true
    }
  }
})
```

### 노드, 트리, virtual DOM

---

```jsx
return createElement('h1', this.blogTitle)
```

```jsx
var header = document.createElement('h1');
header.innerText = this.blogTitle
```

→ 결과적으로 둘은 같다. 

### createElement 전달인자

---

createElement 가 받아들이는 전달인자 

```jsx
// @returns {VNode}
createElement(
  // {String | Object | Function}
  // HTML 태그 이름, 컴포넌트 옵션 또는 함수 중
  // 하나를 반환하는 함수입니다. 필수 사항.
  'div',

  // {Object}
  // 템플릿에서 사용할 속성에 해당하는 데이터 객체입니다
  // 데이터 객체입니다. 선택 사항.
  {
    // (아래 다음 섹션에 자세히 설명되어 있습니다.)
  },

  // {String | Array}
  // VNode 자식들. `createElement()`를 사용해 만들거나,
  // 간단히 문자열을 사용해 'text VNodes'를 얻을 수 있습니다. 선택사항
  [
    'Some text comes first.',
    createElement('h1', 'A headline'),
    createElement(MyComponent, {
      props: {
        someProp: 'foobar'
      }
    })
  ]
)

```

→ 데이터 객체 

```jsx
{
  // `v-bind:class` 와 같음
  // accepting either a string, object, or array of strings and objects.
  class: {
    foo: true,
    bar: false
  },
  // `v-bind:style` 와 같음
  // accepting either a string, object, or array of objects.
  style: {
    color: 'red',
    fontSize: '14px'
  },
  // 일반 HTML 속성
  attrs: {
    id: 'foo'
  },
  // 컴포넌트 props
  props: {
    myProp: 'bar'
  },
  // DOM 속성
  domProps: {
    innerHTML: 'baz'
  },
  // `v-on:keyup.enter`와 같은 수식어가 지원되지 않으나
  // 이벤트 핸들러는 `on` 아래에 중첩됩니다.
  // 수동으로 핸들러에서 keyCode를 확인해야 합니다.
  on: {
    click: this.clickHandler
  },
  // 컴포넌트 전용.
  // `vm.$emit`를 사용하여 컴포넌트에서 발생하는 이벤트가 아닌
  // 기본 이벤트를 받을 수 있게 합니다.
  nativeOn: {
    click: this.nativeClickHandler
  },
  // 사용자 지정 디렉티브.
  // Vue는 이를 관리하기 때문에 바인딩의 oldValue는 설정할 수 없습니다.
  directives: [
    {
      name: 'my-custom-directive',
      value: '2',
      expression: '1 + 1',
      arg: 'foo',
      modifiers: {
        bar: true
      }
    }
  ],
  // 범위 지정 슬롯. 형식은
  // { name: props => VNode | Array<VNode> } 입니다.
  scopedSlots: {
    default: props => createElement('span', props.text)
  },
  // 이 컴포넌트가 다른 컴포넌트의 자식인 경우, 슬롯의 이름입니다.
  slot: 'name-of-slot',
  // 기타 최고 레벨 속성
  key: 'myKey',
  ref: 'myRef',
  // If you are applying the same ref name to multiple
  // elements in the render function. This will make `$refs.myRef` become an
  // array
  refInFor: true
}
```

→ 결과 

```jsx
var getChildrenTextContent = function (children) {
  return children.map(function (node) {
    return node.children
      ? getChildrenTextContent(node.children)
      : node.text
  }).join('')
}

Vue.component('anchored-heading', {
  render: function (createElement) {
    // kebabCase id를 만듭니다.
    var headingId = getChildrenTextContent(this.$slots.default)
      .toLowerCase()
      .replace(/\W+/g, '-')
      .replace(/(^-|-$)/g, '')

    return createElement(
      'h' + this.level,
      [
        createElement('a', {
          attrs: {
            name: headingId,
            href: '#' + headingId
          }
        }, this.$slots.default)
      ]
    )
  },
  props: {
    level: {
      type: Number,
      required: true
    }
  }
})
```

### 제약사항

VNodes 는 고유해야 한다. 

---

→ 유효하지 않은 랜더링 함수 

```jsx
render: function (createElement) {
  var myParagraphVNode = createElement('p', 'hi')
  return createElement('div', [
    // 이런 - Vnode가 중복입니다!
    myParagraphVNode, myParagraphVNode
  ])
}
```

→ 20개의 p 태그 렌더링 

```jsx
render: function (createElement) {
  return createElement('div',
    Array.apply(null, { length: 20 }).map(function () {
      return createElement('p', 'hi')
    })
  )
}
```

### 템플릿 기능을 일반 Js 로 변경

---

→ v-if, v-for

```html
<ul v-if="items.length">
  <li v-for="item in items">{{ item.name }}</li>
</ul>
<p v-else>No items found.</p>
```

→ if/else,  map

```jsx
props: ['items'],
render: function (createElement) {
  if (this.items.length) {
    return createElement('ul', this.items.map(function (item) {
      return createElement('li', item.name)
    }))
  } else {
    return createElement('p', 'No items found.')
  }
}
```

랜더함수에는 직접적으로 v-model 에 대응되는 것이 없음. 직접 구현 필요 

→ v-model 구현

```jsx
props: ['value'],
render: function (createElement) {
  var self = this
  return createElement('input', {
    domProps: {
      value: self.value
    },
    on: {
      input: function (event) {
        self.$emit('input', event.target.value)
      }
    }
  })
}
```

→ Slots 의 this.$slots 에서 정적 슬롯 내용을 VNodes 의 배열로 접근 가능 

```jsx
render : function(createElement){
	// `<div><slot></slot></div>`
	return createElement('div', this.$slots.default)
}
```

→ this.$scopedSlots

```jsx
props: ['message'],
render: function (createElement) {
  // `<div><slot :text="message"></slot></div>`
  return createElement('div', [
    this.$scopedSlots.default({
      text: this.message
    })
  ])
}
```

→ slots() vs children 

```html
<my-functional-component>
  <p v-slot:foo>
    first
  </p>
  <p>second</p>
</my-functional-component>
```

children 은 두 개의 p 태그 반환 

slots().default 는 두 번째 p 태그 반환 

---

javascript 의 childeren(), parentNodes(), childNodes(), document.createElement('태그이름')

등의 vue에서의 사용법을 설명해 놓은 것. 

위의 함수를 미리 알고 있으면 좋다.