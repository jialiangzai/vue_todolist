<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader @addTodo="addTodo"></MyHeader>
        <MyLists :todos="todos"> </MyLists>
        <MyFooter
          :todos="todos"
          @checkAlltodo="checkAlltodo"
          @clearAlltodo="clearAlltodo"
        />
      </div>
    </div>
  </div>
</template>

<script>
import pubsub from "pubsub-js";
import MyHeader from "./components/MyHeader";
import MyLists from "./components/MyLists";
import MyFooter from "./components/MyFooter";

export default {
  name: "App",
  components: {
    MyHeader,
    MyLists,
    MyFooter,
  },
  data() {
    return {
      // todos: [
      //   { id: "001", title: "抽烟", done: true },
      //   { id: "002", title: "喝酒", done: false },
      //   { id: "003", title: "烫头", done: false },
      // ],
      // 本地存储
      todos: JSON.parse(localStorage.getItem("todos")) || [],
    };
  },
  methods: {
    // 添加对象
    addTodo(todoObj) {
      this.todos.unshift(todoObj);
      // console.log(this.todos);
    },
    // 更新\更改done属性是否选中
    checkTodo(id) {
      this.todos.forEach((todo) => {
        if (todo.id === id) todo.done = !todo.done;
      });
    },
    // updateTodo(id, title) {
    //   this.todos.forEach((todo) => {
    //     if (todo.id === id) todo.done = !todo.done;
    //   });
    // },
    updateTodo(id,title) {
      this.todos.forEach((todo) => {
        // 替换title内容
        if (todo.id === id) todo.title = title;
      });
    },

    // 删除
    deleteTodo(_, id) {
      this.todos = this.todos.filter((todo) => todo.id !== id);
    },
    // 全选或者全不选
    checkAlltodo(done) {
      this.todos.forEach((todo) => (todo.done = done));
    },
    clearAlltodo() {
      this.todos = this.todos.filter((todo) => !todo.done);
    },
  },
  watch: {
    todos: {
      deep: true,
      handler(value) {
        localStorage.setItem("todos", JSON.stringify(value));
      },
    },
  },
  mounted() {
    this.$bus.$on("checkTodo", this.checkTodo);
    this.$bus.$on("updateTodo", this.updateTodo);
    // this.$bus.$on('deleteTodo',this.deleteTodo)
    // 消息订阅
    this.pubId = pubsub.subscribe("deleteTodo", this.deleteTodo);
  },
  beforeDestroy() {
    this.$bus.$off("checkTodo");
    this.$bus.$off("updateTodo");

    //  this.$bus.$off('deleteTodo')
    pubsub.unsubscribe(this.pubId);
  },
};
</script>

<style>
/*base*/
body {
  background: #fff;
}

.btn {
  /* display: inline-block; */
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2),
    0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}
.btn-edit {
  color: #fff;
  background-color: skyblue;
  border: 1px solid #123f53;
  margin-right: 5px;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn:focus {
  outline: none;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>
