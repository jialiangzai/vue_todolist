<template>
  <li>
    <label>
      <input
        type="checkbox"
        :checked="todo.done"
        @change="handleCheck(todo.id)"
      />
      <span v-show="!todo.isEdit">{{ todo.title }}</span>
      <!-- 文本框 -->
      <input
        type="text"
        v-show="todo.isEdit"
        :value="todo.title"
        @blur="handleBlur(todo, $event)"
        ref="inputTitle"
      />
    </label>
    <button
      class="btn btn-danger"
      style="dislay: none"
      @click="handleDelete(todo.id)"
    >
      删除
    </button>
    <button
      v-show="!todo.isEdit"
      class="btn btn-edit"
      style="dislay: none"
      @click="handleEdit(todo)"
    >
      编辑
    </button>
  </li>
</template>

<script>
import pubsub from "pubsub-js";
export default {
  name: "MyItems",
  props: ["todo"],
  methods: {
    handleCheck(id) {
      // console.log(id);
      // 通知APP组件将对应的todo对象中的done取反
      // this.checkTodo(id);
      this.$bus.$emit("checkTodo", id);
    },
    handleDelete(id) {
      // console.log(id);
      if (confirm("确认要删除吗?")) {
        // console.log(id);
        // this.deleteTodo(id);
        //  this.$bus.$emit("deleteTodo", id);
        pubsub.publish("deleteTodo", id);
      }
    },
    handleEdit(todo) {
      // 判断
      if (Object.prototype.hasOwnProperty.call(todo, "isEdit")) {
        todo.isEdit = true;
      } else {
        // console.log('我没有isedit');
        this.$set(todo, "isEdit", true);
      }
      // setTimeout(() => {
      //   this.$refs.inputTitle.focus();
      // });
      this.$nextTick(function () {
        this.$refs.inputTitle.focus();
      })
    },
    // 失去焦点回调真正执行修改逻辑
    handleBlur(todo, e) {
      // 此时todo有isedit属性
      todo.isEdit = false;
      if (!e.target.value.trim()) {
        return alert("输入不能为空");
      } else {
        this.$bus.$emit("updateTodo", todo.id, e.target.value);
      }
    },
  },
};
</script>

<style lang="css" scope>
/*item*/

li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}
/* 背景颜色 */
li:hover {
  background-color: #ebdede;
}
li:hover .btn {
  display: block;
}
</style>