<template>
<div>
	<div class="header">
    联系人信息
    <el-button class="add m-l-860" @click="contact_table_create()"><i class="el-icon-plus">添加</i></el-button></div>
    <el-dialog title="联系人信息" :visible.sync="dialogFormVisible">
      <el-form ref="list" :model="list" :rules="rules" label-width="100px">
        <el-form-item label="姓" :label-width="formLabelWidth" prop="f_last_name">
          <el-input v-model="list.f_last_name" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="名" :label-width="formLabelWidth" prop="f_first_name">
          <el-input v-model="list.f_first_name" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="职位" :label-width="formLabelWidth" prop="f_user_position">
          <el-input v-model="list.f_user_position" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="电话" :label-width="formLabelWidth" prop="f_user_phone">
          <el-input v-model="list.f_user_phone" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="邮箱" :label-width="formLabelWidth" prop="f_user_email">
          <el-input v-model="list.f_user_email" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="相关" :label-width="formLabelWidth" prop="f_user_role">
          <el-checkbox-group v-model="list.f_user_role" @change="handleCheckedRolesChange">
            <el-checkbox v-for="item in roles" :label="item.id" :key="item.id">{{item.name}}</el-checkbox>
          </el-checkbox-group>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="contact_table_create_submit('list')">确定</el-button>
          <el-button type="primary" @click="contact_table_cancel()">取消</el-button>
        </el-form-item>
      </el-form>   
    </el-dialog>
    <el-table
    :data="listdata"
    border
    style="width:100%">
      <el-table-column label="姓" width="" align="center">
        <template scope="scope">
          {{scope.row.f_last_name}}
        </template>
      </el-table-column>
      <el-table-column label="名" width="" align="center">
        <template scope="scope">
          {{scope.row.f_first_name}}
        </template>
      </el-table-column>
      <el-table-column label="职业" width="" align="center">
        <template scope="scope">
          <span>{{scope.row.f_user_position}}</span>
        </template>
      </el-table-column>
      <el-table-column label="电话"  align="center" width="">
        <template scope="scope">
          {{scope.row.f_user_phone}}
        </template>
      </el-table-column>
      <el-table-column align="center" label="邮箱" width="">
        <template scope="scope">
          <span>{{scope.row.f_user_email}}</span>
        </template>
      </el-table-column>
      <el-table-column align="center"  label="操作" width="150">
        <template scope="scope">                          
          <el-button size="mini" @click="table_edit(scope.$index, scope.row)">编辑</el-button>
          <el-dialog title="联系人信息" :visible.sync="dialogFormVisible2">
            <el-form ref="list" :model="list1" :rules="rules" label-width="100px">
              <el-form-item label="姓" :label-width="formLabelWidth" prop="f_last_name">
                <el-input v-model="list1.f_last_name" autocomplete="off"></el-input>
              </el-form-item>
              <el-form-item label="名" :label-width="formLabelWidth" prop="f_first_name">
                <el-input v-model="list1.f_first_name" autocomplete="off"></el-input>
              </el-form-item>
              <el-form-item label="职位" :label-width="formLabelWidth" prop="f_user_position">
                <el-input v-model="list1.f_user_position" autocomplete="off"></el-input>
              </el-form-item>
              <el-form-item label="电话" :label-width="formLabelWidth" prop="f_user_phone">
                <el-input v-model="list1.f_user_phone" autocomplete="off"></el-input>
              </el-form-item>
              <el-form-item label="邮箱" :label-width="formLabelWidth" prop="f_user_email">
                <el-input v-model="list1.f_user_email" autocomplete="off"></el-input>
              </el-form-item>
              <el-form-item label="相关" :label-width="formLabelWidth" prop="f_user_role">
                <el-checkbox-group v-model="list1.f_user_role" @change="handleCheckedRolesChange">
                  <el-checkbox v-for="item in roles" :label="item.id" :key="item.id">{{item.name}}</el-checkbox>
                </el-checkbox-group>
              </el-form-item>
              <el-form-item>
                <el-button type="primary" @click="contact_table_update_submit('list')">确定</el-button>
                <el-button type="primary" @click="contact_table_cancel()">取消</el-button>
              </el-form-item>
            </el-form>   
          </el-dialog>
          <el-button size="mini" type="danger" @click="table_delete(scope.$index, scope.row)">删除</el-button>           
        </template>
      </el-table-column>
    </el-table>
    <el-button type="primary" @click="contact_create()">下一步</el-button>
    <el-button type="primary" @click="contact_go()">返回</el-button>
