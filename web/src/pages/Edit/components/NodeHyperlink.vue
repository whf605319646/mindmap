<template>
  <el-dialog
    class="nodeDialog"
    title="超链接"
    :visible.sync="dialogVisible"
    width="500"
  >
    <div class="item">
      <span class="name">链接</span>
      <el-input
        v-model="link"
        size="mini"
        placeholder="http://xxxx.com/"
      ></el-input>
    </div>
    <div class="item">
      <span class="name">名称</span>
      <el-input v-model="linkTitle" size="mini"></el-input>
    </div>
    <span slot="footer" class="dialog-footer">
      <el-button @click="cancel">取 消</el-button>
      <el-button type="primary" @click="confirm">确 定</el-button>
    </span>
  </el-dialog>
</template>

<script>
/**
 * @Author: 王林
 * @Date: 2021-06-24 22:53:17
 * @Desc: 节点超链接内容设置
 */
export default {
  name: "NodeHyperlink",
  data() {
    return {
      dialogVisible: false,
      link: "",
      linkTitle: "",
      activeNodes: [],
    };
  },
  created() {
    this.$bus.$on("node_active", (...args) => {
      this.activeNodes = args[1];
      if (this.activeNodes.length > 0) {
        let firstNode = this.activeNodes[0];
        this.link = firstNode.getData("hyperlink");
        this.linkTitle = firstNode.getData("hyperlinkTitle");
      } else {
        this.link = "";
        this.linkTitle = "";
      }
    });
    this.$bus.$on("showNodeLink", () => {
      this.$bus.$emit('startTextEdit');
      this.dialogVisible = true;
    });
  },
  methods: {
    /**
     * @Author: 王林
     * @Date: 2021-06-22 22:08:11
     * @Desc: 取消
     */
    cancel() {
      this.dialogVisible = false;
      this.$bus.$emit('endTextEdit');
    },

    /**
     * @Author: 王林
     * @Date: 2021-06-06 22:28:20
     * @Desc:  确定
     */
    confirm() {
      this.activeNodes.forEach((node) => {
        node.setHyperlink(this.link, this.linkTitle);
        this.cancel();
      });
    },
  },
};
</script>

<style lang="less" scoped>
.nodeDialog {
  .item {
    display: flex;
    align-items: center;
    margin-bottom: 10px;

    .name {
      display: block;
      width: 50px;
    }
  }
}
</style>