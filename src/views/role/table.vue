<template>
  <div>
    <el-table :data="roleList" border style="width: 100%">
      <el-table-column prop="id" label="角色编号" width="180"></el-table-column>
      <el-table-column
        prop="rolename"
        label="角色名称"
        width="180"
      ></el-table-column>
      <el-table-column prop="status" label="状态">
        <template slot-scope="item">
          <div>
            <el-button v-if="item.row.status == 1" type="success" disabled>启用</el-button>
            <el-button v-else type="danger" disabled>禁用</el-button>
          </div>
        </template>
      </el-table-column>
      <el-table-column label="操作">
        <template slot-scope="item">
          <div>
            <el-button type="primary" @click="edit(item.row.id)"
              >编辑</el-button
            >
            <el-button type="danger" @click="del(item.row.id)">删除</el-button>
          </div>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
// 引入辅助性函数
import { mapActions, mapGetters } from "vuex";
// 引入接口
import { getRoleDelete } from "../../util/axios";
export default {
  data() {
    return {
      //   roleList: [],
    };
  },
  computed: {
    //   获取角色列表
    ...mapGetters({
      roleList: "role/getRoleList",
    }),
  },
  methods: {
    //   获取action方法
    ...mapActions({
      getRoleList: "role/getRoleListAction",
    }),
    //   编辑
    edit(id) {
      this.$emit("edit", id);
    },
    // 删除
    del(id) {
      this.$confirm("确定要删除这条数据吗?！！", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(() => {
          //调取删除接口
          getRoleDelete({ id })
            .then((res) => {
              if (res.data.code == 200) {
                this.$message.success(res.data.msg);
                //重新触发调取列表
                this.getRoleList();
              }
            })
            .catch((err) => {
              this.message.error(err);
            });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除",
          });
        });
    },
  },
  mounted() {
    //   已加载就调用action的方法存值
    this.getRoleList();
  },
};
</script>

<style lang="stylus" scoped></style>