<template>
  <div>
    <!-- 面包屑导航区域 -->
    <Breadcrumb name1="权限管理" name2="权限列表" />
    <!-- 内容主体 -->
    <el-card class="box-card">
      <el-table :data="rightsList" border stripe>
        <el-table-column type="index" label="#"></el-table-column>
        <el-table-column prop="authName" label="权限名称"></el-table-column>
        <el-table-column prop="path" label="路径"></el-table-column>
        <el-table-column prop="level" label="权限等级">
          <template slot-scope="scope">
            <el-tag v-if="scope.row.level === '0'">一级</el-tag>
            <el-tag type="success" v-else-if="scope.row.level === '1'">二级</el-tag>
            <el-tag type="warning" v-else>三级</el-tag>
          </template>
        </el-table-column>
      </el-table>
    </el-card>
  </div>
</template>

<script>
import Breadcrumb from '../Breadcrumb.vue'
export default {
  name: 'Rights',
  components: { Breadcrumb },
  data() {
    return {
      rightsList: [],
      level: [
        { type: '', label: '一级' },
        { type: 'success', label: '二级' },
        { type: 'warning', label: '三级' }
      ]
    }
  },
  created() {
    this.getRightList()
  },
  methods: {
    async getRightList() {
      const { data: res } = await this.$http.get('rights/list')
      if (res.meta.status !== 200) return this.$message.error('获取权限列表失败')
      this.rightsList = res.data
    }
  }
}
</script>
