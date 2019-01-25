<template>	
  <div>
    <div class="header">集团基础信息</div>
    <el-table
      :data="customer.info"
      style="width: 100%">
      <el-table-column type="expand">
        <template slot-scope="props">
          <el-form label-position="left" inline class="demo-table-expand">
            <el-form-item label="集团名称">
              <span>{{ props.row.name }}</span>
            </el-form-item>
            <el-form-item label="网站">
              <span>{{ props.row.web_site }}</span>
            </el-form-item>
            <el-form-item label="总机">
              <span>{{ props.row.contact_phone }}</span>
            </el-form-item>
            <el-form-item label="集团编号">
              <span>{{ props.row.group_number }}</span>
            </el-form-item>
            <el-form-item label="地址">
              <span>{{ props.row.address }}</span>
            </el-form-item>
            <el-form-item label="财年截止时间">
              <span>{{ props.row.fiscal_month }}</span>
            </el-form-item>
          </el-form>
        </template>
      </el-table-column>
      <el-table-column label="集团名称" prop="name">
      </el-table-column>
      <el-table-column
        label="集团编号"
        prop="group_number">
      </el-table-column>
      <el-table-column
        label="操作">
        <template slot-scope="scope">
          <el-button type="text" @click="customer_save(scope.$index, scope.row)">编辑</el-button><i class="el-icon-edit"></i>
          <el-dialog title="集团信息" :visible.sync="dialogFormVisible">
            <el-form ref="form" :model="list" label-width="100px" :visible.sync="dialogFormVisible" >
              <el-form-item label="集团名称">
                <el-input v-model="list.name"></el-input>
              </el-form-item>
              <el-form-item label="网站">
                <el-input v-model="list.web_site"></el-input>
              </el-form-item>
              <el-form-item label="行业">
                <el-select v-model="list.industry_id" placeholder="请选择">
                  <el-option
                    v-for="item in customer.industrys"
                    :key="item.id"
                    :label="item.name"
                    :value="item.id">
                  </el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="国家">
                <el-select v-model="list.country_id" placeholder="请选择">
                  <el-option
                    v-for="item2 in customer.countrys"
                    :key="item2.id"
                    :label="item2.name"
                    :value="item2.id">
                  </el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="总机">
                <el-input v-model="list.contact_phone"></el-input>
              </el-form-item>
              <el-form-item label="集团编号">
                <el-input v-model="list.group_number"></el-input>
              </el-form-item>
              <el-form-item label="地址">
                <el-input v-model="list.address"></el-input>
              </el-form-item>
              <el-form-item label="公司类型">
                <el-select v-model="list.type_id" placeholder="请选择">
                  <el-option
                    v-for="item3 in customer.types"
                    :key="item3.id"
                    :label="item3.name"
                    :value="item3.id">
                  </el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="财年截至时间">
                <el-input v-model="list.fiscal_month"></el-input>
              </el-form-item>
              <el-form-item>
                    <el-button type="primary" @click="customer_update()">修改完成</el-button>
                    <el-button @click="dialogFormVisible = false">取消</el-button>
              </el-form-item>
            </el-form> 
          </el-dialog>        
        </template>
      </el-table-column>
    </el-table>
    <div class="header">公司信息<el-button class="add"><i class="el-icon-plus">添加</i></el-button></div>
    <el-table
      :data="company"
      style="width: 100%"
      width="400">
      <el-table-column label="公司名称" width="">
        <template scope="scope">
          {{scope.row.name}}
        </template>
      </el-table-column>
      <el-table-column label="公司编号" width="">
        <template scope="scope">
          {{scope.row.company_code}}
        </template>
      </el-table-column>
      <el-table-column label="状态" width="">
        <template scope="scope">
          {{scope.row.status}}
        </template>
      </el-table-column>
      <el-table-column label="操作">
      <template scope="scope">
        <el-button type="text" @click="customer_company_edit(scope.$index, scope.row)">修改</el-button><i class="el-icon-edit"></i>
        <el-dialog title="公司信息" :visible.sync="dialogFormVisible2">
          <el-form ref="form" :model="company1" label-width="100px" :visible.sync="dialogFormVisible2" >
            <el-form-item label="公司中文名称">
              <el-input v-model="company1.name"></el-input>
            </el-form-item>
            <el-form-item label="公司英文名称">
              <el-input v-model="company1.en_name"></el-input>
            </el-form-item>
            <el-form-item>
                  <el-button type="primary" @click="customer_company_update()">修改完成</el-button>
                  <el-button @click="dialogFormVisible2 = false">取消</el-button>
            </el-form-item>
          </el-form> 
        </el-dialog>        
      </template>
      </el-table-column>
    </el-table>
    <div class="header">市场信息</div>
    <el-table
      :data="customer.market"
      border
      style="width:100%">
      <el-table-column label="#Id" width="">
        <template scope="scope">
          {{scope.row.Id}}
        </template>
      </el-table-column>
      <el-table-column label="状态" width="">
        <template scope="scope">
          {{scope.row.CompanyStatus}}
        </template>
      </el-table-column>
      <el-table-column label="所属公司" width="">
        <template scope="scope">
          <span>{{scope.row.CompanyDivision}}</span>
        </template>
      </el-table-column>
      <el-table-column label="市场名称"  align="center" width="">
        <template scope="scope">
          {{scope.row.CompanyDivision}}
        </template>
      </el-table-column>
      <el-table-column align="center" label="币别" width="">
        <template scope="scope">
          <span>{{scope.row.CurrencyName}}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="发票更新"
        width="100">
        <template scope="scope">
          <span>{{scope.row.SyncInvoiceType}}</span>
        </template>
      </el-table-column>
      <el-table-column align="center" label="供应商更新" width="">
        <template scope="scope">
          <span>{{scope.row.SyncVendorType}}</span>
        </template>
      </el-table-column>
      <el-table-column align="center" label="清算负责人" width="">
        <template scope="scope">
          <span>{{scope.row.UserName}}</span>
        </template>
      </el-table-column>
      <el-table-column align="center"  label="操作" width="150">
        <template scope="scope">                          
          <el-button size="mini" @click="customer_market_read(scope.$index, scope.row)">编辑</el-button>
            <el-dialog title="市场信息" :visible.sync="dialogFormVisible3" append-to-body>
              <el-form :model="market1">
              <el-form-item label="市场名称" :label-width="formLabelWidth">
                <el-input v-model="market1.CompanyDivision"></el-input>
              </el-form-item>
              <el-form-item label="币别" :label-width="formLabelWidth">
                      <el-select v-model="market1.CurrencyName " placeholder="请选择">
                        <el-option
                          v-for="currecy in customer.cur"
                          :key="currecy.sign"
                          :label="currecy.name"
                          :value="currecy.name">
                        </el-option>
                      </el-select>
                      <el-select v-model="market1.CurrencySign " placeholder="请选择">
                        <el-option
                          v-for="currecy in customer.cur"
                          :key="currecy.sign"
                          :label="currecy.sign"
                          :value="currecy.sign">
                        </el-option>
                      </el-select>
              </el-form-item>
              <el-form-item label="发票更新方式" :label-width="formLabelWidth">
                      <el-select v-model="market1.SyncInvoiceType" placeholder="请选择">
                        <el-option
                          v-for="item4 in options6"
                          :key="item4.id"
                          :label="item4.name"
                          :value="item4.id">
                        </el-option>
                      </el-select>
              </el-form-item>
              <el-form-item label="发票更新日期" :label-width="formLabelWidth">
                <el-input v-model="market1.SyncInvoiceDate"></el-input>
              </el-form-item>
              <el-form-item label="供应商更新时间" :label-width="formLabelWidth">
                <el-input v-model="market1.SyncVendorDate"></el-input>
              </el-form-item>
              <el-form-item label="供应商更新方式" :label-width="formLabelWidth">
                      <el-select v-model="market1.SyncVendorType" placeholder="请选择">
                        <el-option
                          v-for="item in options6"
                          :key="item.id"
                          :label="item.name"
                          :value="item.id">
                        </el-option>
                      </el-select>
              </el-form-item>
              <el-form-item label="企业对账日" :label-width="formLabelWidth">
                <el-input v-model="market1.ReconciliationDate"></el-input>
              </el-form-item>
              <el-form-item label="备注" :label-width="formLabelWidth">
                <el-input
                  type="textarea"
                  :rows="2"
                  placeholder="请输入内容"
                  v-model="market1.Memo">
                </el-input>
              </el-form-item>
              <el-form-item label="市场清算负责人" :label-width="formLabelWidth">
                      <el-select v-model="market1.UserName" placeholder="请选择">
                        <el-option
                          v-for="item4 in customer.charge"
                          :key="item4.Uid"
                          :label="item4.UserName"
                          :value="item4.UserName">
                        </el-option>
                      </el-select>
              </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
              <el-button @click="dialogFormVisible3 = false">取 消</el-button>
              <el-button type="primary" @click="customer_market_update()">确 定</el-button>
            </div>
            </el-dialog>           
        </template>
      </el-table-column>
    </el-table>
  </div>           
