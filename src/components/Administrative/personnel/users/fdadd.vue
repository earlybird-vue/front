<template>
  <el-form ref="list" :model="list" label-width="400px" :rules="rules">
    <el-form-item label="年销售额" prop="f_sale_volume_id">
      <el-select v-model="list.f_sale_volume_id" placeholder="请选择" width="300">
        <el-option
          v-for="item4 in ext"
          :key="item4.id"
          :label="item4.name"
          :value="item4.id">
        </el-option>
      </el-select>
    </el-form-item>
    <el-form-item prop="f_sale_volume">
      <el-input class="w-200" v-model.number="list.f_sale_volume"></el-input>
    </el-form-item>
    <el-form-item label="年采购额" prop="f_purhchase_volume_id" class="m-t-20">
      <el-select v-model="list.f_purhchase_volume_id" placeholder="请选择">
        <el-option
          v-for="item4 in ext"
          :key="item4.id"
          :label="item4.name"
          :value="item4.id">
        </el-option>
      </el-select>
    </el-form-item>
    <el-form-item prop="f_purhchase_volume">
      <el-input class="w-200" v-model.number="list.f_purhchase_volume"></el-input>
    </el-form-item>
    <el-form-item label="现金流情况" prop="f_cashflow_volume_id">
      <el-select v-model="list.f_cashflow_volume_id" placeholder="请选择">
        <el-option
          v-for="item4 in ext"
          :key="item4.id"
          :label="item4.name"
          :value="item4.id">
        </el-option>
      </el-select>
    </el-form-item>
    <el-form-item prop="f_cashflow_volume">
      <el-input class="w-200" v-model.number="list.f_cashflow_volume"></el-input>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="fd_go()">返回</el-button>
      <el-button type="primary" @click="financial_save('list')">下一步</el-button>
    </el-form-item>
  </el-form>
</template>

<script>
  import http from '../../../../assets/js/http'
  import fomrMixin from '../../../../assets/js/form_com'
  export default {
    data() {
      return {
        ext: [{
          id: '1',
          name: '1000万以下'
        }, {
          id: '2',
          name: '1000-5000万'
        }, {
          id: '3',
          name: '5000万-1亿'
        }, {
          id: '4',
          name: '1亿以上'
        }],
        list: {
          f_group_code: '',
          f_sale_volume_id: '',
          f_sale_volume: '',
          f_purhchase_volume_id: '',
          f_purhchase_volume: '',
          f_cashflow_volume_id: '',
          f_cashflow_volume: ''
        },
        rules: {
          f_sale_volume_id: [
            { required: true, message: '请选择销售额范围', trigger: 'change' }
          ],
          f_sale_volume: [
            { type: 'number', required: true, message: '请输入销售额', trigger: 'blur' }
          ],
          f_purhchase_volume_id: [
            { required: true, message: '请选择采购额范围', trigger: 'change' }
          ],
          f_purhchase_volume: [
            { type: 'number', required: true, message: '请输入采购额', trigger: 'blur' }
          ],
          f_cashflow_volume_id: [
            { required: true, message: '请选择现金流动范围', trigger: 'change' }
          ],
          f_cashflow_volume: [
            { type: 'number', required: true, message: '请输入流动额度', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      get_id() {
        this.list.f_group_code = this.$route.params.id
        console.log(this.list.f_group_code)
      },
      financial_save(list) {
        this.$refs[list].validate((valid) => {
          if (valid) {
            this.apiPost('finance/create', this.list).then((res) => {
              this.handelResponse(res, (data) => {
                _g.toastMsg('success', '添加成功')
                this.$router.push({ name: 'companyAdd', params: { id: res.data.group_code }})
              })
            })
          }
        })
      }
    },
    fd_go() {
      this.$router.go(-1)
    },
    created() {
      this.get_id()
    },
    mixins: [http, fomrMixin]
  }
</script>

<style scoped>

</style>