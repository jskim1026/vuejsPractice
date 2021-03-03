<template>
  <div id="app">
    <todo-header></todo-header>
    <!-- <todo-input v-on:하위 컴포넌트에서 발생시킨 이벤트 이름="현재 컴포넌트의 메소드 명"></todo-input> -->
    <todo-input v-on:addTodoItem="addOneItem"></todo-input>
    <!-- <todo-list v-bind:내려보낼 프롭스 속성 이름="현재 위치의 컴포넌트 데이터 속성"></todo-list> -->
    <todo-list v-bind:propsdata="todoItems"
      v-on:removeItem="removeOneItem"
      v-on:toggleItem="toggleOneItem">
    </todo-list>
    <todo-footer v-on:clearAll="clearAllItem"></todo-footer>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoInput from './components/TodoInput.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue'


export default {
  data() {
    return {
      todoItems: []
    }
  },
  methods: {
    addOneItem(todoItem) {
      const obj = {completed: false, item: todoItem};
      localStorage.setItem(todoItem, JSON.stringify(obj)); // 개발자도구 > Application > LocalStorage
      this.todoItems.push(obj);
    },
    removeOneItem(todoItem, index) {      
      localStorage.removeItem(todoItem.item);
      this.todoItems.splice(index, 1);
    },
    toggleOneItem(todoItem, index) {
      // todoItem.completed = !todoItem.completed;
      // 밑의 코드와 같은 동작이지만, 해당 vue에 선언된 변수이기 때문에 this로 제어함
      this.todoItems[index].completed = !this.todoItems[index].completed;

      // 로컬 스토리지 데이터 갱신
      // localStorage.removeItem(todoItem.item);
      localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
    },
    clearAllItem() {
      localStorage.clear();
      this.todoItems = [];
    }
  },
  created() {
      if(localStorage.length >0) {
          for(let i=0; i<localStorage.length; i++) {
              if(localStorage.key(i) !== 'loglevel:webpack-dev-server') {
                  this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))));
                  // this.todoItems.push(localStorage.key(i));
                  // console.log(localStorage.key(i));
              }
          }
      }
  },
  components: {
    // 향상된 객체 리터럴 적용
    /*'TodoHeader': TodoHeader,
    'TodoInput': TodoInput,
    'TodoList': TodoList,
    'TodoFooter': TodoFooter*/
    TodoHeader,
    TodoInput,
    TodoList,
    TodoFooter
  }
}
</script>

<style>
body {
  text-align: center;
  background-color: #F6F6F6;
}
input {
  border-style: groove;
  width: 200px;
}
button {
  border-style: groove;
}
.shadow {
  box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03);
}
</style>
