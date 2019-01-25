<template>
	<div>
		<div class="m-b-20 ovf-hd">
			<div class="fl" v-show="addShow">
				<router-link class="btn-link-large add-btn" to="groupadd">
					<i class="el-icon-plus"></i>&nbsp;&nbsp;添加
				</router-link>
			</div>
			<div class="fl w-200 m-l-30">
				<el-input placeholder="请输入公司名" v-model="keywords">
					<el-button slot="append" icon="search" @click="search()"></el-button>
				</el-input>
			</div>
		</div>
		<el-table v-bind:data="tableData" highlight-current-row border height="500" >
      <el-table-column label="#ID" width="">
            <template scope="scope">
              {{scope.row.code}}
            </template>
      </el-table-column>
      <el-table-column label="集团名称" width="">
            <template scope="scope">
              {{scope.row.name}}
            </template>
      </el-table-column>
      <el-table-column label="网站" width="">
            <template scope="scope">
              {{scope.row.web_site}}
            </template>
          </el-table-column>
      <el-table-column label="国家" width="">
            <template scope="scope">
              {{scope.row.country_name}}
            </template>
          </el-table-column>
      <el-table-column label="行业" width="">
            <template scope="scope">
              {{scope.row.industry_name}}
            </template>
          </el-table-column>
      <el-table-column label="类型" width="">
            <template scope="scope">
              {{scope.row.type_name}}
            </template>
          </el-table-column>
      <el-table-column label="市场个数" width="">
            <template scope="scope">
              {{scope.row.num}}
            </template>
          </el-table-column>
      <el-table-column  label="操作">
      <template slot-scope="scope">
        <el-button size="small" type="primary" @click="edit_skip(scope.$index)" >管理</el-button>
      </template>        
      </el-table-column>
		</el-table>
		<div class="pos-rel p-t-20">
			<div class="block pages">
				<el-pagination
				@current-change="handleCurrentChange"
				layout="prev, pager, next"
				:page-size="limit"
				:current-page="currentPage"
				:total="dataCount">
				</el-pagination>
			</div>
		</div>
	</div>
</template>

<script>
  import btnGroup from '../../../Common/btn-group.vue'
  import http from '../../../../assets/js/http'

  export default {
    data() {
      return {
        tableData: [],
        dataCount: null,
        currentPage: null,
        keywords: '',
        multipleSelection: [],
        limit: 10
      }
    },
    methods: {
      search() {
        router.push({ path: this.$route.path, query: { keywords: this.keywords, page: 1 }})
      },
      selectItem(val) {
        this.multipleSelection = val
      },
      handleCurrentChange(page) {
        router.push({ path: this.$route.path, query: { keywords: this.keywords, page: page }})
      },
      confirmDelete(item) {
        this.$confirm('确认删除该用户?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          _g.openGlobalLoading()
          this.apiDelete('admin/users/', item.id).then((res) => {
            _g.closeGlobalLoading()
            this.handelResponse(res, (data) => {
              _g.toastMsg('success', '删除成功')
              setTimeout(() => {
                _g.shallowRefresh(this.$route.name)
              }, 1500)
            })
          })
        }).catch(() => {
          // catch error
        })
      },
      getAllUsers() {
        this.loading = true
        const data = {
          params: {
            keywords: this.keywords,
            page: this.currentPage,
            limit: this.limit
          }
        }
        this.apiGet('group/list', data).then((res) => {
          console.log('res = ', _g.j2s(res))
          this.handelResponse(res, (data) => {
            this.tableData = data.list
            this.dataCount = data.data_count
          })
        })
      },
      getCurrentPage() {
        let data = this.$route.query
        if (data) {
          if (data.page) {
            this.currentPage = parseInt(data.page)
          } else {
            this.currentPage = 1
          }
        }
      },
      getKeywords() {
        let data = this.$route.query
        if (data) {
          if (data.keywords) {
            this.keywords = data.keywords
          } else {
            this.keywords = ''
          }
        }
      },
      init() {
        this.getKeywords()
        this.getCurrentPage()
        this.getAllUsers()
      },
      edit_skip(index) {
        console.log(index)
        this.$router.push({ name: 'usersEdit', params: { id: this.tableData[index].code }})
      }
    },
    created() {
      this.init()
    },
    computed: {
      addShow() {
        return _g.getHasRule('users-save')
      },
      editShow() {
        return _g.getHasRule('users-update')
      },
      deleteShow() {
        return _g.getHasRule('users-delete')
      }
    },
    watch: {
      '$route' (to, from) {
        this.init()
      }
    },
    components: {
      btnGroup
    },
    mixins: [http]
  }
</script>