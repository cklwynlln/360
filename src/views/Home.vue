<template>
  <div class="container">
    <div class="addComment">
      <el-input
        type="textarea"
        placeholder="请输入评论内容"
        v-model="content"
        :rows="5"
        style="width: 30%; margin-right: 10px"
      >
      </el-input>
      <el-button @click="addCom(user)">添加评论</el-button>
    </div>
    <div class="addReply">
      <CommentTree
        v-for="item in contents"
        :data="item"
        :key="item.id"
        children="replys"
        :extra="user"
      />
    </div>
  </div>
</template>

<script>
import CommentTree from "@/components/CommentTree";
export default {
  components: { CommentTree },
  name: "Copy",
  data() {
    return {
      visible: {},
      showDetail: false,
      user: {
        nickname: "wangyu",
      },
      content: "",
      contents: [
        {
          id: 1,
          nickname: "xxx1",
          time: "2022.5.17",
          content: "很好",
          replys: [
            { id: 1.1, nickname: "xxx1", time: "2022.5.20", content: "很好o" },
          ],
        },
        {
          id: 2,
          nickname: "xxx2",
          time: "2022.5.17",
          content: "很好",
          replys: [],
        },
        {
          id: 3,
          nickname: "xxx3",
          time: "2022.5.17",
          content: "很好",
          replys: [],
        },
      ],
    };
  },
  computed: {
    // 当前日期
    currentDate() {
      return new Date().toLocaleString("chinese", { hour12: false });
    },
  },
  methods: {
    addCom(user) {
      let comment = {
        id: 0 + "." + Math.random(),
        nickname: user.nickname,
        time: this.currentDate,
        content: this.content,
        replys: [],
      };
      this.content = "";
      this.contents.unshift(comment);
    },
  },
};
</script>
<style>
p {
  text-decoration: underline;
}
p:hover {
  cursor: pointer;
  color: blue;
}
</style>
