<template>
  <section>
    <el-data-table
      :url="url"
      data-path="data.list"
      totalPath="data.totalSize"
      :searchForm="searchForm"
      :columns="columns"
      :form="form"
      :extraButtons="extraButtons"
      :hasView="true"
    >
      <el-table-column prop="status" label="状态">
        <template slot-scope="scope">
          <span :class="scope.row.status && 'status'">{{scope.row.status | formatStatus}}</span>
        </template>
      </el-table-column>
    </el-data-table>
  </section>
</template>

<style lang="stylus">
.status {
  color: #2baf88;
}
</style>

<script>
export default {
  data() {
    return {
      url: '/users', //相对路径
      columns: [
        // 注释掉 type: selection时, 删除按钮变成一行一个, 此时onDelete回调参数为选中行的数据
        {type: 'selection', selectable: (row, index) => index >= 0},
        {prop: 'componentName', label: '组件名称'},

        {prop: 'type', label: '分类'},
        {prop: 'edition', label: '版本'},
        {prop: 'lnguage', label: '开发语言'},
        {
          prop: 'lastUpdateTime',
          label: '最后更新时间',
          formatter: (row, column, cellValue, index) => {
            let now = new Date(cellValue)
            let year = now.getFullYear()
            let month =
              now.getMonth() + 1 < 10
                ? '0' + (now.getMonth() + 1)
                : now.getMonth() + 1
            let date = now.getDate() < 10 ? '0' + now.getDate() : now.getDate()
            return year + '-' + month + '-' + date
          }
        }
        // {
        //   prop: 'status',
        //   label: '状态',
        //   formatter: row => (row.status ? '上架' : '下架')
        // }
      ],
      searchForm: [
        {
          $el: {placeholder: '请输入'},
          label: '组件名称',
          $id: 'componentName',
          $type: 'input'
        },
        {
          $type: 'select',
          $id: 'type',
          label: '分类',
          $options: [
            {
              label: '前端组件',
              value: 1
            },
            {
              label: '测试子组件',
              value: 2
            },
            {
              label: '分布式工具',
              value: 3
            },
            {
              label: '应用服务',
              value: 4
            },
            {
              label: '数据存储',
              value: 5
            }
          ].map(f => ({label: f.label, value: f.value})),
          $el: {
            placeholder: '请选择'
          }
        },
        {
          $type: 'select',
          $id: 'status',
          label: '状态',
          $options: [
            {label: '上架', value: true},
            {label: '下架', value: false}
          ].map(f => ({label: f.label, value: f.value})),
          $el: {
            placeholder: '请选择'
          }
        }
      ],
      form: [
        {
          $type: 'input',
          $id: 'componentName',
          label: '组件名称',
          $el: {
            placeholder: '请输入'
          },
          rules: [
            {
              required: true,
              message: '请输入组件名称',
              trigger: 'blur'
            }
          ]
        },
        {
          $type: 'select',
          $id: 'type',
          label: '分类',
          rules: [{required: true, message: '请选择分类', trigger: 'blur'}],
          $options: [
            {
              label: '前端组件',
              value: 1
            },
            {
              label: '测试子组件',
              value: 2
            },
            {
              label: '分布式工具',
              value: 3
            },
            {
              label: '应用服务',
              value: 4
            },
            {
              label: '数据存储',
              value: 5
            }
          ].map(f => ({label: f.label, value: f.value})),
          $el: {
            placeholder: '请选择'
          }
        },
        {
          $type: 'input',
          $id: 'edition',
          label: '版本',
          $el: {
            placeholder: '请输入'
          },
          rules: [
            {
              required: true,
              message: '请输入版本号',
              trigger: 'blur'
            }
          ]
        },
        {
          $type: 'input',
          $id: 'lnguage',
          label: '开发语言',
          $el: {
            placeholder: '请输入'
          },
          rules: [
            {
              required: true,
              message: '开发语言',
              trigger: 'blur'
            }
          ]
        },
        {
          $type: 'select',
          $id: 'status',
          label: '状态',
          rules: [{required: true, message: '请选择状态', trigger: 'blur'}],
          $options: [
            {label: '上架', value: true},
            {label: '下架', value: false}
          ].map(f => ({label: f.label, value: f.value})),
          $el: {
            placeholder: '请选择'
          }
        }
      ],
      extraButtons: [
        {
          type: 'default',
          text: '上架',
          textFormatter: row => {
            return row.status ? '下架' : '上架'
          },
          //当前行数据
          atClick: row => {
            return new Promise((resolve, reject) => setTimeout(resolve, 1500))
          }
        }
      ]
    }
  },
  filters: {
    formatStatus: function(val) {
      //取非
      return val ? '上架' : '下架'
    }
  }
}
</script>
