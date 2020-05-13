<template>
    <div>
        <!--        面包屑导航区域-->
        <el-breadcrumb separator-class="el-icon-arrow-right">
            <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
            <el-breadcrumb-item>权限管理</el-breadcrumb-item>
            <el-breadcrumb-item>权限列表</el-breadcrumb-item>
        </el-breadcrumb>

        <!--        卡片视图-->
        <el-card>
            <el-table :data="showList" border stripe>
                <el-table-column type="index"></el-table-column>
                <el-table-column label="权限名称" prop="authName"></el-table-column>
                <el-table-column label="路径" prop="path"></el-table-column>
                <el-table-column label="权限等级" prop="level">
                    <template slot-scope="scope">
                        <el-tag v-if="scope.row.level==='0'">一级</el-tag>
                        <el-tag type="success" v-else-if="scope.row.level==='1'">二级</el-tag>
                        <el-tag type="warning" v-else>三级</el-tag>
                    </template>
                </el-table-column>
            </el-table>
            <el-pagination
                    @size-change="handleSizeChange"
                    @current-change="handleCurrentChange"
                    :current-page="paginations.page_index"
                    :page-sizes="paginations.page_sizes"
                    :page-size="paginations.page_size"
                    :layout="paginations.layout"
                    :total="paginations.total">
            </el-pagination>
        </el-card>
    </div>
</template>

<script>
export default {
  created () {
    this.getRightList()
  },
  name: 'Rights',
  data () {
    return {
      showList: [],
      rightList: [],
      paginations: {
        page_index: 1,
        total: 0,
        page_size: 5,
        page_sizes: [5, 10, 15, 20],
        layout: 'total, sizes, prev, pager, next, jumper'
      }
    }
  },
  methods: {
    async getRightList () {
      const { data: res } = await this.$http.get('rights/list')
      if (res.meta.status !== 200) return this.$message.error('获取权限列表失败')
      this.rightList = res.data
      this.setPaginations()
    },
    setPaginations () {
      this.paginations.total = this.rightList.length
      this.paginations.page_index = 1
      this.paginations.page_size = 5

      this.showList = this.rightList.filter((item, index) => {
        return index < this.paginations.page_size
      })
    },
    // eslint-disable-next-line camelcase
    handleSizeChange (page_size) {
      this.paginations.page_index = 1
      // eslint-disable-next-line camelcase
      this.paginations.page_size = page_size
      this.showList = this.rightList.filter((item, index) => {
        // eslint-disable-next-line camelcase
        return index < page_size
      })
    },
    handleCurrentChange (page) {
      const index = this.paginations.page_size * (page - 1)
      const allData = this.paginations.page_size * page
      const tablist = []
      for (let i = index; i < allData; i++) {
        if (this.rightList[i]) {
          tablist.push(this.rightList[i])
        }
        this.showList = tablist
      }
    }
  }
}
</script>

<style scoped>

</style>
