<template>
  <div class="todo-header">
    <input type="text" placeholder="请输入你的任务名称，按回车键确认"  @keyup.enter="add"  v-model="title" />
  </div>
</template>

<script>
import { nanoid } from "nanoid";
export default {
  name: "MyHeader",
  // props: ["addTodo"],
  data() {
    return {
      title: "",
    };
  },
  methods: {
    add(e) {
      if (!this.title.trim()) return alert("请输入内容");
      // console.log(e.target.value)
      // 将用户输入的包装成一个对象 id模拟数据库
      const todoObj = { id: nanoid(), title: e.target.value, done: false };
      // this.addTodo(todoObj)
      this.$emit('addTodo',todoObj)
      // console.log(todoObj);
      this.title = "";
      // console.log(this);
    },
  },
};
</script>

<style lang="css" scoped>
/*header*/
.todo-header input {
  width: 560px;
  height: 28px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 4px 7px;
}

.todo-header input:focus {
  outline: none;
  border-color: rgba(82, 168, 236, 0.8);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075),
    0 0 8px rgba(82, 168, 236, 0.6);
}
</style>
