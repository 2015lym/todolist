<template>
  <div class="item">
    <div class="left-item">
      <el-checkbox size="mini" v-model="done" @change="checkChange" class="item-checkbox"></el-checkbox>
      <input v-model="title" :disabled="true" class="item-tilte">
    </div>
    <div class="right-item">
      <el-button
        @click="deleteItem"
        type="danger"
        icon="el-icon-delete"
        circle
        size="mini"
        class="item-del"
      ></el-button>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

@Component
export default class HelloWorld extends Vue {
  // 传入参数  index、状态、标题
  @Prop() private index!: string;
  @Prop() private done!: boolean;
  @Prop() private title!: string;

  mounted(): void {}

  checkChange(): void {
    this.$emit("checkItem", this.index, this.done, this.title);
  }

  deleteItem(): void {
    this.$emit("deleteItem", this.index, this.done);
  }
}
</script>

<style scoped>
.item {
  height: 30px;
  width: 370px;
  border-bottom: 1px solid #dcdfe6;
}
.left-item {
  width: 300px;
  height: 30px;
  text-align: left;
  float: left;
}
.right-item {
  width: 60px;
  height: 30px;
  float: right;
}
.item-checkbox {
  margin-left: 10px;
  line-height: 30px;
}
.item-tilte {
  margin-left: 10px;
  font-size: 19px;
}
input {
  border: none;
  outline: none;
}
</style>
