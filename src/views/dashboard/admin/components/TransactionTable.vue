<template>
  <div class="processTable">
    <el-table :data="list" style="width: 100%;padding-top: 15px;">
      <el-table-column label="Process ID" min-width="200">
        <template slot-scope="scope">
          {{ scope.row.porcess_id | orderNoFilter }}
        </template>
      </el-table-column>

      <el-table-column label="Start Time" min-width="200">
        <template slot-scope="scope">
          {{ scope.row.start_time }}
        </template>
      </el-table-column>

      <el-table-column label="End Time" min-width="200">
        <template slot-scope="scope">
          {{ scope.row.end_time }}
        </template>
      </el-table-column>
      <!-- <el-table-column label="Price" width="195" align="center">
      <template slot-scope="scope">
        ¥{{ scope.row.price | toThousandFilter }}
      </template>
    </el-table-column> -->
      <el-table-column label="Status" width="100" align="center">
        <template slot-scope="{row}">
          <el-tag :type="row.status | statusFilter">
            {{ row.status }}
          </el-tag>
        </template>
      </el-table-column>

    </el-table>

    <div class="tabListPage">
      <el-pagination
        style="width: 100%;padding-top: 15px;"
        :current-page="currentPage"
        :page-sizes="pageSizes"
        :page-size="PageSize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="totalCount"
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
      />
    </div>
  </div>
</template>

<script>
import { transactionList } from '@/api/remote-search'

export default {
  filters: {
    statusFilter(status) {
      const statusMap = {
        succeded: 'success',
        failed: 'danger',
        warning: 'warning'
      }
      return statusMap[status]
    },
    orderNoFilter(str) {
      return str.substring(0, 30)
    }
  },
  data() {
    return {
      list: null,
      // 总数据
      tableData: [],
      // 默认显示第几页
      currentPage: 1,
      // 总条数，根据接口获取数据长度(注意：这里不能为空)
      totalCount: 1,
      // 个数选择器（可修改）
      pageSizes: [1, 2, 3, 4],
      // 默认每页显示的条数（可修改）
      PageSize: 1
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    fetchData() {
      transactionList().then(response => {
        this.list = response.data.items.slice(0, 8)
      })
    }
  }
}
</script>
