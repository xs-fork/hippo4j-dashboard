<template>
  <el-table :data="list" style="width: 100%;padding-top: 15px;">
    <el-table-column label="threadPool ID" min-width="200">
      <template slot-scope="scope">
        {{ scope.row.groupKey }}
        <!--{{ scope.row.groupKey | orderNoFilter }}-->

      </template>
    </el-table-column>
    <el-table-column label="taskCount" width="195" align="center">
      <template slot-scope="scope">
        {{ scope.row.maxCompletedTaskCount | toThousandFilter }}
      </template>
    </el-table-column>
    <el-table-column label="inst" width="100" align="center">
      <template slot-scope="{row}">
        <el-tag :type="row.inst | statusFilter">
          {{ row.inst }}
        </el-tag>
      </template>
    </el-table-column>
  </el-table>
</template>

<script>
import * as dashborad from '@/api/dashborad'

export default {
  filters: {
    statusFilter (status) {
      const statusMap = {
        success: 'success',
        pending: 'danger'
      }
      return statusMap[status]
    },
    orderNoFilter (str) {
      return str.substring(0, 30)
    }
  },
  data () {
    return {
      list: null
    }
  },
  created () {
    this.fetchData()
  },
  methods: {
    fetchData () {
      this.list = [{ 'order_no': '123456', 'price': '4454', 'status': '1' }]

      dashborad.ranking({}).then(response => {
        this.list = response.rankingChartInfoList

        console.log(response.rankingChartInfoList)
      })
    }
  }
}
</script>
