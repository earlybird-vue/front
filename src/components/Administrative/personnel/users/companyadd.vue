<template>
  <div class="header">
    公司信息 
    <el-button class="add" size="mini" @click="table_company_create()"><i class="el-icon-plus">添加</i></el-button>
    <el-dialog title="公司信息" :visible.sync="dialogFormVisible"> 
      <el-form ref="form" :model="company" label-width="130px">
        <el-form-item label="公司中文名称" :label-width="formLabelWidth">
          <el-input v-model="company.f_name" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="公司英文名称" :label-width="formLabelWidth">
          <el-input v-model="company.f_en_name" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="table_company_cancel()">取消</el-button>
          <el-button type="primary" @click="table_company_create_submit()">确定</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>  
    <el-table
      :data="companydata"
      border
      style="width:100%">
      <el-table-column
        label="公司中文名称"
        width="">
        <template scope="scope">
          <span>{{scope.row.f_name}}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="公司英文名称"
        width="">
        <template scope="scope">
          <span>{{scope.row.f_en_name}}</span>
        </template>
      </el-table-column>
      <el-table-column align="center"  label="操作" width="">
        <template scope="scope">                          
          <el-button size="mini" @click="table_edit(scope.$index, scope.row)">编辑</el-button>
          <el-button size="mini" @click="table_delete(scope.$index)">删除</el-button>          
        </template>
      </el-table-column>
    </el-table>
    <el-button type="primary" @click="company_create()">下一步</el-button>
  </div>
</template>
<script>
  import http from '../../../../assets/js/http'
  import fomrMixin from '../../../../assets/js/form_com'
  export default {
    data() {
      return {
        companyId: '',
        dialogFormVisible: false,
        companydata: [],
        company: {
          f_name: '',
          f_en_name: '',
          f_group_code: ''
        }
      }
    },
    methods: {
      get_id() {
        this.company.f_group_code = this.$route.params.id
        console.log(this.company.f_group_code)
      },
      company_init() {
        this.company = {
          f_name: '',
          f_en_name: '',
          f_group_code: ''
        }
      },
      table_company_create() {
        this.company = {}
        this.company_init()
        this.get_id()
        this.dialogFormVisible = true
      },
      table_company_create_submit() {
        this.companydata.push(this.company)
        this.dialogFormVisible = false
      },
      table_company_cancel() {
        this.company = {}
        this.dialogFormVisible = false
      },
      table_edit(index, row) {
        this.dialogFormVisible = true
        this.company = Object.assign({}, row)
        this.companydata.splice(index, 1)
        this.get_id()
      },
      table_delete(index) {
        this.companydata.splice(index, 1)
      },
      company_create() {
        if (!this.company.f_group_code) {
          alert('请先填写集团信息')
        } else {
          this.apiPost('company/create', this.companydata).then((res) => {
            this.handelResponse(res, (data) => {
              _g.toastMsg('success', '编辑成功')
              this.companyId = res.data.company_code
              this.$router.push({ name: 'marketAdd', params: { id: res.data[0].group_code }})
              console.log(res.data[0])
            })
          })
        }
      }
    },
    created() {
      this.get_id()
    },
    mixins: [http, fomrMixin]
  }
</script>

<style scoped>

</style>