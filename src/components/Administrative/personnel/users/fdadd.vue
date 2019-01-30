<template>
  <el-form ref="form" :model="list" label-width="130px">
    <el-form-item label="年销售额">
      <el-select v-model="list.f_sale_volume_id" placeholder="请选择">
        <el-option
          v-for="item4 in ext"
          :key="item4.id"
          :label="item4.name"
          :value="item4.id">
        </el-option>
      </el-select>
      <el-input v-model="list.f_sale_volume" class="h-40 w-200"></el-input>
    </el-form-item>
    <el-form-item label="年采购额">
      <el-select v-model="list.f_purhchase_volume_id" placeholder="请选择">
        <el-option
          v-for="item4 in ext"
          :key="item4.id"
          :label="item4.name"
          :value="item4.id">
        </el-option>
      </el-select>
      <el-input v-model="list.f_purhchase_volume" class="h-40 w-200"></el-input>
    </el-form-item>
    <el-form-item label="现金流情况">
      <el-select v-model="list.f_cashflow_volume_id" placeholder="请选择">
        <el-option
          v-for="item4 in ext"
          :key="item4.id"
          :label="item4.name"
          :value="item4.id">
        </el-option>
      </el-select>
      <el-input v-model="list.f_cashflow_volume" class="h-40 w-200"></el-input>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="fd_go()">返回</el-button>
      <el-button type="primary" @click="financial_save()">下一步</el-button>
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
        }
      }
    },
    methods: {
      get_id() {
        this.list.f_group_code = this.$route.params.id
        console.log(this.list.f_group_code)
      },
      financial_save() {
        this.apiPost('finance/create', this.list).then((res) => {
          this.handelResponse(res, (data) => {
            _g.toastMsg('success', '编辑成功')
            this.$router.push({ name: 'companyAdd', params: { id: res.data.group_code }})
          })
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