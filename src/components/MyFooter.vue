<template>
  <div class="todo-footer" v-show="total">
    <label>
      <!-- <input type="checkbox" :checked="isAll" @change="checkout"/> -->
      <input type="checkbox"  v-model="isAll" />
    </label>
    <span>
      <span>已完成{{ doneTotal }}</span> / 全部{{ total }}
    </span>
    <button class="btn btn-danger" @click="clearAll">清除已完成任务</button>
  </div>
</template>

<script>
export default {
  name: "MyFooter",
  props: ["todos"],//, "checkAlltodo","clearAlltodo"
  computed: {
    // 全部
    total() {
      return this.todos.length;
    },
    // 已做的
    doneTotal() {
      // return 99
      return this.todos.reduce((pre, cur) => pre + (cur.done ? 1 : 0), 0);
    },
    // isAll() {
    //   // 计算属性可套娃 保证全部的不能拿为负数以免没有list仍会选中 返回布尔值
    //   return this.doneTotal === this.total && this.total > 0;
    // },
    isAll: {
      get() {
        return this.doneTotal === this.total && this.total > 0;
      },
      set(value) {
        // console.log(value);
        // return this.checkAlltodo(value);
        return this.$emit('checkAlltodo',value)
      },
    },
  },
  methods: {
  //   checkout(e){
  //     // console.log(e.target.checked);
  //    this.checkAlltodo(e.target.checked)
  //   }
  // 清空
  clearAll(){
    // this.clearAlltodo()
    this.$emit('clearAlltodo')
  }
  },
};
</script>

<style lang="css" scoped>
/*footer*/
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-footer button {
  float: right;
  margin-top: 5px;
}
</style>
