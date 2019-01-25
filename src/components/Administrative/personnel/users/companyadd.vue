<template>
  <el-form ref="form" :model="company" label-width="130px">
    <el-form-item label="公司中文名称" :label-width="formLabelWidth">
      <el-input v-model="company.f_name" autocomplete="off"></el-input>
    </el-form-item>
    <el-form-item label="公司英文名称" :label-width="formLabelWidth">
      <el-input v-model="company.f_en_name" autocomplete="off"></el-input>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="company_create()">下一步</el-button>
    </el-form-item>
  </el-form>
</template>
<script>
  import http from '../../../../assets/js/http'
  import fomrMixin from '../../../../assets/js/form_com'
  export default {
    data() {
      return {
        companyId: '',
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
      company_create() {
        if (!this.company.f_group_code) {
          alert('请先填写集团信息')
        } else {
          this.apiPost('company/create', this.company).then((res) => {
            this.handelResponse(res, (data) => {
              _g.toastMsg('success', '编辑成功')
              this.companyId = res.data.company_code
              this.$router.push({ name: 'marketAdd', params: { jt_id: res.data.group_code, gs_id: res.data.company_code }})
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