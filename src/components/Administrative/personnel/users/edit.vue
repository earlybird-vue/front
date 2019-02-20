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
    <div class="header">
      公司信息
      <el-button class="add" @click="customer_company_table()"><i class="el-icon-plus">添加</i></el-button></div>
      <el-dialog title="公司信息" :visible.sync="dialogFormVisible4"> 
        <el-form ref="form" :model="companyadd" label-width="130px">
          <el-form-item label="公司中文名称" :label-width="formLabelWidth">
            <el-input v-model="companyadd.f_name" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="公司英文名称" :label-width="formLabelWidth">
            <el-input v-model="companyadd.f_en_name" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="company_cancel()">取消</el-button>
            <el-button type="primary" @click="customer_company_create()">确定</el-button>
          </el-form-item>
        </el-form>
      </el-dialog>  
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
          {{scope.row.status === 1 ? '激活':"禁用"}}
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
    <div class="header">
      市场信息
      <el-button class="add" @click="table_market_create()"><i class="el-icon-plus">添加</i></el-button></div>
      <el-dialog title="市场信息" :visible.sync="dialogFormVisible5">
      <el-form ref="market" :model="market" :rules="rules" label-width="130px">
        <el-form-item label="市场名称" :label-width="formLabelWidth">
          <el-input v-model="market.f_market_name"></el-input>
        </el-form-item>
        <el-form-item label="所属公司" :label-width="formLabelWidth">
          <el-select v-model="market.f_company_code" placeholder="请选择">
            <el-option
              v-for="item in company"
              :key="item.company_code"
              :label="item.name"
              :value="item.company_code">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="币别" :label-width="formLabelWidth">
          <el-select v-model="market.f_currency_name" placeholder="请选择">
            <el-option
              v-for="currecy in Cur"
              :key="currecy.sign"
              :label="currecy.name"
              :value="currecy.name">
            </el-option>
          </el-select>
          <el-select v-model="market.f_currency_sign" placeholder="请选择">
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
        <el-form-item label="清算负责人" :label-width="formLabelWidth">
          <el-select v-model="market.f_charge_user_name" placeholder="请选择">
            <el-option
              v-for="item4 in charge"
              :key="item4.contact_code"
              :label="item4.user_name"
              :value="item4.user_name">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="清算负责人编号" :label-width="formLabelWidth">
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
          <el-button type="text" @click="table_market_create_authorize1()">添加</el-button><i class="el-icon-edit"></i>
            <el-dialog title="邮箱列表" :visible.sync="dialogFormVisible6" append-to-body>
              <el-form :model="market3">
                <el-form-item label="邮箱选择" :label-width="formLabelWidth">
                  <el-select v-model="market3.f_authorized" placeholder="请选择">
                    <el-option
                      v-for="yx in authorize"
                      :key="yx.user_code"
                      :label="yx.user_email"
                      :value="yx">
                    </el-option>
                  </el-select>
                </el-form-item>
              </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="dialogFormVisible2 = false">取 消</el-button>
          <el-button type="primary" @click="table_market_create_authorize_submit(yx)">确 定</el-button>
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
        <el-button type="primary" @click="customer_market_create()">完成</el-button>
      </el-form-item>
    </el-form>
    </el-dialog>
    <el-table
      :data="market1"
      border
      style="width:100%"
      :key="Math.random()">
      <el-table-column label="#Id" width="">
        <template scope="scope">
          {{scope.row.market_code}}
        </template>
      </el-table-column>
      <el-table-column label="状态" width="">
        <template scope="scope">
          {{scope.row.status === 0 ? '正常活跃':"不活跃"}}
        </template>
      </el-table-column>
      <el-table-column label="市场名称"  align="center" width="">
        <template scope="scope">
          {{scope.row.market_name}}
        </template>
      </el-table-column>
      <el-table-column align="center" label="币别" width="">
        <template scope="scope">
          <span>{{scope.row.currency_name}}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="更新方式"
        width="100">
        <template scope="scope">
          <span>{{scope.row.sync_type === 0 ? '自动' : '手动'}}</span>
        </template>
      </el-table-column>
      <el-table-column align="center" label="清算负责人" width="">
        <template scope="scope">
          <span>{{scope.row.charge_user_name}}</span>
        </template>
      </el-table-column>
      <el-table-column align="center"  label="操作" width="150">
        <template scope="scope">                          
          <el-button size="mini" @click="customer_market_edit(scope.$index, scope.row)">编辑</el-button>
            <el-dialog title="市场信息" :visible.sync="dialogFormVisible7">
              <el-form ref="market2" :model="market2" label-width="130px">
                <el-form-item label="市场名称" :label-width="formLabelWidth">
                  <el-input v-model="market2.market_name"></el-input>
                </el-form-item>
                <el-form-item label="所属公司" :label-width="formLabelWidth">
                  <el-select v-model="market2.company_code" placeholder="请选择">
                    <el-option
                      v-for="item in company"
                      :key="item.company_code"
                      :label="item.name"
                      :value="item.company_code">
                    </el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="币别" :label-width="formLabelWidth">
                  <el-select v-model="market2.currency_name" placeholder="请选择">
                    <el-option
                      v-for="currecy in Cur"
                      :key="currecy.sign"
                      :label="currecy.name"
                      :value="currecy.name">
                    </el-option>
                  </el-select>
                  <el-select v-model="market2.currency_sign" placeholder="请选择">
                    <el-option
                      v-for="currecy in Cur"
                      :key="currecy.sign"
                      :label="currecy.sign"
                      :value="currecy.sign">
                    </el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="数据同步方式" :label-width="formLabelWidth">
                  <el-select v-model="market2.sync_type" placeholder="请选择">
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
                    v-model="market2.memo">
                  </el-input>
                </el-form-item>
                <el-form-item label="清算负责人" :label-width="formLabelWidth">
                  <el-select v-model="market2.charge_user_name" placeholder="请选择">
                    <el-option
                      v-for="item4 in charge"
                      :key="item4.contact_code"
                      :label="item4.user_name"
                      :value="item4.user_name">
                    </el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="授权邮箱">
                  <el-button type="text" @click="table_market_create_authorize()">添加</el-button><i class="el-icon-edit"></i>
                  <el-dialog title="邮箱列表" :visible.sync="dialogFormVisible9" append-to-body>
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
                      <el-button @click="dialogFormVisible2 = false">取 消</el-button>
                      <el-button type="primary" @click="table_market_create_authorize_submit1()">确 定</el-button>
                    </div>
                  </el-dialog>
              <el-table
                :data="authordata1"
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
                      <el-button size="mini" @click="table_delete1(scope.$index, scope.row)">删除</el-button>            
                    </template>
                </el-table-column>
              </el-table>
              </el-form-item>
              <el-form-item>
                <el-button type="primary" @click="customer_market_update()">完成</el-button>
              </el-form-item>          
          </el-form>
          </el-dialog>           
        </template>
      </el-table-column>
    </el-table>
      <div class="header">
        联系信息
        <el-button class="add" @click="customer_contact_table()"><i class="el-icon-plus">添加</i></el-button></div>
        <el-dialog title="联系人信息" :visible.sync="dialogFormVisible10"> 
          <el-form ref="form" :model="contactadd" label-width="130px">
            <el-form-item label="姓" :label-width="formLabelWidth">
              <el-input v-model="contactadd.f_last_name" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="名" :label-width="formLabelWidth">
              <el-input v-model="contactadd.f_first_name" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="职位" :label-width="formLabelWidth">
              <el-input v-model="contactadd.f_user_position" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="电话" :label-width="formLabelWidth">
              <el-input v-model="contactadd.f_user_phone" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="邮箱" :label-width="formLabelWidth">
              <el-input v-model="contactadd.f_user_email" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="相关" :label-width="formLabelWidth">
              <el-checkbox-group v-model="contactadd.f_user_role" @change="handleCheckedRolesChange">
                <el-checkbox v-for="(item, index) in roles" :label="item.id" :key="index">{{item.name}}</el-checkbox>
              </el-checkbox-group>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="company_cancel()">取消</el-button>
              <el-button type="primary" @click="customer_contact_create()">确定</el-button>
            </el-form-item>
          </el-form>
        </el-dialog>  
      <el-table
        :data="contact"
        style="width: 100%"
        width="400">
        <el-table-column label="姓名" width="">
          <template scope="scope">
            {{scope.row.user_name}}
          </template>
        </el-table-column>
        <el-table-column label="职位" width="">
          <template scope="scope">
            {{scope.row.user_position}}
          </template>
        </el-table-column>
        <el-table-column label="电话" width="">
          <template scope="scope">
            {{scope.row.user_phone}}
          </template>
        </el-table-column>
        <el-table-column label="邮箱" width="">
          <template scope="scope">
            {{scope.row.user_email}}
          </template>
        </el-table-column>
        <el-table-column label="操作">
        <template scope="scope">
          <el-button type="text" @click="customer_contact_edit(scope.$index, scope.row)">修改</el-button><i class="el-icon-edit"></i>
          <el-dialog title="公司信息" :visible.sync="dialogFormVisible11">
            <el-form ref="form" :model="contact1" label-width="100px" :visible.sync="dialogFormVisible11" >
              <el-form-item label="姓">
                <el-input v-model="contact1.last_name"></el-input>
              </el-form-item>
              <el-form-item label="名">
                <el-input v-model="contact1.first_name"></el-input>
              </el-form-item>
              <el-form-item label="职位">
                <el-input v-model="contact1.user_position"></el-input>
              </el-form-item>
              <el-form-item label="电话">
                <el-input v-model="contact1.user_phone"></el-input>
              </el-form-item>
              <el-form-item label="邮箱">
                <el-input v-model="contact1.user_email"></el-input>
              </el-form-item>
              <el-form-item label="相关" :label-width="formLabelWidth">
                <el-checkbox-group v-model="contact1.user_roles" @change="handleCheckedRolesChange1">
                  <el-checkbox v-for="item in roles" :label="item.id" :key="item.id">{{item.name}}</el-checkbox>
                </el-checkbox-group>
              </el-form-item>
              <el-form-item>
                    <el-button type="primary" @click="customer_contact_update()">修改完成</el-button>
                    <el-button @click="dialogFormVisible11 = false">取消</el-button>
              </el-form-item>
            </el-form> 
          </el-dialog>        
        </template>
        </el-table-column>
      </el-table>
      <div class="header">
        财务信息
      </div>
      <el-table
        :data="finance"
        style="width: 100%"
        width="400">
        <el-table-column label="年销售额" width="">
          <template scope="scope">
            {{scope.row.sale_volume}}
          </template>
        </el-table-column>
        <el-table-column label="年采购额" width="">
          <template scope="scope">
            {{scope.row.purhchase_volume}}
          </template>
        </el-table-column>
        <el-table-column label="资金流状况" width="">
          <template scope="scope">
            {{scope.row.cashflow_volume}}
          </template>
        </el-table-column>
        <el-table-column label="状态" width="">
          <template scope="scope">
            {{scope.row.status === 1 ? '正常':"不正常"}}
          </template>
        </el-table-column>
        <el-table-column label="操作">
        <template scope="scope">
          <el-button type="text" @click="customer_finance_edit(scope.$index, scope.row)">修改</el-button><i class="el-icon-edit"></i>
          <el-dialog title="财务信息" :visible.sync="dialogFormVisible12">
          <el-form ref="form" :model="finance1" label-width="130px" :visible.sync="dialogFormVisible12">  
            <el-form-item label="年销售额">
              <el-select v-model="finance1.sale_volume_id" placeholder="请选择">
                <el-option
                  v-for="item4 in ext"
                  :key="item4.id"
                  :label="item4.name"
                  :value="item4.id">
                </el-option>
              </el-select>
              <el-input v-model="finance1.sale_volume" class="h-40 w-200"></el-input>
            </el-form-item>
            <el-form-item label="年采购额">
              <el-select v-model="finance1.purhchase_volume_id" placeholder="请选择">
                <el-option
                  v-for="item4 in ext"
                  :key="item4.id"
                  :label="item4.name"
                  :value="item4.id">
                </el-option>
              </el-select>
              <el-input v-model="finance1.purhchase_volume" class="h-40 w-200"></el-input>
            </el-form-item>
            <el-form-item label="现金流情况">
              <el-select v-model="finance1.cashflow_volume_id" placeholder="请选择">
                <el-option
                  v-for="item4 in ext"
                  :key="item4.id"
                  :label="item4.name"
                  :value="item4.id">
                </el-option>
              </el-select>
              <el-input v-model="finance1.cashflow_volume" class="h-40 w-200"></el-input>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="dialogFormVisible12 = false">取消</el-button>
              <el-button type="primary" @click="customer_finance_update()">修改</el-button>
    </el-form-item>
            </el-form> 
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
  import { compare } from 'semver'

  export default {
    inject: ['reload'],
    data() {
      return {
        isLoading: false,
        dialogFormVisible: false,
        dialogFormVisible4: false,
        dialogFormVisible5: false,
        dialogFormVisible6: false,
        dialogFormVisible7: false,
        dialogFormVisible9: false,
        dialogFormVisible2: false,
        dialogFormVisible3: false,
        dialogFormVisible10: false,
        dialogFormVisible11: false,
        dialogFormVisible12: false,
        contactadd: {
          f_first_name: '',
          f_last_name: '',
          f_user_position: '',
          f_user_phone: '',
          f_user_email: '',
          f_user_role: [],
          f_group_code: ''
        },
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
        companyadd: {
          f_name: '',
          f_en_name: '',
          f_group_code: ''
        },
        companydata: [],
        id: null,
        company_id: null,
        company1_id: null,
        customer: [],
        list: '',
        company: [],
        company2: {},
        company3: [],
        market5: {
          f_authorized_user_code: []
        },
        company1: {
          f_name: '',
          f_en_name: '',
          f_code: '',
          f_status: ''
        },
        market: {
          f_group_code: '',
          f_company_code: '',
          f_market_name: '',
          f_currency_name: '',
          f_currency_sign: '',
          f_sync_type: '',
          f_memo: '',
          f_charge_user_code: '',
          f_charge_user_name: '',
          f_authorized_user_code: []
        },
        way: [{
          id: 0,
          name: '自动'
        }, {
          id: 1,
          name: '手动'
        }],
        Cur: [{
          sign: '¥',
          name: 'RMB'
        }, {
          sign: '$',
          name: 'USD'
        }],
        market1: [],
        market_id: '',
        market2: '',
        charge: [],
        authordata: [],
        authordata1: [],
        marketdata: [],
        market3: {
          f_authorized: ''
        },
        market4: {
          f_authorized: ''
        },
        contact: [],
        contact_id: null,
        contact1: '',
        contact2: {},
        finance: [],
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
        finance1: '',
        finance2: {}
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
      get_simple_list() {
        this.apiGet('contact/simplelist/' + this.market2.group_code).then((res) => {
          this.handelResponse(res, (data) => {
            this.authorize = res.data
            console.log(this.authorize)
          })
        })
      },
      get_contact_list() {
        this.apiGet('contact/list/' + this.$route.params.id).then((res) => {
          this.handelResponse(res, (data) => {
            this.contact = res.data.list
          })
        })
      },
      get_fd_list() {
        this.apiGet('finance/get/' + this.$route.params.id).then((res) => {
          this.handelResponse(res, (data) => {
            this.finance = res.data
            this.finance = [this.finance]
            console.log(this.finance)
          })
        })
      },
      customer_save(index, row) {
        this.list = this.customer.info
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
            this.company_id = res.data.list[0].company_code
            this.customer_market_read()
          })
        })
      },
      company_init() {
        this.companyadd = {
          f_name: '',
          f_en_name: '',
          f_group_code: ''
        }
      },
      customer_company_table() {
        this.companyadd = {}
        this.company_init()
        this.companyadd.f_group_code = this.id
        this.dialogFormVisible4 = true
      },
      customer_company_cancel() {
        this.companyadd = {}
        this.dialogFormVisible4 = false
      },
      customer_company_create() {
        this.companydata.push(this.companyadd)
        this.apiPost('company/create', this.companydata).then((res) => {
          this.handelResponse(res, (data) => {
            _g.toastMsg('success', '添加成功')
            this.customer_company_read()
            console.log(this.company)
            this.dialogFormVisible4 = false
          })
        })
      },
      customer_company_edit(index, row) {
        this.apiGet('company/get/' + this.company_id).then((res) => {
          this.handelResponse(res, (data) => {
            this.company1 = res.data
            console.log(this.company1)
            this.dialogFormVisible2 = true
            this.company1 = Object.assign({}, row)
          })
        })
      },
      customer_company_update() {
        this.company2.f_name = this.company1.name
        this.company2.f_en_name = this.company1.en_name
        this.company2.f_code = this.company1.company_code
        this.company2.f_status = this.company1.status
        this.apiPost('company/update', this.company2).then((res) => {
          this.handelResponse(res, (data) => {
            _g.toastMsg('success', '修改成功')
            this.customer_company_read()
            this.dialogFormVisible2 = false
          })
        })
      },
      customer_market_read() {
        console.log(this.company_id)
        this.apiGet('market/list/' + this.company_id).then((res) => {
          this.handelResponse(res, (data) => {
            this.market1 = res.data.list
            this.customer_market_company_read()
          })
        })
      },
      customer_add_market_read() {
        this.apiGet('market/list/' + this.company1_id).then((res) => {
          this.handelResponse(res, (data) => {
            this.market1 = res.data.list
            this.customer_market_company_read()
          })
        })
      },
      customer_market_company_read() {
        this.apiGet('company/get/' + this.company_id).then((res) => {
          this.handelResponse(res, (data) => {
            for (let i = 0; i < this.market1.length; i++) {
              this.market1[i].company_name = res.data.name
            }
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
          f_charge_user_code: '',
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
        this.dialogFormVisible5 = true
      },
      authorize_init() {
        this.market3 = {
          f_authorized: ''
        }
      },
      authorize_init1() {
        this.market4 = {
          f_authorized: ''
        }
      },
      table_market_create_authorize1() {
        this.market3 = {}
        this.authorize_init()
        this.dialogFormVisible6 = true
      },
      table_market_create_authorize() {
        this.market4 = {}
        this.authorize_init1()
        this.dialogFormVisible9 = true
      },
      table_market_create_authorize_submit() {
        if (this.authordata.indexOf(this.market3.f_authorized) === -1) {
          this.authordata.push(this.market3.f_authorized)
          this.dialogFormVisible6 = false
        } else {
          alert('此邮箱已经授权')
        }
      },
      table_market_create_authorize_submit1() {
        for (let i = 0; i < this.authordata1.length; i++) {
          if (this.market4.f_authorized === this.authordata1[i]) {
            alert('此邮箱已经授权')
          } else {
            this.authordata1.push(this.market4.f_authorized)
            this.dialogFormVisible9 = false
          }
        }
      },
      table_edit(index, row) {
        this.dialogFormVisible6 = true
        this.market3 = Object.assign({}, row)
        this.authordata.splice(index, 1)
      },
      table_edit1(index, row) {
        this.dialogFormVisible9 = true
        this.market4 = Object.assign({}, row)
        this.authordata1.splice(index, 1)
      },
      table_delete(index) {
        this.authordata.splice(index, 1)
      },
      table_delete1(index) {
        this.authordata1.splice(index, 1)
      },
      customer_market_create() {
        for (let i = 0; i < this.authordata.length; i++) {
          this.market.f_authorized_user_code.push(this.authordata[i].user_code)
        }
        this.marketdata.push(this.market)
        this.company1_id = this.market.f_company_code
        this.apiPost('market/create', this.marketdata).then((res) => {
          this.handelResponse(res, (data) => {
            _g.toastMsg('success', '编辑成功')
            this.market1 = res.data
            this.dialogFormVisible5 = false
            this.authordata = []
          })
        })
      },
      customer_market_edit(index, row) {
        this.market_id = this.market1[index].market_code
        this.apiGet('market/get/' + this.market_id).then((res) => {
          this.handelResponse(res, (data) => {
            this.market2 = res.data
            this.get_simple_list()
            this.authordata1 = []
            for (let i = 0; i < res.data.emails_list.length; i++) {
              for (let j = 0; j <= i; j++) {
                console.log(res.data.emails_list[i].user_code)
                if (res.data.authorized_user_codes[j] === res.data.emails_list[i].user_code) {
                  this.authordata1.push(res.data.emails_list[i])
                }
              }
            }
          })
        })
        this.dialogFormVisible7 = true
        this.market2 = Object.assign({}, row)
      },
      customer_market_update() {
        this.isLoading = !this.isLoading
        this.market5.f_code = this.market2.market_code
        this.market5.f_group_code = this.market2.group_code
        this.market5.f_company_code = this.market2.company_code
        this.market5.f_market_name = this.market2.market_name
        this.market5.f_currency_name = this.market2.currency_name
        this.market5.f_currency_sign = this.market2.currency_sign
        this.market5.f_sync_type = this.market2.sync_type
        this.market5.f_memo = this.market2.memo
        this.market5.f_charge_user_name = this.market2.charge_user_name
        this.market5.f_charge_user_code = this.market2.charge_user_code
        this.market5.f_status = this.market2.status
        for (let i = 0; i < this.authordata1.length; i++) {
          this.market5.f_authorized_user_code.push(this.authordata1[i].user_code)
        }
        this.apiPost('market/update', this.market5).then((res) => {
          this.handelResponse(res, (data) => {
            _g.toastMsg('success', '修改成功')
            this.customer_market_read()
            this.dialogFormVisible7 = false
            this.authordata1 = []
          })
        })
      },
      handleCheckedRolesChange(value) {
        let checkcount = value.length
        this.checkAll = checkcount === this.roles.length
      },
      handleCheckedRolesChange1(value) {
        let checkcount = value.length - 1
        this.checkAll = checkcount === this.roles.length
      },
      contact_init() {
        this.contactadd = {
          f_first_name: '',
          f_last_name: '',
          f_user_position: '',
          f_user_phone: '',
          f_user_email: '',
          f_user_role: [],
          f_group_code: ''
        }
      },
      customer_contact_table() {
        this.contactadd = {}
        this.contact_init()
        this.dialogFormVisible10 = true
      },
      customer_contact_create() {
        this.contactadd.f_group_code = this.$route.params.id
        this.apiPost('contact/create', this.contactadd).then((res) => {
          this.handelResponse(res, (data) => {
            _g.toastMsg('success', '添加成功')
            this.dialogFormVisible10 = false
          })
        })
      },
      customer_contact_edit(index, row) {
        this.contact_id = this.contact[index].contact_code
        this.apiGet('contact/get/' + this.contact_id).then((res) => {
          this.handelResponse(res, (data) => {
            this.contact1 = res.data
            this.contact1.user_roles = []
            if (this.contact1.is_charge === 1) {
              this.contact1.user_roles.push('charge')
            }
            if (this.contact1.is_enterprise === 1) {
              this.contact1.user_roles.push('enterprise')
            }
            if (this.contact1.is_financial === 1) {
              this.contact1.user_roles.push('financial')
            }
          })
          console.log(this.contact1)
        })
        this.dialogFormVisible11 = true
        this.contact1 = Object.assign({}, row)
      },
      customer_contact_update() {
        this.isLoading = !this.isLoading
        this.contact2.f_code = this.contact1.contact_code
        this.contact2.f_last_name = this.contact1.last_name
        this.contact2.f_first_name = this.contact1.first_name
        this.contact2.f_user_position = this.contact1.user_position
        this.contact2.f_user_phone = this.contact1.user_phone
        this.contact2.f_user_email = this.contact1.user_email
        this.contact2.f_user_role = this.contact1.user_roles
        this.contact2.f_status = this.contact1.status
        this.apiPost('contact/update', this.contact2).then((res) => {
          this.handelResponse(res, (data) => {
            _g.toastMsg('success', '修改成功')
            this.dialogFormVisible11 = false
            this.get_contact_list()
          })
        })
      },
      customer_finance_edit(index, row) {
        this.apiGet('finance/get/' + this.finance[index].group_code).then((res) => {
          this.handelResponse(res, (data) => {
            this.finance1 = res.data
            this.dialogFormVisible12 = true
            this.finance1 = Object.assign({}, row)
          })
        })
      },
      customer_finance_update() {
        this.finance2.f_code = this.finance1.finance_code
        this.finance2.f_group_code = this.finance1.group_code
        this.finance2.f_sale_volume_id = this.finance1.sale_volume_id
        this.finance2.f_sale_volume = this.finance1.sale_volume
        this.finance2.f_purhchase_volume_id = this.finance1.purhchase_volume_id
        this.finance2.f_purhchase_volume = this.finance1.purhchase_volume
        this.finance2.f_cashflow_volume_id = this.finance1.cashflow_volume_id
        this.finance2.f_cashflow_volume = this.finance1.cashflow_volume
        this.finance2.status = this.finance1.status
        this.apiPost('finance/update', this.finance2).then((res) => {
          this.handelResponse(res, (data) => {
            _g.toastMsg('success', '修改成功')
            this.get_fd_list()
            this.dialogFormVisible12 = false
          })
        })
      }
    },
    created() {
      this.customer_read()
      this.customer_company_read()
      this.get_contact_list()
      this.get_fd_list()
    },
    mixins: [http, fomrMixin]
  }
</script>