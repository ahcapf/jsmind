<template>
  <div class="report">
    <div class="title">系统测试结论</div>
    <div class="caption">【执行率】：测试计划内的测试用例【执行数 除以 有效用例数】</div>
    <div class="caption">【覆盖率】：分母：测试计划所关联的【研发计划内所有故事数量+历史缺陷数量】：分子：测试计划中测试用例所关联的【故事+历史缺陷】</div>
    <div class="caption">【出口标准】：1.覆盖率达到100%；2.执行率达到100%；3.缺陷全部关闭</div>
    <div class="caption">&nbsp;&nbsp;若判断条件无数据满足，则排除该条判定条件。</div>
    <div class="subtitle">11</div>
    <el-table :data="list" :header-cell-style="{background:'#FAFAFA'}" style="width: 100%" class="myProjectTable">
      <el-table-column prop="projectType" label="总用例数" />
      <el-table-column prop="projectType" label="有效用例数" />
      <el-table-column prop="projectType" label="执行数" />
      <el-table-column prop="projectType" label="通过数" />
      <el-table-column prop="projectType" label="阻碍数" />
      <el-table-column prop="projectType" label="L1/L2缺陷情况（系统测试）" />
      <el-table-column prop="projectType" label="缺陷（关闭总数）（系统测试）" />
      <el-table-column prop="projectType" label="执行率" />
      <el-table-column prop="projectType" label="覆盖率" />
    </el-table>
    <!-- 分页 -->
    <el-pagination
      :current-page="pagination.currentPage"
      :page-sizes="pagination.pageSizes"
      :page-size="pagination.pageSize"
      :total="pagination.total"
      layout="total, sizes, prev, pager, next, jumper"
      background
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"/>
  </div>
</template>

<script>
export default {
  name: 'FunTestReport',
  data() {
    return {
      pagination: {
        currentPage: 1,
        pageSize: 10,
        pageSizes: [10, 20, 500],
        total: 0
      },
      list: [{}]
    }
  },
  mounted() {
  },
  methods: {
    handleSizeChange(val) {
      this.pagination.pageSize = val
      this.myProjectList = this.handlePage(
        this.searchProjectList,
        this.pagination.pageSize,
        this.pagination.currentPage
      )
    },
    handleCurrentChange(val) {
      this.pagination.currentPage = val
      this.myProjectList = this.handlePage(
        this.searchProjectList,
        this.pagination.pageSize,
        this.pagination.currentPage
      )
    },
    // 分页功能
    handlePage(data, pageSize, currentPage) {
      return data.slice(currentPage * 10 - 10, pageSize * currentPage)
    }
  }
}
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
  .report{
    background: white;
    padding: 10px;
    .title{
      border-left: 2px #1e92ff solid;
      padding-left: 8px;
      font-size: 1.4em;
      margin: 10px 0;
      &:first-child{
        margin: 0 0 10px;
      }
    }
    .caption{
      padding: 10px 0 0;
      color: gray;
    }
    .el-pagination {
      float: right;
      margin-top: 20px;
    }
  }
</style>
