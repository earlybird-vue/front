<template>
		<el-form ref="form" :model="form" :rules="rules" label-width="130px">
			<el-form-item label="集团信息">
          <div>
              <el-button type="text" @click="table_creat_small()">编辑</el-button><i class="el-icon-edit"></i>
              <el-dialog title="集团信息" :visible.sync="dialogFormVisible">
               <el-form ref="form" :model="list" label-width="100px" :visible.sync="dialogFormVisible" >
                  <el-form-item label="集团名称">
                    <el-input v-model="list.name"></el-input>
                  </el-form-item>
                 <el-form-item label="网站">
                    <el-input v-model="list.site"></el-input>
                  </el-form-item>
                 <el-form-item label="行业">
                    <el-select v-model="list.trade" placeholder="请选择">
                      <el-option
                        v-for="item in Industry"
                        :key="item.id"
                        :label="item.name"
                        :value="item.id">
                      </el-option>
                    </el-select>
                  </el-form-item>
                  <el-form-item label="国家">
                    <el-select v-model="list.country" placeholder="请选择">
                      <el-option
                        v-for="item2 in Country"
                        :key="item2.id"
                        :label="item2.name"
                        :value="item2.id">
                      </el-option>
                    </el-select>
                  </el-form-item>
                  <el-form-item label="总机">
                    <el-input v-model="list.phone"></el-input>
                  </el-form-item>
                  <el-form-item label="集团编号">
                    <el-input v-model="list.division"></el-input>
                  </el-form-item>
                  <el-form-item label="地址">
                    <el-input v-model="list.address"></el-input>
                  </el-form-item>
                 <el-form-item label="公司类型">
                    <el-select v-model="list.type" placeholder="请选择">
                      <el-option
                        v-for="item3 in Type"
                        :key="item3.id"
                        :label="item3.name"
                        :value="item3.id">
                      </el-option>
                    </el-select>
                  </el-form-item>
                  <el-form-item label="财年截至时间">
                    <el-input v-model="list.endtime"></el-input>
                  </el-form-item>
                  <el-form-item label="企业负责人">
                    <el-button type="text" @click="table_creat()">编辑</el-button><i class="el-icon-edit"></i>
                    <el-dialog title="企业负责人" :visible.sync="dialogFormVisible4" append-to-body>
                      <el-form :model="box1">
                        <el-form-item label="姓" :label-width="formLabelWidth">
                          <el-input v-model="box1.firstname" autocomplete="off"></el-input>
                        </el-form-item>
                        <el-form-item label="名" :label-width="formLabelWidth">
                          <el-input v-model="box1.lastname" autocomplete="off"></el-input>
                        </el-form-item>
                        <el-form-item label="职位" :label-width="formLabelWidth">
                          <el-input v-model="box1.title" autocomplete="off"></el-input>
                        </el-form-item>
                        <el-form-item label="电话" :label-width="formLabelWidth">
                          <el-input v-model="box1.phone" autocomplete="off"></el-input>
                        </el-form-item>
                        <el-form-item label="邮箱" :label-width="formLabelWidth">
                          <el-input v-model="box1.email" autocomplete="off"></el-input>
                        </el-form-item>
                        <el-form-item label="相关" :label-width="formLabelWidth">
                          <el-select v-model="box1.role" placeholder="请选择">
                            <el-option
                              v-for="item in contact1"
                              :key="item.id"
                              :label="item.name"
                              :value="item.id">
                            </el-option>
                          </el-select>
                        </el-form-item>
                      </el-form>
                      <div slot="footer" class="dialog-footer">
                        <el-button @click="dialogFormVisible4 = false">取 消</el-button>
                        <el-button type="primary" @click="table_create_submit()">确 定</el-button>
                      </div>
                    </el-dialog>
                  <el-table
                    :data="contact"
                    border
                    style="width:100%">
                    <el-table-column label="姓" width="">
                      <template scope="scope">
                        {{scope.row.firstname}}
                      </template>
                    </el-table-column>
                    <el-table-column label="名" width="">
                      <template scope="scope">
                        {{scope.row.lastname}}
                      </template>
                    </el-table-column>
                    <el-table-column label="职业" width="">
                      <template scope="scope">
                        <span>{{scope.row.title}}</span>
                      </template>
                    </el-table-column>
                    <el-table-column label="电话"  align="center" width="">
                      <template scope="scope">
                        {{scope.row.phone}}
                      </template>
                    </el-table-column>
                    <el-table-column align="center" label="邮箱" width="">
                      <template scope="scope">
                        <span>{{scope.row.email}}</span>
                      </template>
                    </el-table-column>
                    <el-table-column
                      label="相关"
                      width="100">
                      <template scope="scope">
                        <span>{{scope.row.role}}</span>
                      </template>
                    </el-table-column>
                    <el-table-column align="center"  label="操作" width="150">
                      <template scope="scope">                          
                        <el-button size="mini" @click="table_edit(scope.$index, scope.row)">编辑</el-button>
                        <el-button size="mini" type="danger" @click="table_delete(scope.$index, scope.row)">删除</el-button>           
                      </template>
                    </el-table-column>
                  </el-table>
                  </el-form-item>
                  <el-form-item label="年销售额">
                    <el-select v-model="list.scope1" placeholder="请选择">
                      <el-option
                        v-for="item4 in options4"
                        :key="item4.Id"
                        :label="item4.name"
                        :value="item4.Id">
                      </el-option>
                    </el-select>
                    <el-input v-model="list.other1" class="h-40 w-200"></el-input>
                  </el-form-item>
                  <el-form-item label="年采购额">
                    <el-select v-model="list.scope2" placeholder="请选择">
                      <el-option
                        v-for="item4 in options4"
                        :key="item4.Id"
                        :label="item4.name"
                        :value="item4.Id">
                      </el-option>
                    </el-select>
                    <el-input v-model="list.other2" class="h-40 w-200"></el-input>
                  </el-form-item>
                  <el-form-item label="现金流情况">
                    <el-select v-model="list.scope3" placeholder="请选择">
                      <el-option
                        v-for="item4 in options4"
                        :key="item4.Id"
                        :label="item4.name"
                        :value="item4.Id">
                      </el-option>
                    </el-select>
                    <el-input v-model="list.other3" class="h-40 w-200"></el-input>
                  </el-form-item>
                  <el-form-item>
                    <el-button type="primary" @click="add_table_small('list')">立即创建</el-button>
                    <el-button @click="dialogFormVisible = false">取消</el-button>
                  </el-form-item>
              </el-form>
              </el-dialog>
          </div>       
			</el-form-item>
			<el-form-item label="公司信息">
				<el-button type="text" @click="table_create_company()">编辑</el-button><i class="el-icon-edit"></i>
          <el-dialog title="公司信息" :visible.sync="dialogFormVisible2" append-to-body>
            <el-form :model="company1">
            <el-form-item label="公司中文名称" :label-width="formLabelWidth">
              <el-input v-model="company1.name" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="公司英文名称" :label-width="formLabelWidth">
              <el-input v-model="company1.enname" autocomplete="off"></el-input>
            </el-form-item>
          </el-form>
          <div slot="footer" class="dialog-footer">
            <el-button @click="dialogFormVisible2 = false">取 消</el-button>
            <el-button type="primary" @click="table_create_company_sumbit()">确 定</el-button>
          </div>
          </el-dialog>
        <el-table
          :data="company2"
          border
          style="width:100%">
          <el-table-column
            label="公司中文名称"
            width="">
            <template scope="scope">
              <span>{{scope.row.name}}</span>
            </template>
          </el-table-column>
          <el-table-column
            label="公司英文名称"
            width="">
            <template scope="scope">
              <span>{{scope.row.enname}}</span>
            </template>
          </el-table-column>
          <el-table-column align="center"  label="操作" width="">
              <template scope="scope">                          
                <el-button size="mini" @click="table_edit2(scope.$index, scope.row)">编辑</el-button>          
              </template>
          </el-table-column>
        </el-table>
			</el-form-item>
      <el-form-item label="市场信息">
				<el-button type="text" @click="get_charge()">编辑</el-button><i class="el-icon-edit"></i>
          <el-dialog title="市场信息" :visible.sync="dialogFormVisible3" append-to-body>
            <el-form :model="market">
            <el-form-item label="市场名称" :label-width="formLabelWidth">
              <el-input v-model="market.CompanyDivision"></el-input>
            </el-form-item>
            <el-form-item label="币别" :label-width="formLabelWidth">
                    <el-select v-model="market.CurrencyName " placeholder="请选择">
                      <el-option
                        v-for="currecy in Cur"
                        :key="currecy.sign"
                        :label="currecy.name"
                        :value="currecy.name">
                      </el-option>
                    </el-select>
                    <el-select v-model="market.CurrencySign " placeholder="请选择">
                      <el-option
                        v-for="currecy in Cur"
                        :key="currecy.sign"
                        :label="currecy.name"
                        :value="currecy.sign">
                      </el-option>
                    </el-select>
            </el-form-item>
            <el-form-item label="发票更新方式" :label-width="formLabelWidth">
                    <el-select v-model="market.SyncInvoiceType" placeholder="请选择">
                      <el-option
                        v-for="item4 in options6"
                        :key="item4.id"
                        :label="item4.name"
                        :value="item4.id">
                      </el-option>
                    </el-select>
            </el-form-item>
            <el-form-item label="发票更新日期" :label-width="formLabelWidth">
              <el-input v-model="market.SyncInvoiceDate"></el-input>
            </el-form-item>
            <el-form-item label="供应商更新时间" :label-width="formLabelWidth">
              <el-input v-model="market.SyncVendorDate"></el-input>
            </el-form-item>
            <el-form-item label="供应商更新方式" :label-width="formLabelWidth">
                    <el-select v-model="market.SyncVendorType" placeholder="请选择">
                      <el-option
                        v-for="item in options6"
                        :key="item.id"
                        :label="item.name"
                        :value="item.id">
                      </el-option>
                    </el-select>
            </el-form-item>
            <el-form-item label="企业对账日" :label-width="formLabelWidth">
              <el-input v-model="market.ReconciliationDate"></el-input>
            </el-form-item>
            <el-form-item label="备注" :label-width="formLabelWidth">
              <el-input
                type="textarea"
                :rows="2"
                placeholder="请输入内容"
                v-model="market.Memo">
              </el-input>
            </el-form-item>
            <el-form-item label="市场清算负责人" :label-width="formLabelWidth">
                    <el-select v-model="market.UserName" placeholder="请选择">
                      <el-option
                        v-for="item4 in marketData"
                        :key="item4.Uid"
                        :label="item4.UserName"
                        :value="item4.UserName">
                      </el-option>
                    </el-select>
            </el-form-item>
          </el-form>
          <div slot="footer" class="dialog-footer">
            <el-button @click="dialogFormVisible3 = false">取 消</el-button>
            <el-button type="primary" @click="market_save()">确 定</el-button>
          </div>
          </el-dialog>
        <el-table
          :data="market1"
          border
          style="width:100%">
          <el-table-column label="#Id" width="">
            <template scope="scope">
              {{scope.row.Id}}
            </template>
          </el-table-column>
          <el-table-column label="状态" width="">
            <template scope="scope">
              {{scope.row.status}}
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
              <el-button size="mini" @click="table_edit(scope.$index, scope.row)">编辑</el-button>
              <el-button size="mini" type="danger" @click="table_delete(scope.$index, scope.row)">删除</el-button>           
            </template>
          </el-table-column>
        </el-table>
			</el-form-item>
			<el-form-item>
				<el-button type="primary" @click="add('form')" :loading="isLoading">提交</el-button>
				<el-button @click="goback()">返回</el-button>
			</el-form-item>
		</el-form>

