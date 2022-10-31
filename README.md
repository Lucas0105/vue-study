# vue-study
- [vue 설치방법](#vue-설치방법)
- [vue 속성](#vue-속성)
- [Basic of Syntax](#basic-of-syntax)

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