</div>
</template>
<script>
  import http from '../../../../assets/js/http'
  import fomrMixin from '../../../../assets/js/form_com'

  export default {
    data() {
      let validphone = (rule, value, callback) => {
        let reg = /^1(3|4|5|7|8)[0-9]{9}$/
        if (!reg.test(value)) {
          callback(new Error('您输入的电话号码有误'))
        } else {
          callback()
        }
      }
      return {
        roles: [{
          id: 'enterprise',
          name: '企业负责人'
        }, {
          id: 'financial',
          name: '财务负责人'
        }, {
          id: 'charge',
          name: '清算负责人'
        }],
        dialogFormVisible: false,
        dialogFormVisible2: false,
        index: null,
        checkedRoles: [],
        listdata: [],
        list: {
          f_first_name: '',
          f_last_name: '',
          f_user_position: '',
          f_user_phone: '',
          f_user_email: '',
          f_user_role: [],
          f_group_code: ''
        },
        list1: '',
        rules: {
          f_last_name: [
            { required: true, message: '请输入姓氏', trigger: 'blur' }
          ],
          f_first_name: [
            { required: true, message: '请输入名字', trigger: 'blur' }
          ],
          f_user_position: [
            { required: true, message: '请输入职位', trigger: 'blur' }
          ],
          f_user_phone: [
            { required: true, message: '请输入电话', trigger: 'blur' },
            { validator: validphone, trigger: 'blur' }
          ],
          f_user_email: [
            { type: 'email', required: true, message: '请填写邮箱', trigger: 'blur' }
          ],
          f_user_role: [
            { required: true, message: '请选择相关负责人', trigger: 'change' }
          ]
        }
      }
    },
    methods: {
      get_id() {
        this.list.f_group_code = this.$route.params.id
        console.log(this.list.f_group_code)
      },
      init() {
        this.list = {
          f_first_name: '',
          f_last_name: '',
          f_user_position: '',
          f_user_phone: '',
          f_user_email: '',
          f_user_role: [],
          f_group_code: ''
        }
      },
      contact_table_create() {
        this.list = {}
        this.init()
        this.get_id()
        this.dialogFormVisible = true
      },
      contact_table_cancel() {
        this.list = {}
        this.dialogFormVisible = false
      },
      contact_table_create_submit(list) {
        this.$refs[list].validate((valid) => {
          if (valid) {
            this.listdata.push(this.list)
            this.dialogFormVisible = false
            console.log(this.listdata)
            console.log(this.list.f_group_code)
          }
        })
      },
      contact_table_update_submit(list) {
        this.$refs[list].validate((valid) => {
          if (valid) {
            this.listdata.splice(this.index, 1)
            this.listdata.push(this.list1)
            this.dialogFormVisible2 = false
          }
        })
      },
      table_edit(index, row) {
        this.dialogFormVisible2 = true
        this.list1 = Object.assign({}, row)
        this.index = index
        this.get_id()
      },
      table_delete(index, row) {
        this.listdata.splice(index, 1)
      },
      contact_create() {
        this.apiPost('contact/create', this.listdata).then((res) => {
          this.handelResponse(res, (data) => {
            console.log(this.listdata)
            _g.toastMsg('success', '添加成功')
            this.$router.push({ name: 'fdAdd', params: { id: this.listdata[0].f_group_code }})
          })
        })
      },
      contact_go() {
        this.$router.go(-1)
      },
      handleCheckedRolesChange(value) {
        let checkcount = value.length
        this.checkAll = checkcount === this.roles.length
      }
    },
    beforeRouteLeave(to, from, next) {
      to.meta.keepAlive = true
      next()
    },
    mounted() {
      this.get_id()
    },
    mixins: [http, fomrMixin]
  }
</script>