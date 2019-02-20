<template>
	<el-form :model="list" :rules="rules" ref="list" label-width="100px" class="demo-ruleForm">
    <el-form-item label="集团名称" prop="f_name" class="h-40 w-300">
      <el-input v-model="list.f_name"></el-input>
    </el-form-item>
    <el-form-item label="网站" prop="f_web_site" class="h-40 w-300">
      <el-input v-model="list.f_web_site"></el-input>
    </el-form-item>
    <el-form-item label="行业" prop="f_industry_id">
      <el-select v-model="list.f_industry_id" placeholder="请选择">
        <el-option
          v-for="item in Industry"
          :key="item.id"
          :label="item.name"
          :value="item.id">
        </el-option>
      </el-select>
    </el-form-item>
    <el-form-item label="国家" prop="f_country_id">
      <el-select v-model="list.f_country_id" placeholder="请选择">
        <el-option
          v-for="item2 in Country"
          :key="item2.id"
          :label="item2.name"
          :value="item2.id">
        </el-option>
      </el-select>
    </el-form-item>
    <el-form-item label="总机" prop="f_contact_phone" class="h-40 w-300">
      <el-input v-model="list.f_contact_phone"></el-input>
    </el-form-item>
    <el-form-item label="集团编号" prop="f_group_number" class="h-40 w-300">
      <el-input v-model="list.f_group_number"></el-input>
    </el-form-item>
    <el-form-item label="地址" prop="f_address" class="h-40 w-300">
      <el-input v-model="list.f_address"></el-input>
    </el-form-item>
    <el-form-item label="公司类型" prop="f_type_id">
      <el-select v-model="list.f_type_id" placeholder="请选择">
        <el-option
          v-for="item3 in Type"
          :key="item3.id"
          :label="item3.name"
          :value="item3.id">
        </el-option>
      </el-select>
    </el-form-item>
    <el-form-item label="财年截至时间" class="h-40 w-300">
      <el-input v-model="list.f_fiscal_month"></el-input>
    </el-form-item>
    <el-form-item> 
      <el-button type="primary" @click="submitForm('list')" :loading="isLoading">下一步</el-button>
    </el-form-item>
	</el-form>  
</template>
<script>
  import http from '../../../../assets/js/http'
  import fomrMixin from '../../../../assets/js/form_com'

  export default {
    data() {
      return {
        isLoading: false,
        Industry: '',
        Country: '',
        Type: '',
        group_id: '',
        list: {
          f_name: '',
          f_web_site: '',
          f_group_number: '',
          f_address: '',
          f_industry_id: '',
          f_country_id: '',
          f_type_id: '',
          f_contact_phone: '',
          f_fiscal_month: ''
        },
        rules: {
          f_name: [
            { required: true, message: '请输入集团名称', trigger: 'blur' }
          ],
          f_web_site: [
            { required: true, message: '请输入集团网站', trigger: 'blur' }
          ],
          f_group_number: [
            { required: true, message: '请输入集团编号', trigger: 'blur' }
          ],
          f_industry_id: [
            { type: 'number', required: true, message: '请选择行业', trigger: 'change' }
          ],
          f_country_id: [
            { type: 'number', required: true, message: '请选择国家', trigger: 'change' }
          ],
          f_contact_phone: [
            { required: true, message: '请输入总机', trigger: 'blur' }
          ],
          f_address: [
            { required: true, message: '请输入地址', trigger: 'blur' }
          ],
          f_type_id: [
            { type: 'number', required: true, message: '请选择公司类型', trigger: 'change' }
          ]
        }
      }
    },
    methods: {
      read() {
        this.apiGet('group/get/jt_1548139507').then((res) => {
          this.handelResponse(res, (data) => {
            this.Industry = data.industrys
            this.Country = data.countrys
            this.Type = data.types
          })
        })
      },
      submitForm(list) {
        this.$refs[list].validate((valid) => {
          if (valid) {
            this.apiPost('group/create', this.list).then((res) => {
              this.handelResponse(res, (data) => {
                _g.toastMsg('success', '编辑成功')
                this.group_id = res.data.code
                this.$router.push({ name: 'contactAdd', params: { id: res.data.code }})
                console.log(this.$route.params.id)
              })
            })
          }
        })
      }
    },
    created() {
      this.read()
    },
    mixins: [http, fomrMixin]
  }
</script>