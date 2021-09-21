<template>
  <ul class="todo-main">
    <MyItems v-for="todoObj in todos" :key="todoObj.id" :todo="todoObj" />
  </ul>
</template>

<script>
import MyItems from "./MyItems.vue";
export default {
  name: "MyLists",
  components: { MyItems },
  props:['todos']
};
</script>

<style lang="css" scoped>
/*main*/
.todo-main {
  margin-left: 0px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0px;
}

.todo-empty {
  height: 40px;
  line-height: 40px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding-left: 5px;
  margin-top: 10px;
}
</style>