</template>
<style type="text/css">
	.form-checkbox:first-child{
		margin-left: 15px;
	}
</style>
<script>
  import http from '../../../../assets/js/http'
  import fomrMixin from '../../../../assets/js/form_com'

  export default {
    data() {
      return {
        dialogFormVisible1: false,
        dialogFormVisible2: false,
        dialogFormVisible3: false,
        dialogFormVisible4: false,
        isLoading: false,
        visible2: false,
        textarea: '',
        maketData: '',
        company3: '',
        options6: [{
          id: '0',
          name: '自动更新'
        }, {
          id: '1',
          name: '手动更新'
        }],
        market: {
          CompanyId: '',
          ComoanyDivision: '',
          CurrencySign: '',
          CurrencyName: '',
          UserId: '',
          UserName: '',
          ReconciliationDate: '',
          SyncVendorDate: '',
          SyncVendorType: '',
          SyncInvoiceDate: '',
          SyncInvoiceType: '',
          Memo: '',
          GroupId: ''
        },
        form: {
          username: '',
          password: '',
          realname: '',
          structure_id: null,
          remark: '',
          groups: [],
          name: '',
          region: '',
          date1: '',
          date2: '',
          delivery: false,
          type: [],
          resource: '',
          desc: ''
        },
        company1: {
          name: '',
          enname: '',
          groupid: ''
        },
        market1: [],
        company2: [],
        list: {
          name: '',
          site: '',
          trade: '',
          division: '',
          country: '',
          phone: '',
          adress: '',
          type: '',
          endtime: '',
          contact: [],
          other1: '',
          other2: '',
          other3: '',
          ext1: '',
          ext2: '',
          ext3: ''
        },
        companyId: '',
        list1: [],
        Industry: '',
        Country: '',
        Type: '',
        Cur: '',
        CurrencyName: '',
        box1: {
          firstname: '',
          lastname: '',
          title: '',
          phone: '',
          email: '',
          role: '',
          status: 1
        },
        editLoading: false,
        contact: [],
        company: [],
        formLabelWidth: '120px',
        dialogFormVisible: false
      }
    },
    methods: {
      init() {
        this.box1 = {
          firstname: '',
          lastname: '',
          title: '',
          phone: '',
          email: '',
          role: '',
          status: 1
        }
      },
      table_creat() {
        this.box1 = {}
        this.init()
        this.dialogFormVisible4 = true
      },
      table_create_submit(index) {
        this.contact.push(this.box1)
        this.list.contact = this.contact
        console.log(this.contact)
        this.dialogFormVisible4 = false
      },
      table_delete(index, row) {
        this.contact.splice(index, 1)
      },
      table_edit(index, row) {
        this.dialogFormVisible4 = true
        this.box1 = Object.assign({}, row)
        this.contact.splice(index, 1)
      },
      table_edit2(index, row) {
        this.apiGet('company/update', this.company2.Id).then((res) => {
          this.dialogFormVisible2 = true
          this.company1 = Object.assign({}, row)
          this.company2.splice(index, 1)
        })
      },
      small_init() {
        this.list = {
          name: '',
          site: '',
          trade: '',
          division: '',
          country: '',
          phone: '',
          address: '',
          type: '',
          endtime: '',
          other1: '',
          other2: '',
          other3: '',
          contact: ''
        }
      },
      table_creat_small() {
        this.list = {}
        this.small_init()
        this.dialogFormVisible = true
      },
      add_table_small(list) {
        this.isLoading = !this.isLoading
        this.apiPost('customer/create', this.list).then((res) => {
          console.log(this.list)
          this.handelResponse(res, (data) => {
            console.log('res = ', _g.j2s(res))
            _g.toastMsg('success', '添加成功')
            this.companyId = res.data.companyId
            console.log(this.companyId)
          }, () => {
            this.isLoading = !this.isLoading
          })
        })
      },
      company_init() {
        this.company1 = {
          name: '',
          enname: '',
          groupid: ''
        }
      },
      table_create_company() {
        this.company1 = {}
        this.company_init()
        this.company1.groupid = this.companyId
        this.dialogFormVisible2 = true
      },
      table_create_company_sumbit() {
        this.isLoading = !this.isLoading
        this.apiPost('company/create', this.company1).then((res) => {
          this.handelResponse(res, (data) => {
            _g.toastMsg('success', '添加成功')
            this.company2.push(res.data)
            this.company3 = res.data.id
          }, () => {
            this.isLoading = !this.isLoading
          })
        })
      },
      market_init() {
        this.market = {
          CompanyId: '',
          CompanyDivision: '',
          CurrencySign: '',
          CurrencyName: '',
          UserId: '',
          UserName: '',
          ReconciliationDate: '',
          SyncVendorDate: '',
          SyncVendorType: '',
          SyncInvoiceDate: '',
          SyncInvoiceType: '',
          Memo: '',
          GroupId: ''
        }
      },
      get_charge() {
        this.market = {}
        this.market_init()
        this.apiGet('market/getCharge/' + 'companyId/' + this.companyId).then((res) => {
          this.handelResponse(res, (data) => {
            _g.toastMsg('success', '开始编辑')
            this.marketData = res.data
            this.market.UserId = res.data.Uid
            this.market.CompanyId = this.company3
            this.market.GroupId = this.companyId
            console.log(this.market.CompanyId)
            this.dialogFormVisible3 = true
          })
        })
      },
      market_save() {
        this.isLoading = !this.isLoading
        this.apiPost('market/create', this.market).then((res) => {
          this.handelResponse(res, (data) => {
            _g.toastMsg('success', '添加成功')
            this.market1.push(res.data)
            console.log(this.market1)
          })
        })
      },
      getAllGroups() {
        this.apiGet('/customer/get').then((res) => {
          this.handelResponse(res, (data) => {
            this.Industry = data.industry
            this.Country = data.country
            this.Type = data.type
            this.options4 = data.ext
            this.Cur = data.cur
          })
        })
      }
    },
    created() {
      this.getAllGroups()
    },
    mixins: [http, fomrMixin]
  }
</script>