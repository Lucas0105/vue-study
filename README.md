# vue 기초
- [vue 설치방법](#vue-설치방법)
- [vue 속성](#vue-속성)
- [Basic of Syntax](#basic-of-syntax)

# vue CLI
- [설치 방법](#설치-방법)
- [컴포넌트 사용하는 방법](#컴포넌트-사용하는-방법)
- [값을 전달하는 방법 (pass props) 부모 -> 자식](#값을-전달하는-방법)
- [자식이 부모에게 값의 변경을 요청 하는 방법(Emit)](#부모값의-변경-요청)

vue 공식 문서(https://v2.vuejs.org/)

## vue 설치방법
- vscode Vetur exension 설치
- Chrome에서 Vue.js devtools extension 설치 - 확장 프로그램 관리에서 파일 URL에 대한 엑세스 허용

## vue 속성
### el
- Vue instance와 DOM을 mount(연결)하는 방법(id 혹은 class와 마운트 가능)

```
# view
<div id='app'>

</div>

<script>
  // view Model
  const app = new Vue({
    el: '#app'    // app id 와 연결
  })
</script>
```

### data
- 데이터 객체
```
const app = new Vue({
el: '#app',
data: {
  message: 'Hello, Vue!'
}

})
```


### methods
- vue instance의 method들을 정의하는 곳
```
const app = new Vue({
  el: '#app',
  data: {
    message: 'Hello'
  },
  methods: {
    print: function() {
      console.log(this.message)
    },
    
    bye () {
      console.log(this.message)
    }
  }
})

app.print()
```
- 주의 : 메서드를 정의할 때 Arrow Function을 사용하면 안됨 => this가 window를 가리키기 때문
- 정의하고 난 후 메서드 안에서는 Arrow Function을 사용해도 됨

### computed
- 최초로 렌더링 될 때 한번만 호출하여 계산 => 계산된 값을 사용하기 때문에 함수가 아닌 변수형식으로 사용함
- 하지만 종속된 대상이 변할 때 계산을 함
- method는 호출 될 때마다 함수를 실행함

```
const app = new Vue({
  computed: {
    print: function() {
      console.log(this.message)
    }
  }
})
```

### watch
- 특정 데이터의 변화를 감지하는 기능
```
const app = new Vue({
  data: {
    number: 0,
    name: '',
    myObj: {completed: true}
  },
  method: {
    method 함수: function() {
      
    }
  },
  watch: {
    number: function(val, oldVal){
      console.log("number이 변경되었습니다.")
      console.log("현재 값은 val")
      console.log("변동 전 값은 oldVal")
    },
    name:{
      handler: "method 함수",
    },
    myObj:{
      handler: "method 함수1",
      deep: true,           # 변수 내부에 있는 값이 변경되도 감시할 수 있도록 설정
    }
  }
})
```

### filters
- | 키워드를 통해 필터를 사용할 수 있음
- 이어서 사용하는 Chainig 가능
```
<p> {{ number | getOddNums }} </p>


<script>
  const app = new Vue({
    data: {
    numbers: [1, 2, 3, 4, 5],
    },
    filters:{
      getOddNums: function (nums) {
        oddNum = nums.filter((num) => {
          return num % 2
        })
        
        return oddNum
      }
    }
  })
</script>
```

## Basic of Syntax
### Template Syntax
- Template에서 쓰는 문법
```
{{ message }}
```


### Directives
- v-접두사를 통해 속성 값을 할당 할 수 있음
```
v-on:submit.prevent="onSubmit"
```

#### v-text
```
<p v-text="변수"></p>
<!-- {{ }} 와 동일한 역할 -->
```

- v-html(XSS 공격이 가능하기 때문에 사용자가 입력하거나 제공하는 컨텐츠에는 절대 사용금지)
```
<p v-html="html"></p>
```

#### v-show
```
<p v-show="변수(false)">잘 보이나요?</p> <!-- 안보이지만 개발자도구에서는 볼 수 있음 -->
<p v-show="변수(true)">잘 보이나요?</p> <!-- 잘 보임 -->
```

#### v-if
```
<p v-if="변수(false)">잘 보이나요?</p> <!-- 안보이고 개발자도구에서 찾아 볼 수 없음 -->
<p v-if="변수(true)">잘 보이나요?</p> <!-- 잘 보임 -->
``` 
- v-else-if, v-else 사용 가능

- v-show 와 v-if 의 차이점
```
v-show : 초기 렌더링 비용 높음, 토글 비용 낮음
v-if : 초기 렌더링 비용 낮음, 토글 비용 높음 (더 자주 쓰임)
```

#### v-for
```
<div v-for="(item, index) in myArr":key="index">
  <p>{{ index }}번째 아이템</p>
  <p>{{ item.name }}</p>
  
  # 객체 show
  <div v-for="(value, key) in item":key="key">
    {{ key }} - {{ value }}
  </div>
</div>

<script>
const app = new Vue({
  data:{
    myArr: [
      {id: 1, name: 'python'},
      {id: 2, name: 'javaScript'},
      {id: 3, name: 'vue'},
    ]
  }
})

</script>
```
- v-for를 사용할때는 반드시 key속성을 사용해야 함 (각 구성의 순서를 보장하기 위해서 작성)


#### v-on
- addEventListener와 동일한 기능
```
<button v-on:click="method나 변수 사용 가능">
</button>

<button v-on:click="number++">
</button>
```
- v-on:을 @로 대체할 수 있음

#### v-bind
- HTML 속성에 변수를 적용할 수 있음
```
  <a v-bind:href="url">Go To NAVER</a>
  // href 속성에 url 변수 적용
  <a :href="url">Go To NAVER</a>

  <p v-bind:class="redTextClass"></p>
  <p v-bind:class="{ 'red_text' : true}"></p>
  <p v-bind:class="[redTextClass, boarderBlack]"></p>
```
- v-bind를 :으로 변경할 수 있음


#### v-model
- 양방향 바인딩
```
<input v-model="myMessage" type="text"> # 해당 값이 변경되면
<h3>{{ myMessage}}</h3>                 # myMessage 값이 변경됨
```
- 한글은 마지막 글자가 보이지 않는 문제점 발생 (IME로 조합형 언어이기 때문에 늦음)

- 아래 방식으로 대체
```
    <input @input="chagneText" :value="bye_msg"  type="text">
    
    
    <script>
    chagneText (event) {
      this.bye_msg = event.target.value
    }
    </script>
```

## 설치 방법
### 설치
```
npm install -g @vue/cli
```

### 프로젝트 생성
- vscode terminal에서 진행
```
vue create 프로젝트 이름
```

## 컴포넌트 사용하는 방법

#### 1. 컴포넌트 파일 만들기
- components 폴더 안에 만들기

#### 2. script에 이름 등록
```
<script>
export default {
  name:"MyComponent"
}
</script>
```

#### 3. template에 요소 추가
- templates안에는 반드시 하나의 요소만 추가 해야 함
```
<template>
  <div>
    <h1>이거는 내가 만든 새로운 컴포넌트다!!!!!!!!!!!!!!!!!</h1>
  </div>
</template>
```


### component 등록
#### 1. 불러오기
- app.vue에서 import
```
<script>
  import MyComponent from './components/MyComponent.vue'
  import MyComponent from '@/components/MyComponent'
</script>
```
- scr 절대 경로가 @로 설정되어 있음, .vue도 생략가능

#### 2. 등록하기
- app.vue에서 등록
```
<script>
export default {
  name: 'App',
  components: {
    HelloWorld,
    MyComponent,
  }
}
</script>
```

#### 3. 보여주기
- app.vue template에 보여주기
```
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    <MyComponent/>
  </div>
</template>
```

## 값을 전달하는 방법

### Static 정적으로 데이터 보내기
- HTML이기 때문에 소문자로 인식하여 캐밥케이스로 작성
- ParentComponent
```
<ChildComponent msg-test="캐밥 케이스로 전달해주세요." />
```

- ChildComponent
```
<template>
{{ msgTest }}
</template>

export default {
  name:"ChildComponent",
  props: {
    msgTest: String
  }

}
<script>


</script>
```

### 동적으로 보내기
- ParentComponent
```
<ChildComponent :msg-test="변수" />
```
- 부모 컴포넌트의 값이 변하면 자식 컴포넌트 값도 변함


## 부모값의 변경 요청
- 상위 컴포넌트는 이벤트를 통해 데이터를 받음

### Child Component
```
<template>
<button @click="ChildToParent"> 클릭! </button>
</template>

<script>
  export default {
    methods: {
      ChildToParent : function() {    
        # this.$emit('child-to-parent', 'child data')           # static data
        this.$emit('child-to-parent', this.childInputData)      # dynamic data

      }
      
    }
  }
</script>
```

### Parent Component
```
<template>
  <ChildComponent @child-to-parent="parentGetEvent" />
</template>

<script>
  export default {
    methods: {
      parentGetEvent: function(inputDate) {
        console.log('자식 컴포넌트에서 발생한 이벤트!")
        console.log(`child component로 부터 ${inputDate} 받음`)
      }
    }
  }
</script>
  
```

- 주의: 자식의 자식이나 부모의 부모로 점프할 수 없다.
- 중간 컴포넌트를 거쳐 사용해야 한다.