</template>
<style>
	.form-checkbox:first-child{
		margin-left: 15px;
	}
  .demo-table-expand {
    font-size: 0;
  }
  .demo-table-expand label {
    width: 200px;
    color: #99a9bf;
  }
  .demo-table-expand .el-form-item {
    margin-right: 0;
    margin-bottom: 0;
    width: 50%;
  }
  .header{
    color:aqua;
    font-size: 30px;
  }
  .add{
    margin-left: 900px;
    background-color: aqua;
  }
</style>
<script>
  import http from '../../../../assets/js/http'
  import fomrMixin from '../../../../assets/js/form_com'

  export default {
    data() {
      return {
        isLoading: false,
        dialogFormVisible: false,
        dialogFormVisible2: false,
        dialogFormVisible3: false,
        id: null,
        options6: [
          {
            id: 0,
            name: '自动更新'
          },
          {
            id: 1,
            name: '手动更新'
          }
        ],
        customer: [],
        list: '',
        company: [],
        company1: {
          f_code: '',
          f_name: '',
          f_en_name: '',
          f_status: ''
        },
        market1: {}
      }
    },
    methods: {
      customer_read() {
        this.id = this.$route.params.id
        console.log(this.id)
        this.apiGet('group/get/' + this.id).then((res) => {
          this.handelResponse(res, (data) => {
            console.log(res.data)
            this.customer = res.data
            this.customer.info = [this.customer.info]
            console.log(this.customer.group)
          })
        })
      },
      customer_save(index, row) {
        this.list = this.customer.group
        this.list = Object.assign({}, row)
        this.dialogFormVisible = true
      },
      customer_update() {
        this.isLoading = !this.isLoading
        this.apiPost('group/update', this.list).then((res) => {
          this.handelResponse(res, (data) => {
            _g.toastMsg('success', '修改成功')
            this.customer_read()
            this.dialogFormVisible = false
          })
        })
      },
      customer_company_read() {
        this.apiGet('company/list/' + this.id).then((res) => {
          this.handelResponse(res, (data) => {
            this.company = res.data.list
            console.log(this.company)
          })
        })
      },
      customer_company_edit(index, row) {
        this.company1.f_code = this.company.company_code
        this.company1.f_name = this.company.name
        this.company1.f_en_name = this.company.en_name
        this.company1.f_status = this.company.status
        console.log(this.company1)
        this.company1 = Object.assign({}, row)
        this.dialogFormVisible2 = true
      },
      customer_company_update() {
        this.isLoading = !this.isLoading
        this.apiPost('company/update', this.company1).then((res) => {
          this.handelResponse(res, (data) => {
            _g.toastMsg('success', '修改成功')
            this.customer_company_read()
            this.dialogFormVisible2 = false
          })
        })
      },
      customer_market_edit(index, row) {
        this.market1 = Object.assign({}, row)
        this.dialogFormVisible3 = true
      },
      customer_market_update() {
        this.isLoading = !this.isLoading
        this.apiPost('market/update', this.market1).then((res) => {
          this.handelResponse(res, (data) => {
            _g.toastMsg('success', '修改成功')
            this.customer_read()
            this.dialogFormVisible3 = false
          })
        })
      }
    },
    created() {
      this.customer_read()
      this.customer_company_read()
    },
    mixins: [http, fomrMixin]
  }
</script>