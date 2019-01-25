<template>
  <el-form ref="market" :model="market" :rules="rules" label-width="130px">
    <el-form-item label="市场名称" :label-width="formLabelWidth">
      <el-input v-model="market.f_market_name"></el-input>
    </el-form-item>
    <el-form-item label="币别" :label-width="formLabelWidth">
            <el-select v-model="market.f_currency_name " placeholder="请选择">
              <el-option
                v-for="currecy in Cur"
                :key="currecy.sign"
                :label="currecy.name"
                :value="currecy.name">
              </el-option>
            </el-select>
            <el-select v-model="market.f_currency_sign " placeholder="请选择">
              <el-option
                v-for="currecy in Cur"
                :key="currecy.sign"
                :label="currecy.sign"
                :value="currecy.sign">
              </el-option>
            </el-select>
    </el-form-item>
    <el-form-item label="数据同步方式" :label-width="formLabelWidth">
            <el-select v-model="market.f_sync_type" placeholder="请选择">
              <el-option
                v-for="item4 in way"
                :key="item4.id"
                :label="item4.name"
                :value="item4.id">
              </el-option>
            </el-select>
    </el-form-item>
    <el-form-item label="备注" :label-width="formLabelWidth">
      <el-input
        type="textarea"
        :rows="2"
        placeholder="请输入内容"
        v-model="market.f_memo">
      </el-input>
    </el-form-item>
    <el-form-item label="清算负责人" :label-width="formLabelWidth">
            <el-select v-model="market.f_charge_user_name" placeholder="请选择">
              <el-option
                v-for="item4 in charge"
                :key="item4.contact_code"
                :label="item4.user_name"
                :value="item4.user_code">
              </el-option>
            </el-select>
    </el-form-item>
    <el-form-item label="清算负责人编号" :label-width="formLabelWidth">
            <el-select v-model="market.f_charge_contact_code" placeholder="请选择">
              <el-option
                v-for="item4 in charge"
                :key="item4.contact_code"
                :label="item4.contact_code"
                :value="item4.cintact_code">
              </el-option>
            </el-select>
    </el-form-item>
    <el-form-item>
    <router-link to="list">
      <el-button type="primary"@click="market_save('market')">完成</el-button>
    </router-link>
    </el-form-item>
  </el-form>
</template>

<script>
  import http from '../../../../assets/js/http'
  import fomrMixin from '../../../../assets/js/form_com'
  export default {
    data() {
      return {
        id: '',
        isLoading: false,
        charge: '',
        way: [{
          id: '0',
          name: '自动'
        }, {
          sign: '1',
          name: '手动'
        }],
        Cur: [{
          sign: '¥',
          name: 'RMB'
        }, {
          sign: '$',
          name: 'USD'
        }],
        market: {
          f_group_code: '',
          f_company_code: '',
          f_market_name: '',
          f_currency_name: '',
          f_currency_sign: '',
          f_sync_type: '',
          f_memo: '',
          f_charge_contact_code: '',
          f_charge_user_name: '',
          f_authorized_user_email: ''
        },
        rules: {
          f_market_name: [
            { required: true, message: '请输入集团名称', trigger: 'blur' }
          ],
          f_currency_name: [
            { required: true, message: '请选择币别', trigger: 'change' }
          ],
          f_currency_sign: [
            { required: true, message: '请选择币别标识符', trigger: 'change' }
          ],
          f_sync_type: [
            { required: true, message: '请选择更新方式', trigger: 'change' }
          ],
          f_charge_user_name: [
            { required: true, message: '请选择负责人', trigger: 'change' }
          ],
          f_charge_user_code: [
            { required: true, message: '请选择负责人编号', trigger: 'change' }
          ]
        }
      }
    },
    methods: {
      get_id() {
        this.market.f_group_code = this.$route.params.jt_id
        this.market.f_company_code = this.$route.params.gs_id
      },
      get_charge() {
        this.apiGet('contact/getCharge/' + this.market.f_group_code).then((res) => {
          this.handelResponse(res, (data) => {
            this.charge = res.data.charge_list
          })
        })
      },
      market_save(market) {
        this.isLoading = !this.isLoading
        this.$refs[market].validate((valid) => {
          if (valid) {
            this.apiPost('market/create', this.market).then((res) => {
              this.handelResponse(res, (data) => {
                _g.toastMsg('success', '编辑成功')
                this.$router.push({ name: 'usersList' })
              })
            })
          }
        })
      }
    },
    created() {
      this.get_id()
      this.get_charge()
    },
    mixins: [http, fomrMixin]
  }
</script>

<style scoped>

</style>