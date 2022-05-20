<template>
  <ul>
    <li>
      <span>{{ data.nickname }} {{ data.time }}</span>
      <el-button
        style="margin-left: 10px"
        type="text"
        v-show="data[children] && data[children].length > 0"
        @click="showChildren = !showChildren"
        >{{ ["展开", "隐藏"][Number(showChildren)] }}</el-button
      >
      <h5>{{ data.content }}</h5>
      <p @click="showReply(data.id)">reply</p>
      <div class="reply-input">
        <el-input
          placeholder="请输入你的回复"
          v-show="visible[data.id]"
          v-model="userReply[data.id]"
          style="margin-right: 10px"
        ></el-input>
        <el-button v-show="visible[data.id]" @click="addReply(data)"
          >提交</el-button
        >
      </div>
      <!-- 递归调用自身 -->
      <CommentTree
        v-for="item in data[children]"
        :data="item"
        v-show="showChildren"
        :key="item.id"
        :children="children"
      ></CommentTree>
    </li>
  </ul>
</template>
<script>
export default {
  name: "CommentTree",
  props: {
    // 评论数据
    data: {
      default: function () {
        return {};
      },
    },
    // children 字段映射
    children: {
      default: "children",
    },
    // 额外参数
    extra: {
      default: () => {},
    },
  },
  data() {
    return {
      visible: {},
      userReply: {},
      showChildren: true,
    };
  },
  methods: {
    showReply(id) {
      const visible = this.visible[id];
      // 确保数据响应式
      this.$set(this.visible, id, !visible);
    },
    addReply(item) {
      const { id } = item;
      const form = {
        id: id + "." + Math.random(), // 随机评论id确保唯一
        time: new Date().toLocaleString("chinese", { hour12: false }),
        content: this.userReply[item.id],
        replys: [],
        ...this.extra,
      };
      this.showChildren = true;

      item.replys = Object.assign([], item.replys);
      item.replys.unshift(form);
      this.userReply[item.id] = "";
    },
  },
};
</script>
<style lang="css">
.reply-input {
  display: flex;
  align-items: center;
  width: 300px;
}
</style>
