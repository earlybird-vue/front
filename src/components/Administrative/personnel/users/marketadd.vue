<template>
  <div class="header">
    市场信息
    <el-button class="add" size="mini" @click="table_market_create()"><i class="el-icon-plus">添加</i></el-button>   
    <el-dialog title="市场信息" :visible.sync="dialogFormVisible">
      <el-form ref="market" :model="market" :rules="rules" label-width="130px">
        <el-form-item label="市场名称" :label-width="formLabelWidth" prop="f_market_name">
          <el-input v-model="market.f_market_name" class='w-200'></el-input>
        </el-form-item>
        <el-form-item label="所属公司" :label-width="formLabelWidth" prop="f_company_code">
          <el-select v-model="market.f_company_code" placeholder="请选择">
            <el-option
              v-for="item in company"
              :key="item.company_code"
              :label="item.name"
              :value="item.company_code">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="币别" :label-width="formLabelWidth" prop="f_currency_name">
          <el-select v-model="market.f_currency_name" placeholder="请选择">
            <el-option
              v-for="currecy in Cur"
              :key="currecy.sign"
              :label="currecy.name"
              :value="currecy.name">
            </el-option>
          </el-select>
          <el-select v-model="market.f_currency_sign" placeholder="请选择" prop="f_currency_sign">
            <el-option
              v-for="currecy in Cur"
              :key="currecy.sign"
              :label="currecy.sign"
              :value="currecy.sign">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="数据同步方式" :label-width="formLabelWidth" prop="f_sync_type">
          <el-select v-model="market.f_sync_type" placeholder="请选择">
            <el-option
              v-for="item in way"
              :key="item.id"
              :label="item.name"
              :value="item.id">
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
        <el-form-item label="清算负责人" :label-width="formLabelWidth" prop="f_charge_user_name">
          <el-select v-model="market.f_charge_user_name" placeholder="请选择">
            <el-option
              v-for="item4 in charge"
              :key="item4.contact_code"
              :label="item4.user_name"
              :value="item4.user_name">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="清算负责人编号" :label-width="formLabelWidth" prop="f_charge_user_code">
          <el-select v-model="market.f_charge_user_code" placeholder="请选择">
            <el-option
              v-for="item4 in charge"
              :key="item4.contact_code"
              :label="item4.contact_code"
              :value="item4.contact_code">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="授权邮箱">
          <el-button type="text" @click="table_market_create_authorize()">添加</el-button><i class="el-icon-edit"></i>
            <el-dialog title="邮箱列表" :visible.sync="dialogFormVisible2" append-to-body>
              <el-form :model="market2">
                <el-form-item label="邮箱选择" :label-width="formLabelWidth">
                  <el-select v-model="market2.f_authorized" placeholder="请选择">
                    <el-option
                      v-for="item4 in authorize"
                      :key="item4.user_code"
                      :label="item4.user_email"
                      :value="item4">
                    </el-option>
                  </el-select>
                </el-form-item>
              </el-form>
              <div slot="footer" class="dialog-footer">
                <el-button @click="dialogFormVisible2 = false">取 消</el-button>
                <el-button type="primary" @click="table_market_create_authorize_submit()">确 定</el-button>
              </div>
            </el-dialog>
          <el-table
            :data="authordata"
            border
            style="width:100%">
            <el-table-column
              label="授权邮箱"
              width="">
              <template scope="scope">
                <span>{{scope.row.user_email}}</span>
              </template>
            </el-table-column>
            <el-table-column
              label="邮箱拥有者"
              width="">
              <template scope="scope">
                <span>{{scope.row.user_name}}</span>
              </template>
            </el-table-column>
            <el-table-column
              label="邮箱拥有者手机"
              width="">
              <template scope="scope">
                <span>{{scope.row.user_phone}}</span>
              </template>
            </el-table-column>
            <el-table-column align="center"  label="操作" width="">
                <template scope="scope">                          
                  <el-button size="mini" @click="table_delete(scope.$index, scope.row)">删除</el-button>            
                </template>
            </el-table-column>
          </el-table>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="table_market_create_submit('market')">完成</el-button>
        </el-form-item>
      </el-form>
      </el-dialog>
    <el-table
      :data="marketdata"
      border
      style="width:100%">
      <el-table-column label="市场名称"  align="center" width="">
        <template scope="scope">
          {{scope.row.f_market_name}}
        </template>
      </el-table-column>
      <el-table-column align="center" label="币别" width="">
        <template scope="scope">
          <span>{{scope.row.f_currency_name}}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="数据更新方式"
        width="100">
        <template scope="scope">
          <span>{{scope.row.f_sync_type === 0 ? '自动' : '手动'}}</span>
        </template>
      </el-table-column>
      <el-table-column align="center" label="备注" width="">
        <template scope="scope">
          <span>{{scope.row.f_memo}}</span>
        </template>
      </el-table-column>
      <el-table-column align="center" label="清算负责人" width="">
        <template scope="scope">
          <span>{{scope.row.f_charge_user_name}}</span>
        </template>
      </el-table-column>
      <el-table-column align="center"  label="操作" width="150">
        <template scope="scope">                          
          <el-button size="mini" @click="table_edit2(scope.$index, scope.row)">编辑</el-button>
            <el-dialog title="市场信息1" :visible.sync="dialogFormVisible3">
              <el-form ref="market3" :model="market3" label-width="130px" :rules="rules">
                <el-form-item label="市场名称" :label-width="formLabelWidth" prop="f_market_name">
                  <el-input v-model="market3.f_market_name"></el-input>
                </el-form-item>
                <el-form-item label="所属公司" :label-width="formLabelWidth" prop="f_company_code">
                  <el-select v-model="market3.f_company_code" placeholder="请选择">
                    <el-option
                      v-for="item in company"
                      :key="item.company_code"
                      :label="item.name"
                      :value="item.company_code">
                    </el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="币别" :label-width="formLabelWidth" prop="f_currency_name">
                  <el-select v-model="market3.f_currency_name" placeholder="请选择">
                    <el-option
                      v-for="currecy in Cur"
                      :key="currecy.sign"
                      :label="currecy.name"
                      :value="currecy.name">
                    </el-option>
                  </el-select>
                  <el-select v-model="market3.f_currency_sign" placeholder="请选择">
                    <el-option
                      v-for="currecy in Cur"
                      :key="currecy.sign"
                      :label="currecy.sign"
                      :value="currecy.sign">
                    </el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="数据同步方式" :label-width="formLabelWidth" prop="f_sync_type">
                  <el-select v-model="market3.f_sync_type" placeholder="请选择">
                    <el-option
                      v-for="item in way"
                      :key="item.id"
                      :label="item.name"
                      :value="item.id">
                    </el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="备注" :label-width="formLabelWidth">
                  <el-input
                    type="textarea"
                    :rows="2"
                    placeholder="请输入内容"
                    v-model="market3.f_memo">
                  </el-input>
                </el-form-item>
                <el-form-item label="清算负责人" :label-width="formLabelWidth" prop="f_charge_user_name">
                  <el-select v-model="market3.f_charge_user_name" placeholder="请选择">
                    <el-option
                      v-for="item4 in charge"
                      :key="item4.contact_code"
                      :label="item4.user_name"
                      :value="item4.user_name">
                    </el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="清算负责人编号" :label-width="formLabelWidth" prop="f_charge_user_code">
                  <el-select v-model="market3.f_charge_user_code" placeholder="请选择">
                    <el-option
                      v-for="item4 in charge"
                      :key="item4.contact_code"
                      :label="item4.user_name"
                      :value="item4.user_code">
                    </el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="授权邮箱">
                  <el-button type="text" @click="table_market_create_authorize1()">添加</el-button><i class="el-icon-edit"></i>
                  <el-dialog title="邮箱列表" :visible.sync="dialogFormVisible4" append-to-body>
                    <el-form :model="market4">
                      <el-form-item label="邮箱选择" :label-width="formLabelWidth">
                        <el-select v-model="market4.f_authorized" placeholder="请选择">
                          <el-option
                            v-for="yx1 in authorize"
                            :key="yx1.user_code"
                            :label="yx1.user_email"
                            :value="yx1">
                          </el-option>
                        </el-select>
                      </el-form-item>
                    </el-form>
                    <div slot="footer" class="dialog-footer">
                      <el-button @click="dialogFormVisible4 = false">取 消</el-button>
                      <el-button type="primary" @click="table_market_create_authorize_submit1()">确 定</el-button>
                    </div>
                  </el-dialog>
              <el-table
                :data="authordata"
                border
                style="width:100%">
                <el-table-column
                  label="授权邮箱"
                  width="">
                  <template scope="scope">
                    <span>{{scope.row.user_email}}</span>
                  </template>
                </el-table-column>
                <el-table-column
                  label="邮箱拥有者"
                  width="">
                  <template scope="scope">
                    <span>{{scope.row.user_name}}</span>
                  </template>
                </el-table-column>
                <el-table-column
                  label="邮箱拥有者手机"
                  width="">
                  <template scope="scope">
                    <span>{{scope.row.user_phone}}</span>
                  </template>
                </el-table-column>
                <el-table-column align="center"  label="操作" width="">
                    <template scope="scope">                          
                      <el-button size="mini" @click="table_delete(scope.$index, scope.row)">删除</el-button>            
                    </template>
                </el-table-column>
              </el-table>
              </el-form-item>
              <el-form-item>
                <el-button type="primary" @click="table_market_update_submit('market3')">完成</el-button>
              </el-form-item>          
          </el-form>
          </el-dialog>
          <el-button size="mini" type="danger" @click="table_delete2(scope.$index, scope.row)">删除</el-button>           
        </template>
      </el-table-column>
    </el-table>
    <el-button type="primary" @click="market_create()">完成</el-button>
  </div>
