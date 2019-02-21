<template>
  <div class="header">
    公司信息 
    <el-button class="add" @click="table_company_create()"><i class="el-icon-plus">添加</i></el-button>
    <el-dialog title="公司信息" :visible.sync="dialogFormVisible"> 
      <el-form ref="company" :model="company" label-width="130px" :rules="rules">
        <el-form-item label="公司中文名称" :label-width="formLabelWidth" prop="f_name">
          <el-input v-model="company.f_name" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="公司英文名称" :label-width="formLabelWidth" prop="f_en_name">
          <el-input v-model="company.f_en_name" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="table_company_cancel()">取消</el-button>
          <el-button type="primary" @click="table_company_create_submit('company')">确定</el-button>
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
          <el-dialog title="公司信息" :visible.sync="dialogFormVisible2"> 
            <el-form ref="company1" :model="company1" label-width="130px" :rules="rules">
              <el-form-item label="公司中文名称" :label-width="formLabelWidth" prop="f_name">
                <el-input v-model="company1.f_name" autocomplete="off"></el-input>
              </el-form-item>
              <el-form-item label="公司英文名称" :label-width="formLabelWidth" prop="f_en_name">
                <el-input v-model="company1.f_en_name" autocomplete="off"></el-input>
              </el-form-item>
              <el-form-item>
                <el-button type="primary" @click="dialogFormVisible2 = false">取消</el-button>
                <el-button type="primary" @click="table_company_update_submit('company1')">确定</el-button>
              </el-form-item>
            </el-form>
          </el-dialog>  
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
      let validen_name = (rule, value, callback) => {
        let reg = /^[a-zA-Z]+$/
        if (!reg.test(value)) {
          callback(new Error('只能输入字母'))
        } else {
          callback()
        }
      }
      return {
        companyId: '',
        index: null,
        dialogFormVisible: false,
        dialogFormVisible2: false,
        companydata: [],
        company: {
          f_name: '',
          f_en_name: '',
          f_group_code: ''
        },
        company1: '',
        rules: {
          f_name: [
            { required: true, message: '请输入公司名称', trigger: 'blur' }
          ],
          f_en_name: [
            { required: true, message: '请输入公司英文名称', trigger: 'blur' },
            { validator: validen_name, trigger: 'blur' }
          ]
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
      table_company_create_submit(company) {
        this.$refs[company].validate((valid) => {
          if (valid) {
            this.companydata.push(this.company)
            this.dialogFormVisible = false
          }
        })
      },
      table_company_update_submit(company) {
        this.$refs[company].validate((valid) => {
          if (valid) {
            this.companydata.splice(this.index, 1)
            this.companydata.push(this.company1)
            this.dialogFormVisible2 = false
          }
        })
      },
      table_company_cancel() {
        this.company = {}
        this.dialogFormVisible = false
      },
      table_edit(index, row) {
        this.dialogFormVisible2 = true
        this.company1 = Object.assign({}, row)
        this.index = index
        this.get_id()
        console.log(this.index)
      },
      table_delete(index) {
        this.companydata.splice(index, 1)
      },
      company_create() {
        this.apiPost('company/create', this.companydata).then((res) => {
          this.handelResponse(res, (data) => {
            _g.toastMsg('success', '添加成功')
            this.companyId = res.data.company_code
            this.$router.push({ name: 'marketAdd', params: { id: res.data[0].group_code }})
            console.log(res.data[0])
          })
        })
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