<template>
  <div class="dashboard">
    <div class="head">
      <h2>ToDoList</h2>
      <el-input v-model="todoTitle" placeholder="添加ToDo" class="todo-input"></el-input>
      <el-button
        type="success"
        icon="el-icon-check"
        circle
        style="margin-left: 15px"
        @click="addTodo"
      ></el-button>
      <h2>正在进行 {{doingArray.length}}</h2>
      <div v-for="(item, index) of doingArray" :key="'doing'+index" style="margin-top: 15px">
        <Item
          v-if="item.done == false"
          :index="index"
          :title="item.title"
          :done="item.done"
          @deleteItem="deleteItem"
          @checkItem="checkItem"
        ></Item>
      </div>
      <h2>已经完成 {{doneArray.length}}</h2>
      <div v-for="(item, index) of doneArray" :key="'done'+index" style="margin-top: 15px">
        <Item
          v-if="item.done == true"
          :index="index"
          :title="item.title"
          :done="item.done"
          @deleteItem="deleteItem"
          @checkItem="checkItem"
        ></Item>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import Item from "./item.vue";

@Component({
  components: {
    Item
  }
})
export default class HelloWorld extends Vue {
  // 添加 Todo 标题
  private todoTitle: string = "";
  // 数据数组
  private dataArray: Array<Object> = [];

  // 待办数组
  private doingArray: Array<Object> = [];
  // 完成数组
  private doneArray: Array<Object> = [];

  // 生命周期
  private mounted(): void {
    this.getTodoList();
  }

  // 获取列表数据
  private getTodoList(): void {
    let dataString: string = localStorage.getItem("todo-list");
    if (dataString != null) {
      this.dataArray = JSON.parse(dataString);
      for (let i = 0; i < this.dataArray.length; i++) {
        const element: any = this.dataArray[i];
        if (element.done == true) {
          this.doneArray.push(element);
        } else {
          this.doingArray.push(element);
        }
      }
    }
  }

  // 添加Todo
  private addTodo(): void {
    if (this.todoTitle == "") {
      this.$message({
        type: "info",
        message: "请输入标题"
      });
    } else {
      let item = {
        done: false,
        title: this.todoTitle
      };
      this.doingArray.push(item);
      this.dataArray.push(item);
      this.todoTitle = "";
      localStorage.setItem("todo-list", JSON.stringify(this.dataArray));
    }
  }

  // 删除项目
  private deleteItem(index: number, done: boolean) {
    if (done) {
      this.doneArray.splice(index, 1);
    } else {
      this.doingArray.splice(index, 1);
    }
    this.dataArray = this.doingArray.concat(this.doneArray);
    localStorage.setItem("todo-list", JSON.stringify(this.dataArray));
  }

  // 打钩
  private checkItem(index: number, done: boolean, title: string) {
    let newItem = {
      done: done,
      title: title
    };
    if (done) {
      this.doingArray.splice(index, 1);
      this.doneArray.push(newItem);
    } else {
      this.doneArray.splice(index, 1);
      this.doingArray.push(newItem);
    }
    this.dataArray = this.doingArray.concat(this.doneArray);
    localStorage.setItem("todo-list", JSON.stringify(this.dataArray));
  }
}
</script>

<style scoped>
.todo-input {
  width: 300px;
}
.item {
  margin: 0px auto;
}
</style>