</template>

<script>
  import http from '../../../../assets/js/http'
  import fomrMixin from '../../../../assets/js/form_com'
  export default {
    data() {
      return {
        id: '',
        isLoading: false,
        index: null,
        authordata: [],
        authordata1: [],
        dialogFormVisible2: false,
        dialogFormVisible3: false,
        dialogFormVisible4: false,
        dialogFormVisible: false,
        charge: [],
        market2: {
          f_authorized: ''
        },
        market3: '',
        market4: {
          f_authorized: ''
        },
        authorize: '',
        company: '',
        marketdata: [],
        way: [{
          id: '0',
          name: '自动'
        }, {
          id: '1',
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
          f_authorized_user_code: []
        },
        rules: {
          f_market_name: [
            { required: true, message: '请输入集团名称', trigger: 'blur' }
          ],
          f_company_code: [
            { required: true, message: '请选择公司', trigger: 'blur' }
          ],
          f_currency_name: [
            { required: true, message: '请选择币别', trigger: 'blur' }
          ],
          f_currency_sign: [
            { required: true, message: '请选择币别标识符', trigger: 'blur' }
          ],
          f_sync_type: [
            { required: true, message: '请选择更新方式', trigger: 'blur' }
          ],
          f_charge_user_name: [
            { required: true, message: '请选择负责人', trigger: 'blur' }
          ],
          f_charge_user_code: [
            { required: true, message: '请选择负责人编号', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      get_id() {
        this.market.f_group_code = this.$route.params.id
      },
      get_company_list() {
        this.apiGet('company/list/' + this.market.f_group_code).then((res) => {
          this.handelResponse(res, (data) => {
            this.company = res.data.list
          })
        })
      },
      get_charge() {
        this.apiGet('contact/getCharge/' + this.market.f_group_code).then((res) => {
          this.handelResponse(res, (data) => {
            this.charge = res.data.charge_list
            this.authorize = res.data.emails_list
          })
        })
      },
      market_init() {
        this.market = {
          f_group_code: '',
          f_company_code: '',
          f_market_name: '',
          f_currency_name: '',
          f_currency_sign: '',
          f_sync_type: '',
          f_memo: '',
          f_charge_user_name: '',
          f_authorized_user_code: []
        }
      },
      table_market_create() {
        this.market = {}
        this.authordata = []
        this.market_init()
        this.get_id()
        this.get_company_list()
        this.get_charge()
        this.dialogFormVisible = true
      },
      table_market_create_submit(market) {
        this.$refs[market].validate((valid) => {
          if (valid) {
            this.market.f_authorized_user_code = this.authordata
            this.marketdata.push(this.market)
            this.dialogFormVisible = false
            console.log(this.marketdata)
          }
        })
      },
      table_market_update_submit(market) {
        this.$refs[market].validate((valid) => {
          if (valid) {
            this.market3.f_authorized_user_code = this.authordata
            this.marketdata.splice(this.index, 1)
            this.marketdata.push(this.market3)
            this.dialogFormVisible3 = false
            console.log(this.marketdata)
          }
        })
      },
      authorize_init() {
        this.market2 = {
          f_authorized: ''
        }
      },
      authorize_init1() {
        this.market4 = {
          f_authorized: ''
        }
      },
      table_market_create_authorize() {
        this.market2 = {}
        this.authorize_init()
        this.dialogFormVisible2 = true
      },
      table_market_create_authorize1() {
        this.market4 = {}
        this.authorize_init1()
        this.dialogFormVisible4 = true
      },
      table_market_create_authorize_submit() {
        if (this.authordata.indexOf(this.market2.f_authorized) === -1) {
          this.authordata.push(this.market2.f_authorized)
          this.dialogFormVisible2 = false
        } else {
          alert('此邮箱已经授权')
        }
      },
      table_market_create_authorize_submit1() {
        if (this.authordata.length) {
          for (let i = 0; i < this.authordata.length; i++) {
            if (this.market4.f_authorized === this.authordata[i]) {
              alert('此邮箱已经授权')
            } else {
              this.authordata.push(this.market4.f_authorized)
              this.dialogFormVisible4 = false
            }
          }
        } else {
          this.authordata.push(this.market4.f_authorized)
          this.dialogFormVisible4 = false
        }
      },
      table_edit(index, row) {
        this.dialogFormVisible2 = true
        this.market2 = Object.assign({}, row)
        this.authordata.splice(index, 1)
      },
      table_delete(index) {
        this.authordata.splice(index, 1)
      },
      table_edit2(index, row) {
        this.dialogFormVisible3 = true
        this.market3 = Object.assign({}, row)
        this.index = index
        this.get_id()
      },
      table_delete2(index) {
        this.marketdata.splice(index, 1)
      },
      market_create() {
        this.apiPost('market/create', this.marketdata).then((res) => {
          this.handelResponse(res, (data) => {
            _g.toastMsg('success', '添加成功')
            this.$router.push({ name: 'usersList' })
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