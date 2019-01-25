<template>
	<el-form ref="list" :model="list" label-width="100px">
    <el-form-item label="姓" :label-width="formLabelWidth">
      <el-input v-model="list.f_last_name" autocomplete="off"></el-input>
    </el-form-item>
    <el-form-item label="名" :label-width="formLabelWidth">
      <el-input v-model="list.f_first_name" autocomplete="off"></el-input>
    </el-form-item>
    <el-form-item label="职位" :label-width="formLabelWidth">
      <el-input v-model="list.f_user_position" autocomplete="off"></el-input>
    </el-form-item>
    <el-form-item label="电话" :label-width="formLabelWidth">
      <el-input v-model="list.f_user_phone" autocomplete="off"></el-input>
    </el-form-item>
    <el-form-item label="邮箱" :label-width="formLabelWidth">
      <el-input v-model="list.f_user_email" autocomplete="off"></el-input>
    </el-form-item>
    <el-form-item label="相关" :label-width="formLabelWidth">
      <el-select v-model="list.f_user_role" placeholder="请选择">
        <el-option
          v-for="item in roles"
          :key="item.id"
          :label="item.name"
          :value="item.id">
        </el-option>
      </el-select>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="contact_create()">下一步</el-button>
    </el-form-item>
	</el-form>  
</template>
<script>
  import http from '../../../../assets/js/http'
  import fomrMixin from '../../../../assets/js/form_com'

  export default {
    data() {
      return {
        roles: [{
          id: 'Enterprise',
          name: '企业负责人'
        }, {
          id: 'Financial',
          name: '财务负责人'
        }, {
          id: 'Charge',
          name: '清算负责人'
        }],
        list: {
          f_first_name: '',
          f_last_name: '',
          f_user_position: '',
          f_user_phone: '',
          f_user_email: '',
          f_user_role: '',
          f_group_code: ''
        }
      }
    },
    methods: {
      get_id() {
        this.list.f_group_code = this.$route.params.id
        console.log(this.list.f_group_code)
      },
      contact_create() {
        this.apiPost('contact/create', this.list).then((res) => {
          this.handelResponse(res, (data) => {
            _g.toastMsg('success', '编辑成功')
            this.$router.push({ name: 'fdAdd', params: { id: this.list.f_group_code }})
          })
        })
      }
    },
    mounted() {
      this.get_id()
    },
    mixins: [http, fomrMixin]
  }
</script>