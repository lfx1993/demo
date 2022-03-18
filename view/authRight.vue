<template>
  <div class="container">
    <mt-field v-model="dataForm.confirmationTypeName" @click.native="popupSlot(1)" class="must" :readonly="true" label="确权类别" placeholder="请选择">
      <i class="iconfont icon-right"></i>
    </mt-field>
    <div v-if="this.dataForm.confirmationType == '1'" class="newWidth">
      <a @click="nextUrl2('checkcontract')" class="mint-cell mint-field must">
        <div class="mint-cell-wrapper">
          <div class="mint-cell-title">
            <span class="mint-cell-text">业务采购合同</span>
          </div>
          <div class="mint-cell-value">
            <div v-if="contractsubInfo && contractsubInfo.subContract" class="mint-field-core">
              <small class="gray">{{
                contractsubInfo.subContract.contractNum
              }}</small>
              <p>{{ contractsubInfo.subContract.name }}</p>
            </div>
            <div v-else class="mint-field-core">
              <input placeholder="请选择" type="text" readonly="readonly" class="mint-field-core" />
            </div>
            <div class="mint-field-other">
              <i class="iconfont icon-right"></i>
            </div>
          </div>
        </div>
      </a>
      <div v-if="contractsubInfo && contractsubInfo.subContract">
        <mt-field v-model="contractsubInfo.subContract.estimatedAmount" :disabled="true" :readonly="true" label="业务采购合同金额">
          元
        </mt-field>
        <mt-field v-model="contractsubInfo.subContract.supplierName" :disabled="true" :readonly="true" label="供应商名称">
          <i class="iconfont"></i>
        </mt-field>
        <mt-field v-model="contractsubInfo.subContract.saleName" :disabled="true" :readonly="true" label="销售负责人">
          <i class="iconfont"></i>
        </mt-field>
        <mt-field v-model="contractsubInfo.subContract.subContractType" :disabled="true" :readonly="true" label="业务采购合同类型">
          <i class="iconfont"></i>
        </mt-field>
        <mt-field v-model="contractsubStatus" :disabled="true" :readonly="true" label="业务采购合同状态">
          <i class="iconfont"></i>
        </mt-field>
        <div class="title">关联交付项目信息</div>
        <mt-field v-model="
            contractsubInfo.project && contractsubInfo.project.projectName
          " :disabled="true" :readonly="true" label="交付项目">
          <i class="iconfont"></i>
        </mt-field>
        <mt-field class="must" v-model="zbDataform.zbStatusname" @click.native="popupSlot(8)" :readonly="true" label="客户确权状态" placeholder="请选择">
          <i class="iconfont icon-right"></i>
        </mt-field>
        <div class="title">关联销售合同信息</div>
        <mt-field v-model="
            contractsubInfo.saleContract && contractsubInfo.saleContract.name
          " :disabled="true" :readonly="true" label="销售合同">
          <i class="iconfont"></i>
        </mt-field>
        <mt-field v-model="
            contractsubInfo.saleContract && contractsubInfo.saleContract.state
          " :disabled="true" :readonly="true" label="销售合同状态">
          <i class="iconfont"></i>
        </mt-field>
        <mt-field v-model="
            contractsubInfo.saleContract &&
            contractsubInfo.saleContract.receivedAmountRate
          " :disabled="true" :readonly="true" label="销售合同回款比例">
          <i class="iconfont"></i>
        </mt-field>
      </div>

      <div class="title">业务采购合同确权文件信息</div>
      <a class="mint-cell mint-field must">
        <div class="mint-cell-wrapper">
          <div class="mint-cell-title" style="width: 140px">
            <span class="mint-cell-text">关联付款计划</span>
          </div>
          <div class="mint-cell-value">
            <input @click="popupSlot(10)" v-model="zbDataform.planName" type="text" readonly="true" placeholder="请选择" class="mint-field-core" />
            <div class="mint-field-other">
              <i class="iconfont icon-right"></i>
            </div>
          </div>
        </div>
      </a>
      <mt-field v-model="fukuanInfo.collePayForm" :disabled="true" :readonly="true" label="付款形式">
        <i class="iconfont"></i>
      </mt-field>
      <mt-field v-model="fukuanInfo.amount" :disabled="true" :readonly="true" label="计划付款金额">
        元
      </mt-field>
      <mt-field v-model="zbDataform.confirmFileTypeName" class="must" @click.native="popupSlot(4)" :readonly="true" label="确权文件类型" placeholder="请选择">
        <i class="iconfont icon-right"></i>
      </mt-field>
      <mt-field @blur.native.capture="
          zbDataform.amount = $get_thousand_num(zbDataform.amount)
        " ; v-model.trim="zbDataform.amount" class="must" placeholder="请输入" label="业务采购合同确权金额">
        元
      </mt-field>
      <mt-field v-if="zbDataform.confirmFileTypeName == '工作量确认单'" v-model.trim="zbDataform.confirmWorkload" class="must" placeholder="请输入" label="业务采购合同确权工作量">
        人月
      </mt-field>
      <mt-field v-model="zbDataform.yzTypename" class="must" @click.native="popupSlot(9)" :readonly="true" label="印章类型" placeholder="请选择">
        <i class="iconfont icon-right"></i>
      </mt-field>
      <mt-cell class="bordernone must dangernew" title="上传确权文件">
        <span class="danger">（当合同类型为人月类、其他时，需上传第三方人员的考勤打卡记录、工作过程/成果文档、验收报告。）</span>
      </mt-cell>
      <div class="item fileBox">
        <span v-for="(val, i) in zbDataform.coFiles" :key="i">
          {{ val.fileName }}
          <i @click="deleteFile(i, 3, '')" class="iconfont icon-close"></i>
        </span>
        <span class="baseColor">+上传<input @change="fileChange($event, 3, '')" type="file" accept="" multiple /></span>
      </div>
      <mt-cell class="bordernone must" title="申请业务采购合同确权理由"></mt-cell>
      <mt-field v-model.trim="zbDataform.subConfirmReason" placeholder="请描述销售合同的回款进度，项目的进展阶段及确权状态" :attr="{ maxlength: 2000 }" type="textarea" rows="4"></mt-field>
      <div>
        <mt-cell class="must bordernone" title="审批人"></mt-cell>
        <div class="item">
          <ul class="flexbox">
            <li v-for="(item, index) in approverList" class="flex-1 circle exam" :key="index">
              <img v-if="item.avatar" class="img-block" :src="item.avatar" />
              <img v-else class="img-block" src="../assets/img/userImg.png" />
              <span class="next" v-if="index != approverList.length - 1"></span>
              <p>{{ item.memberName }}</p>
            </li>
          </ul>
        </div>
        <a class="mint-cell bordernone">
          <div class="mint-cell-wrapper">
            <div class="mint-cell-title">
              <span class="mint-cell-text">抄送人</span>
              <small class="gray">相关项目经理、销售、QA等人员</small>
            </div>
          </div>
        </a>
        <div class="item bordernone">
          <ul class="flexbox">
            <li v-for="(item, index) in noticeList" class="flex-1 circle" :key="index">
              <img v-if="item.avatar" class="img-block" :src="item.avatar" />
              <img v-else class="img-block" src="../assets/img/userImg.png" />
              <i v-if="item.showflag != ''" @click="deleteUser(index, 3)" class="iconfont icon-close"></i>
              <p>{{ item.memberName }}</p>
            </li>
            <li @click="checkUser(3)" class="flex-1 circle">
              <div class="img-block">+</div>
            </li>
          </ul>
        </div>
        <div class="text-center gray tip">
          <small>如有疑问请联系项目管理部</small>
        </div>
        <div class="btn">
          <mt-button @click="
              dataForm.confirmationType == '0' ? submit() : subcontractSubmit()
            " type="primary" size="large">提交
          </mt-button>
        </div>
      </div>
    </div>
    <div v-else>
      <!--
        <a @click="nextUrl('checkproject',1)" class="mint-cell mint-field must">
          <div class="mint-cell-wrapper">
            <div class="mint-cell-title">
              <span class="mint-cell-text">项目</span>
            </div>
            <div class="mint-cell-value">
              <div v-if="projectInfo" class="mint-field-core">
                <small class="gray">{{projectInfo.projectNum}}</small>
                <p>{{projectInfo.projectName}}</p>
              </div>
              <div v-else class="mint-field-core">
                <input placeholder="请选择" type="text" readonly="readonly" class="mint-field-core">
              </div>
              <div class="mint-field-other">
                <i class="iconfont icon-right"></i>
              </div>
            </div>
          </div>
        </a>
      -->
      <a @click="nextUrl('salescontract', 4)" class="mint-cell mint-field must">
        <div class="mint-cell-wrapper">
          <div class="mint-cell-title">
            <span class="mint-cell-text">销售合同</span>
          </div>
          <div class="mint-cell-value">
            <div v-if="newSalesContractInfo" class="mint-field-core">
              <small class="gray">{{ newSalesContractInfo.contractNum }}</small>
              <p>{{ newSalesContractInfo.name }}</p>
            </div>
            <div v-else class="mint-field-core">
              <input placeholder="请选择" type="text" readonly="readonly" class="mint-field-core" />
            </div>
            <div class="mint-field-other">
              <i class="iconfont icon-right"></i>
            </div>
          </div>
        </div>
      </a>

      <!--
        <a v-if="orderList && orderList.length >0" @click="nextUrl('checkorder',2)" class="mint-cell mint-field must">
                                                                   <div class="mint-cell-wrapper">
                                                                     <div class="mint-cell-title">
                                                                       <span class="mint-cell-text">关联订单</span>
                                                                     </div>
                                                                     <div class="mint-cell-value">
                                                                       <div v-if="dataForm.orderId" class="mint-field-core">
                                                                         <small class="gray">{{dataForm.orderNum}}</small>
                                                                         <p>{{dataForm.orderName}}</p>
                                                                       </div>
                                                                       <div v-else class="mint-field-core">
                                                                         <input placeholder="请选择" type="text" readonly="readonly" class="mint-field-core">
                                                                       </div>
                                                                       <div class="mint-field-other">
                                                                         <i class="iconfont icon-right"></i>
                                                                       </div>
                                                                     </div>
                                                                   </div>
                                                                 </a>
      -->
      <!--
        <mt-field v-if="projectInfo" v-model="projectInfo.projectManagerName" :disabled="true" :readonly="true" label="项目经理">
          <i class="iconfont"></i>
        </mt-field>
        <mt-field v-else :disabled="true" :readonly="true" label="项目经理">
          <i class="iconfont"></i>
        </mt-field>
      -->
      <mt-field v-if="newSalesContractInfo" v-model="newSalesContractInfo.headOfSalesName" :disabled="true" :readonly="true" label="销售负责人">
        <i class="iconfont"></i>
      </mt-field>
      <mt-field v-else :disabled="true" :readonly="true" label="销售负责人">
        <i class="iconfont"></i>
      </mt-field>

      <!-- 客户确权 + 内部确权 -->
      <div v-if="saleContractInfo && dataForm.confirmationType == 0">
        <!--
          <div class="title">销售合同信息</div>
          <a class="mint-cell mint-field">
            <div class="mint-cell-wrapper">
              <div class="mint-cell-title">
                <span class="mint-cell-text">销售合同</span>
              </div>
              <div class="mint-cell-value">
                <div class="mint-field-core">
                  <small class="gray">{{saleContractInfo.contractNum}}</small>
                  <p>{{saleContractInfo.name}}</p>
                </div>
                <div class="mint-field-other">
                  <i class="iconfont"></i>
                </div>
              </div>
            </div>
          </a>
        -->
        <!-- 隐藏销售合同部分字段 -->
        <div v-if="false">
          <mt-field v-model="dataForm.subContractTypeName" :disabled="true" :readonly="true" label="核算类型">
            <i class="iconfont"></i>
          </mt-field>
          <mt-field v-model="dataForm.contractStatusName" :disabled="true" :readonly="true" label="合同状态">
            <i class="iconfont"></i>
          </mt-field>
          <mt-field v-model="projectInfo.bankName" :disabled="true" :readonly="true" label="签约客户">
            <i class="iconfont"></i>
          </mt-field>
        </div>

        <!-- 客户确权   真实子合同信息 -->
        <div v-if="realSubContractList && realSubContractList.length > 0">
          <div class="title">真实子合同信息</div>
          <a @click="nextUrl('realSubContract', 3)" class="mint-cell mint-field must">
            <div class="mint-cell-wrapper">
              <div class="mint-cell-title">
                <span class="mint-cell-text">关联真实子合同</span>
              </div>
              <div class="mint-cell-value">
                <div v-if="realSubContractInfo" class="mint-field-core">
                  <small class="gray">{{
                    realSubContractInfo.contractNum
                  }}</small>
                  <p>{{ realSubContractInfo.contractName }}</p>
                </div>
                <div v-else class="mint-field-core">
                  <input placeholder="请选择" type="text" readonly="readonly" class="mint-field-core" />
                </div>
                <div class="mint-field-other">
                  <i class="iconfont icon-right"></i>
                </div>
              </div>
            </div>
          </a>
          <mt-field v-model="realSubContractInfo.subContractTypeName" :disabled="true" :readonly="true" label="核算类型">
            <i class="iconfont"></i>
          </mt-field>
          <mt-field v-model="realSubContractInfo.contractState" :disabled="true" :readonly="true" label="合同状态">
            <i class="iconfont"></i>
          </mt-field>
          <mt-field v-model="realSubContractInfo.bankName" :disabled="true" :readonly="true" label="签约客户">
            <i class="iconfont"></i>
          </mt-field>
          <mt-field v-model="realSubContractInfo.contractAmo" :disabled="true" :readonly="true" label="合同备案金额">
            <i class="iconfont"></i>
          </mt-field>
          <mt-field v-model="realSubContractInfo.salesDirectorUser" :disabled="true" :readonly="true" label="销售负责人">
            <i class="iconfont"></i>
          </mt-field>
        </div>
        <div v-if="planTypeSlot[0].values.length > 0">
          <div>
            <!-- 确权信息start -->
            <div class="title">
              确权信息
              <span v-if="dataForm.confirmationType == '0'" class="danger">（客户确权文件必须为通过了行方规定的验收流程，且实施类项目要求提交终验报告，否则视为无效确权）</span>
            </div>
            <a class="mint-cell mint-field must">
              <div class="mint-cell-wrapper">
                <div class="mint-cell-title" style="width: 140px">
                  <span class="mint-cell-text">确权对应的确权计划</span>
                </div>
                <div class="mint-cell-value">
                  <input @click="popupSlot(2)" v-model="dataForm.planName" type="text" readonly="true" placeholder="请选择" class="mint-field-core" />
                  <div class="mint-field-other">
                    <i class="iconfont icon-right"></i>
                  </div>
                </div>
              </div>
            </a>
            <datetime @on-clear="dataForm.startCycle = null" clear-text="清空" :max-year="3000" :disabled="hasStartDateCycle" :readonly="hasStartDateCycle" v-model="dataForm.startCycle" @on-confirm="confirmDate" class="bordernone" format="YYYY-MM" placeholder="请选择" title="对应开始周期"></datetime>
            <datetime @on-clear="dataForm.endCycle = null" clear-text="清空" :max-year="3000" v-if="!hasEndDateCycle" v-model="dataForm.endCycle" :startDate="startDateCycle" class="bordernone" format="YYYY-MM" placeholder="请选择" title="对应结束周期"></datetime>

            <mt-field v-else v-model="dataForm.endCycle" :disabled="true" class="bordernone" placeholder="请选择" label="对应结束周期">
              <i class="iconfont"></i>
            </mt-field>
            <div class="danger tip-small">
              (即:成本发生月，本次选择的月份，下次不可再选。人月类/派单类/维保类填写)
            </div>

            <mt-field v-model="dataForm.confirmFileTypeName" class="must" @click.native="popupSlot(4)" :readonly="true" label="确权文件类型" placeholder="请选择">
              <i class="iconfont icon-right"></i>
            </mt-field>
            <mt-field v-if="dataForm.confirmationType == '0'" v-model="dataForm.confirmcustomersTypeName" class="must" @click.native="popupSlot(7)" :readonly="true" label="客户确权方式" placeholder="请选择">
              <i class="iconfont icon-right"></i>
            </mt-field>
            <div v-if="
                dataForm.confirmcustomersType ||
                dataForm.confirmcustomersType == '0'
              ">
              <mt-cell class="bordernone must" title="备注" v-if="dataForm.confirmcustomersType == '3'"></mt-cell>
              <mt-field v-model.trim="dataForm.comment" v-if="dataForm.confirmcustomersType == '3'" placeholder="例：仅有客户签字，仅有邮箱确认等" :attr="{ maxlength: 2000 }" type="textarea" rows="4"></mt-field>
              <a class="mint-cell mint-field">
                <div class="mint-cell-wrapper">
                  <div class="mint-cell-title" style="width: 100%">
                    <span v-if="dataForm.confirmcustomersType == '3'" class="mint-cell-text">客户确权合规性：不合规</span>
                    <span v-if="dataForm.confirmcustomersType != '3'" class="mint-cell-text">客户确权合规性：合规</span>
                  </div>
                </div>
              </a>
            </div>
            <datetime v-model="dataForm.yearResult" class="bordernone must" format="YYYY" placeholder="请选择" title="所属业绩年"></datetime>
            <!--
              隐藏 <mt-field class="must" label="类型名称" placeholder="请输入"><i class="iconfont"></i></mt-field>
            -->
            <mt-field v-if="dataForm.planName && dataForm.planName != '无'" v-model="dataForm.planAmount" :disabled="true" label="计划总金额" placeholder="">元</mt-field>
            <!-- 确权信息end -->
            <div v-if="preSplitList.length > 0">
              <div v-if="preConfirmInfo" class="title">内部确权</div>
              <mt-field v-if="preConfirmInfo" v-model="preConfirmInfo.amount" disabled="true" label="预计确权总金额">元
              </mt-field>
              <mt-field v-if="preConfirmInfo" v-model="preConfirmInfo.workload" disabled="true" label="预计总确认工作量">人月
              </mt-field>
              <!-- <div class="title">预计项目收入拆分</div> -->
              <!--
                <a v-for="(item, index) in preSplitList">
                  <div class="title">预计项目拆分({{index+1}})</div>
                  <a class="mint-cell mint-field must">
                    <div class="mint-cell-wrapper">
                      <div class="mint-cell-title">
                        <span class="mint-cell-text">项目</span>
                      </div>
                      <div class="mint-cell-value">
                        <div v-if="item.projectName" class="mint-field-core">
                          <small class="gray">{{item.projectNum}}</small>
                          <p>{{item.projectName}}</p>
                        </div>
                      </div>
                    </div>
                  </a>
                  <mt-field :disabled="true" v-model="item.projectAmount" label="预计项目收入">元</mt-field>
                  <mt-field v-model="item.confirmWorkload" :disabled="true" label="预计确认工作量">人月</mt-field>
                  人月类、派单类显示 预计项目收入/预计确认工作量
                  <mt-field :disabled="true" v-if="item.avgSalary" v-model="item.avgSalary" label="预计人均月收入">元</mt-field>
                </a>
              -->
            </div>
            <div class="gray tip-small"></div>
            <!-- 客户确权添加订单信息 -->
            <div class="title">
              订单信息
              <span class="danger">（请先选择确权文件类型后再勾选订单）</span>
            </div>
            <div class="bgwhite">
              <div v-if="dataForm.confirmFileTypeName">
                <mt-button type="primary" size="small" @click="addWork" style="margin: 10px 0">+添加订单信息</mt-button>
                <span class="danger" style="margin-left: 10px">注：订单一旦客户确权后，将不能在该订单下进行报工，请谨慎选择</span>
              </div>
              <div v-if="
                  checkworkList &&
                  checkworkList.length > 0 &&
                  this.dataForm.subContractTypeName == '人月类'
                ">
                <div>
                  <el-row class="workLoadstyle" style="height: 55px; line-height: 20px; padding: 10px">
                    <el-col :span="3"> --- </el-col>
                    <el-col :span="3"> 客户确权总工作量（人月） </el-col>
                    <el-col :span="3"> 客户确权总收入（元） </el-col>
                    <el-col :span="3"> 订单数量 </el-col>
                    <el-col :span="3">
                      订单发放 <span class="baseColor">a</span>
                    </el-col>
                    <el-col :span="3">
                      订单发放 <span class="danger">b</span>
                    </el-col>
                    <el-col :span="3"> 订单计划确权金额（元） </el-col>
                    <el-col :span="3"> 操作 </el-col>
                  </el-row>
                  <el-row class="workLoadstylesecond borderbottom">
                    <el-col :span="3" class="workLoadstyle"> 订单信息 </el-col>
                    <el-col :span="3" class="borderright">
                      {{
                        WorkOrderData[0].workOrderConfirmPerson
                          ? WorkOrderData[0].workOrderConfirmPerson
                          : 0
                      }}
                    </el-col>
                    <el-col :span="3" class="borderright">
                      {{
                        WorkOrderData[0].workOrderConfirmMoney
                          ? $get_thousand_num(
                              WorkOrderData[0].workOrderConfirmMoney
                            )
                          : 0
                      }}
                    </el-col>
                    <el-col :span="3" class="borderright">
                      {{ checkworkList.length ? checkworkList.length : 0 }}
                    </el-col>
                    <el-col :span="3" class="borderright">
                      {{
                        WorkOrderData[0].planAvalue
                          ? WorkOrderData[0].planAvalue
                          : 0
                      }}
                    </el-col>
                    <el-col :span="3" class="borderright">
                      {{
                        WorkOrderData[0].planBvalue
                          ? WorkOrderData[0].planBvalue
                          : 0
                      }}
                    </el-col>
                    <el-col :span="3" class="borderright">
                      {{
                        WorkOrderData[0].cusExpectMoney
                          ? $get_thousand_num(WorkOrderData[0].cusExpectMoney)
                          : 0
                      }}
                    </el-col>
                    <el-col :span="3">
                      <el-button type="text" @click="addWork">查看详情</el-button>
                    </el-col>
                  </el-row>
                  <el-row class="
                      workLoadstylesecond
                      bordertop
                      borderbottom
                      borderstyle
                    ">
                    <el-col :span="3" class="workLoadstyle">
                      导入工作量明细
                    </el-col>
                    <el-col :span="3" class="borderright">
                      {{ workDetailPerson ? workDetailPerson : "--" }}
                    </el-col>
                    <el-col :span="3" class="borderright">
                      {{ workDetailIncome ? workDetailIncome : "--" }}
                    </el-col>
                    <el-col :span="3" class="borderright"> -- </el-col>
                    <el-col :span="3" class="borderright"> -- </el-col>
                    <el-col :span="3" class="borderright"> -- </el-col>
                    <el-col :span="3" class="borderright"> -- </el-col>
                    <el-col :span="3"> -- </el-col>
                  </el-row>
                  <el-row class="workLoadstylesecond borderbottom">
                    <el-col :span="3" class="workLoadstyle">偏差 </el-col>
                    <el-col :span="3" class="borderright danger">
                      {{
                        workDetailPerson &&
                        WorkOrderData[0].workOrderConfirmPerson
                          ? Math.abs(
                              (
                                WorkOrderData[0].workOrderConfirmPerson -
                                workDetailPerson
                              ).toFixed(2)
                            )
                          : "--"
                      }}
                    </el-col>
                    <el-col :span="3" class="borderright danger">
                      {{
                        WorkOrderData[0].workOrderConfirmMoney &&
                        workDetailIncome
                          ? Math.abs(
                              (
                                WorkOrderData[0].workOrderConfirmMoney -
                                workDetailIncome
                              ).toFixed(2)
                            )
                          : "--"
                      }}
                    </el-col>
                    <el-col :span="3" class="borderright"> -- </el-col>
                    <el-col :span="3" class="borderright"> -- </el-col>
                    <el-col :span="3" class="borderright"> -- </el-col>
                    <el-col :span="3" class="borderright"> -- </el-col>
                    <el-col :span="3"> -- </el-col>
                  </el-row>
                </div>
                <div v-if="
                    WorkOrderData[0].workOrderConfirmMoney &&
                    workDetailIncome &&
                    WorkOrderData[0].workOrderConfirmMoney != workDetailIncome
                  ">
                  <mt-cell class="bordernone must" title="偏差原因说明"></mt-cell>
                  <mt-field v-model.trim="deviationReason" class="borderBottom10" placeholder="请输入,最多300个字" :attr="{ maxlength: 300 }" type="textarea" rows="4"></mt-field>
                </div>
                <div class="btn">
                  <mt-button @click="exportWorkConfirm" type="primary" class="exportbtn" size="large">
                    导出工作量明细表
                  </mt-button>
                  <input style="display: none" type="file" ref="fileBtn" @change="getFile($event)" accept="*" />
                  <mt-button @click="getbtn" type="primary" class="exportbtn" size="large">导入工作量明细表</mt-button>
                  <span class="danger">可上传新的工作量明细信息，覆盖原有信息</span>
                </div>
              </div>
              <el-table v-if="
                  checkworkList &&
                  checkworkList.length > 0 &&
                  this.dataForm.subContractTypeName != '人月类'
                " :data="WorkOrderData" :header-cell-style="{ background: '#f2f2f2' }" style="width: 100%; margin: 10px 0">
                <el-table-column align="center" label="订单数量">
                  <template slot-scope="scope">
                    <span>{{ checkworkList.length }}</span>
                  </template>
                </el-table-column>
                <el-table-column prop="planAvalue" align="center">
                  <template slot="header" slot-scope="scope">
                    订单发放 <span class="baseColor">a</span>
                  </template>
                </el-table-column>
                <el-table-column align="center" prop="planBvalue">
                  <template slot="header" slot-scope="scope">
                    订单发放 <span class="danger">b</span>
                  </template>
                </el-table-column>
                <el-table-column align="center" prop="cusExpectMoney" label="订单计划确权金额（元）">
                  <template slot-scope="scope">
                    <span>{{
                      $get_thousand_num(scope.row.cusExpectMoney)
                    }}</span>
                  </template>
                </el-table-column>
                <el-table-column align="center" prop="workOrderConfirmPerson" label="客户确权总工作量（人月）">
                </el-table-column>
                <el-table-column align="center" prop="workOrderConfirmMoney" label="客户确权总收入（元）">
                  <template slot-scope="scope">
                    <span>{{
                      $get_thousand_num(scope.row.workOrderConfirmMoney)
                    }}</span>
                  </template>
                </el-table-column>
                <el-table-column align="center" label="操作">
                  <template slot-scope="scope">
                    <el-button type="text" @click="addWork">查看详情</el-button>
                  </template>
                </el-table-column>
              </el-table>
            </div>
            <!-- 项目收入拆分  注释 -->
            <!--
              <div v-if="projectRevenueList && projectRevenueList.length > 0">
                <div class="title">项目收入拆分
                  <small style="color: #333;">
                    (拆分总金额
                    <small class="danger">{{amountTotal}}元</small>
                    <small v-if="dataForm.confirmFileType==0 || dataForm.confirmFileType==2 || dataForm.confirmFileType==3">
                      ,拆分总工作量
                      <small class="danger">{{WorkloadTotal}}人月</small>
                    </small>
                  </small>
                  )
                </div>
                <div class="bgwhite" style="padding:10px;">
                  <el-table v-if="projectRevenueList && projectRevenueList.length > 0" :data="projectRevenueList" :header-cell-style="{background:'#f2f2f2'}" style="width: 100%;margin:10px 0;">
                    <el-table-column align="center" prop="projectNum" label="项目编号">
                    </el-table-column>
                    <el-table-column align="center" prop="projectName" label="项目名称">
                    </el-table-column>
                    <el-table-column align="center" prop="confirmWorkload" label="项目确认工作量（人月）">
                    </el-table-column>
                    <el-table-column align="center" prop="projectAmount" label="项目收入">
                      <template slot-scope="scope">
                        <span>{{$get_thousand_num(scope.row.projectAmount)}}</span>
                      </template>
                    </el-table-column>
                  </el-table>
                </div>
              </div>
            -->
            <div v-for="(item, index) in authList">
              <div class="title">
                确权明细({{ index + 1 }})
                <i v-if="authList.length > 1" @click="deleteAuth($event, index)" class="iconfont icon-delete fr baseColor"></i>
              </div>
              <!-- 内部确权非必填 -->
              <mt-field v-model.trim="item.name" :class="{ must: dataForm.confirmationType == 0 }" placeholder="请输入" label="确权文件名称">
                <i class="iconfont"></i>
              </mt-field>

              <!-- 客户确权展示 -->
              <datetime :max-year="3000" v-if="dataForm.confirmationType == 0" v-model="item.custConfirmTime" class="must" format="YYYY-MM-DD" placeholder="请选择" title="行方确权日期"></datetime>

              <!-- 客户确权+客户验收报告不显示 -->
              <div v-if="
                  !(
                    dataForm.confirmationType == 0 &&
                    dataForm.confirmFileType == 1
                  )
                ">
                <datetime @on-clear="item.confirmStartCycle = null" clear-text="清空" :max-year="3000" v-model="item.confirmStartCycle" format="YYYY-MM" placeholder="请选择" title="确权开始周期"></datetime>
                <datetime @on-clear="item.confirmEndCycle = null" clear-text="清空" :max-year="3000" v-model="item.confirmEndCycle" format="YYYY-MM" :start-date="item.confirmStartCycle + '-01'" placeholder="请选择" title="确权结束周期"></datetime>
              </div>
              <mt-field v-model.trim="item.confirmWorkload" @blur.native.capture="checkWorkload(index, 1)" v-if="
                  dataForm.confirmFileType == 0 ||
                  dataForm.confirmFileType == 2 ||
                  dataForm.confirmFileType == 3
                " type="number" :class="{
                  must:
                    dataForm.confirmFileType == 0 ||
                    dataForm.confirmFileType == 3,
                }" placeholder="请输入,最多两位小数" label="确认工作量">人月</mt-field>

              <!-- 客户确权+客户验收报告不显示 -->
              <div v-if="
                  !(
                    dataForm.confirmationType == 0 &&
                    dataForm.confirmFileType == 1
                  )
                ">
                <mt-field @focus.native.capture="
                    item.amount = item.amount.replace(/,/g, '')
                  " class="bordernone must" @blur.native.capture="checkMoney(item.amount, index)" v-model.trim="item.amount" type="text" placeholder="请输入" label="确权金额">元</mt-field>
              </div>

              <div v-if="false" class="danger tip-small">
                (实施类以确权计划金额作为确权金额进行拆分)
              </div>
              <mt-cell v-if="
                  dataForm.confirmFileType == 0 ||
                  dataForm.confirmFileType == 2 ||
                  dataForm.confirmFileType == 3
                " :class="{ must: dataForm.confirmationType == 0 }" class="bordernone" title="附件"></mt-cell>
              <div v-if="
                  dataForm.confirmFileType == 0 ||
                  dataForm.confirmFileType == 1 ||
                  dataForm.confirmFileType == 2 ||
                  dataForm.confirmFileType == 3
                " class="item fileBox">
                <span v-for="(val, i) in item.enclosureUrl">
                  {{ val.fileName }}
                  <i @click="deleteFile(i, 1, index)" class="iconfont icon-close"></i>
                </span>
                <span class="baseColor">+上传<input @change="fileChange($event, 1, index)" type="file" accept="" multiple /></span>
              </div>
            </div>
            <!-- 内部确权/客户确权偏差百分比 -->
            <div v-if="preSplitList.length > 0" class="percent-list">
              内部确权/客户确权偏差百分比: {{ deviationPercent }}%
            </div>
          </div>
        </div>
        <div style="color: red; text-align: center; margin-top: 20px" v-else>
          无对应确权计划，无法发起确权，请先联系QA协助创建。
        </div>
      </div>

      <!-- 判断是否有计划 -->
      <div v-if="planTypeSlot[0].values.length > 0">
        <div>
          <mt-cell class="bordernone" title="备注"></mt-cell>
          <mt-field v-model.trim="dataForm.remark" class="borderBottom10" placeholder="请输入,最多300个字" :attr="{ maxlength: 300 }" type="textarea" rows="4"></mt-field>
          <div>
            <div class="title">
              确权盖章信息
              <span style="margin-left: 15px" class="danger">确权单如我司先盖章的流程为：提交确权盖章审批→线下盖章→邮寄→项目经理在评论中上传合规的确权单→流程继续审批</span>（合同状态为备案、评审时，不允许客户确权盖章。合同状态为已签状态时，允许盖章。）
            </div>
            <mt-field v-if="
                dataForm.contractStatus == '2' ||
                dataForm.contractStatus == '-1'
              " v-model="dataForm.isSealname" class="must" @click.native="popupSlot(11)" :readonly="true" label="是否盖章" placeholder="请选择">
              <i class="iconfont icon-right"></i>
            </mt-field>
            <mt-field v-else v-model="dataForm.isSealname" class="must" :readonly="true" label="是否盖章">
            </mt-field>
            <mt-cell v-if="dataForm.isSealname == '是'" class="must bordernone" title="印章类型">
              <small style="position: absolute; left: 70px" class="gray">（可多选）</small>
            </mt-cell>
            <div class="item" v-if="dataForm.isSealname == '是'">
              <!--
                <div class="mint-cell signet" style="border-bottom:none">印章类型
                                                                   <span class="signetSelect">（可多选）</span>
                                                                 </div>
              -->
              <span v-for="(item, index) in signetList" :key="index" class="signetSelect">
                <a v-if="item.checked" @click="checkTag(index)" class="checked">{{ item.name }}</a>
                <a v-else @click="checkTag(index)">{{ item.name }}</a>
              </span>
            </div>
            <mt-cell v-if="dataForm.isSealname == '是'" class="bordernone must" title="收件人信息"></mt-cell>
            <mt-field v-if="dataForm.isSealname == '是'" v-model="dataForm.recipientInfo" class="borderBottom10" placeholder="请输入" :attr="{ maxlength: 3000 }" type="textarea" rows="4"></mt-field>
          </div>
          <mt-cell class="must bordernone" title="审批人"></mt-cell>
          <div class="item">
            <ul class="flexbox">
              <li v-for="(item, index) in approverList" class="flex-1 circle exam">
                <img v-if="item.avatar" class="img-block" :src="item.avatar" />
                <img v-else class="img-block" src="../assets/img/userImg.png" />
                <span class="next" v-if="index != approverList.length - 1"></span>
                <p>{{ item.name }}</p>
              </li>
            </ul>
          </div>
          <a class="mint-cell bordernone">
            <div class="mint-cell-wrapper">
              <div class="mint-cell-title">
                <span class="mint-cell-text">抄送人</span>
                <small class="gray">相关项目经理、销售、QA等人员</small>
              </div>
            </div>
          </a>
          <div class="item bordernone">
            <ul class="flexbox">
              <li v-for="(item, index) in noticeList" class="flex-1 circle">
                <img v-if="item.avatar" class="img-block" :src="item.avatar" />
                <img v-else class="img-block" src="../assets/img/userImg.png" />
                <i v-if="item.showflag != ''" @click="deleteUser(index, 3)" class="iconfont icon-close"></i>
                <p>{{ item.name }}</p>
              </li>
              <li @click="checkUser(3)" class="flex-1 circle">
                <div class="img-block">+</div>
              </li>
            </ul>
          </div>
          <div class="text-center gray tip">
            <small>如有疑问请联系项目管理部</small>
          </div>
          <div class="btn">
            <mt-button @click="submit" type="primary" size="large">提交</mt-button>
          </div>
        </div>
      </div>
    </div>
    <mt-popup v-model="popupVisible" popup-transition="popup-fade" position="bottom">
      <ul class="flex-center flex flexbtns">
        <li style=" font-size: 16px;width: 50%;text-align: left; padding-left: 20px; " @click="cancle()">取消 </li>
        <li style="
            font-size: 16px;
            width: 50%;
            text-align: right;
            padding-right: 20px;
          " class="baseColor" @click="sure()">
          确定
        </li>
      </ul>
      <mt-picker :slots="slots" @change="onValuesChange"></mt-picker>
    </mt-popup>
  </div>
</template>

<script>
import httpServer from "@/assets/init/initaxios";
import dingding from "@/assets/init/dingdinginit";
import init from "@/assets/init/init";
import axios from "axios";
import { MessageBox, Indicator } from "mint-ui";

export default {
  name: "authRight",
  data() {
    return {
      signetList: [
        //印章类型
        { id: "7", name: "网络公章", checked: false },
        { id: "2", name: "软件公章", checked: false },
        { id: "6", name: "法人章", checked: false },
      ],
      workDetailFile: {},
      deviationReason: "",
      workDetailIncome: "",
      workDetailPerson: "",
      planList: [],
      fukuanInfo: {
        collePayForm: "",
        amount: "",
      },
      contractsubStatus: "已签",
      contractsubInfo: {},
      fileList: [], //业务采购合同上传确权文件
      zbDataform: {
        //业务采购合同确权数据
        contractId: "",
        planId: "",
        planName: "",
        confirmFileType: "",
        amount: "",
        confirmWorkload: "",
        sealType: "",
        subConfirmReason: "",
        coFiles: [],
        projectConfirmState: "",
        zbStatusname: "",
        yzTypename: "",
      },
      acatulTotalmoney: 0,
      acatulTotalpeople: 0,
      checkworkList: [], //查询订单数据信息
      WorkOrderData: [],
      // projectRevenueList: [],    //项目收入拆分数据列表
      psInfo: [],
      acatulTotalmoney: 0,
      format: "YYYY-MM-DD",
      startDateCycle: null,
      hasStartDateCycle: false, //判断对应确权开始周期是否可选，true不可选
      hasEndDateCycle: false, //判断对应确权开始周期是否可选，true不可选
      isAuthChecked: true, //true可以修改确权信息，false内部确权不可修改
      value: null, // 选中的值
      slotsIndex: null, // 确权list index
      slotsType: null, // popupSlot(type) 传入的type值
      slots: [],
      authList: [
        {
          name: null, // 确权文件名称
          custConfirmTime: null, // 行方确权日期
          confirmStartCycle: null, //确权开始周期
          confirmEndCycle: null, //确权结束周期
          incomeEstimate: null, //估算说明
          enclosureUrl: [
            {
              fileName: "",
              annexUrl: "",
            },
          ], // 客户验收报告或者附件list
          onlineReport: [
            {
              fileName: "",
              annexUrl: "",
            },
          ], // 上线报告
          confirmWorkload: null, // 工作量
          amount: "0", // 确权金额
          preCustConfirmTime: null, //预计客户确权时间
        },
      ],
      projectSplitList: [],
      popupVisible: false,
      dataForm: {
        yearResult: "", //业绩年
        recipientInfo: "", //收件人信息
        isSeal: "", //盖章审批
        isSealname: "",
        planAmount: 0, //计划总金额
        amount: "0", //确权总金额
        contractStatusName: null, //合同状态名称
        contractStatus: null, //合同状态
        confirmationType: "", // 确权类型 1客户确权，2内部确权，3业务采购合同确权
        confirmationTypeName: "",
        confirmcustomersType: null, //客户确权方式 1客户盖章（总行章或部门章均有效） 2客户工作邮箱+确权单附件（有客户签字） 3 确权单（有客户签字）+客户名片 4其他
        confirmcustomersTypeName: null,
        comment: "", //客户确权方式下的备注
        projectName: null, // 项目名称
        projectNum: null, //项目编号
        projectManager: null, // 项目经理
        contractName: null, // 销售合同名称
        contractNum: null, //合同编号
        subContractType: null, // 销售核算类型
        subContractTypeName: null, // 销售核算类型名称
        planId: null, // 确权计划id
        planName: null, // 计划名称
        startCycle: null, // 对应开始周期
        endCycle: null, // 对应结束周期
        confirmFileType: null, // 确权文件类型
        confirmFileTypeName: null, // 确权文件类型名称
        contractConfirm: null, // 确权信息
        isProjectSplit: null, // 项目收入是否拆分 确权拆分0否，反显项目 1是
        isProjectSplitName: null, // 是否拆分名称 否 是
        contractProjectSplit: null, // 项目拆分
        approverId: null, // 审批人
        notificationsId: null, // 通知人
        createUserId: null, // 登录人
        contractId: null, // 合同id
        supplierName: null, // 供应商名称
        relationContractName: null, //关联销售合同名称
        relationContractNum: null, //关联合同编号
        planTotalMoney: null, // 计划收款总金额
        nowBackMoney: null, // 已回款金额
        remark: null, // 备注
        bankName: null, //签约客户
        orderName: "", //订单名称
        orderNum: "", //订单编号
        orderId: "", //订单id
      },
      projectInfo: null, // 项目信息
      contractInfo: null, // 业务采购合同信息
      saleContractInfo: null, // 销售合同
      newSalesContractInfo: null, //
      subContractInfo: null, // 业务采购合同
      approverList: [], // 审批人
      noticeList: [], // 通知人
      planTypeSlot: [
        {
          // 确权对应的确权计划
          flex: 1,
          values: [],
          className: "slot",
          value: "",
          textAlign: "center",
        },
      ],
      planType: {},
      subContractTypeSlot: [
        {
          // 确权对应的确权计划
          flex: 1,
          values: [],
          className: "slot",
          value: "",
          textAlign: "center",
        },
      ],
      subContractType: {},
      splitListSlot: [
        {
          flex: 1,
          values: [],
          className: "slot",
          value: "",
          textAlign: "center",
        },
      ],
      splitType: {},
      preConfirmInfo: null,
      preSplitList: [], //预计项目收入拆分
      deviationPercent: 0, //偏差百分比
      inquiryFlag: true, //如果有历史记录的话，但是点击的是不复用
      storageContent: null, //查询历史记录数据
      orderList: [], //订单数据
      realSubContractList: [], //真实子合同信息
      realSubContractInfo: {}, //选择真实子合同信息
    };
  },
  created: function () {
    console.log("authRight页面--created");
    dingding.setHeader({ title: "合同确权" });
    let contractInfo = JSON.parse(sessionStorage.getItem("contactInfo"));
    if (contractInfo) {
      this.contractsubInfo = contractInfo;
      this.contractsubInfo.subContract.estimatedAmount = this.$get_thousand_num(
        contractInfo.subContract.estimatedAmount
      );
      this.contractsubInfo.subContract.subContractType =
        init.subcontractType[this.contractsubInfo.subContract.subContractType];
      this.contractsubInfo.saleContract.name =
        "（" +
        this.contractsubInfo.saleContract.contractNum +
        "）" +
        this.contractsubInfo.saleContract.name;
      this.contractsubInfo.saleContract.state =
        init.contractStatue[this.contractsubInfo.saleContract.state];
      let projectName = this.contractsubInfo.project
        ? "（" +
        this.contractsubInfo.project.projectNum +
        "）" +
        this.contractsubInfo.project.projectName
        : "";
      this.contractsubInfo.project && this.contractsubInfo.project.projectName
        ? (this.contractsubInfo.project.projectName = projectName)
        : "";
      this.queryPlan2(this.contractsubInfo.subContract.id);
      this.approverList = contractInfo.approvalUsers
        ? contractInfo.approvalUsers
        : [];
      this.noticeList = contractInfo.ccUsers ? contractInfo.ccUsers : [];
    }
    if (sessionStorage.getItem("zbDataform")) {
      this.zbDataform = JSON.parse(sessionStorage.getItem("zbDataform"));
    }
    if (sessionStorage.getItem("dataForm")) {
      this.dataForm = JSON.parse(sessionStorage.getItem("dataForm"));
    }
    if (sessionStorage.getItem("signetList")) {
      this.signetList = JSON.parse(sessionStorage.getItem("signetList"));
    }
    if (sessionStorage.getItem("workDetailIncome")) {
      this.workDetailIncome = sessionStorage.getItem("workDetailIncome");
    }
    if (sessionStorage.getItem("workDetailPerson")) {
      this.workDetailPerson = sessionStorage.getItem("workDetailPerson");
    }
    if (this.dataForm.confirmationType == "0") {
      this.queryConfirmLatestRecord();
    }
  },
  watch: {
    authList: {
      handler(newVal, oldVal) {
        let total = 0,
          total1 = 0;
        let list = newVal.map((val) => {
          total += Number(val.amount.toString().replace(/,/g, ""));
          total1 += Number(val.confirmWorkload);
          return val;
        });
        if (
          this.dataForm.isProjectSplitName == "否" &&
          this.projectSplitList.length == 1
        ) {
          this.projectSplitList[0].projectAmount = total;
          this.projectSplitList[0].Workload = total1;
        }

        // this.amountTotal = total.toFixed(2)
        // this.WorkloadTotal = total1.toFixed(2)
        return list;
      },
      deep: true,
      immediate: true,
    },
  },
  computed: {
    // 确权总金额
    amountTotal() {
      let total = 0;
      this.authList.forEach((val) => {
        total += Number(val.amount.toString().replace(/,/g, ""));
      });
      if (
        this.dataForm.isProjectSplitName == "否" &&
        this.projectSplitList.length == 1
      ) {
        this.projectSplitList[0].projectAmount = total;
      }
      if (this.preSplitList.length > 0) {
        if (total == 0) {
          this.deviationPercent = 0;
        } else {
          this.deviationPercent = (
            ((Number(this.preConfirmInfo.amount.toString().replace(/,/g, "")) -
              total) *
              100) /
            total
          ).toFixed(2);
        }
      }
      return $get_thousand_num(total);
    },
    // 确认工作量总人月
    WorkloadTotal() {
      let total1 = 0;
      this.authList.map((val) => {
        total1 += Number(val.confirmWorkload);
      });

      if (
        this.dataForm.isProjectSplit == "否" &&
        this.projectSplitList.length == 1
      ) {
        this.projectSplitList[0].Workload = total1;
      }

      return total1.toFixed(2);
    },
  },
  methods: {
    //选择印章类型
    checkTag(index) {
      this.signetList[index].checked = !this.signetList[index].checked;
    },
    //导出工作量明细
    exportWorkConfirm() {
      let vm = this;
      let contractId = vm.saleContractInfo.id;
      let woes = [];
      vm.checkworkList.forEach(function (ele) {
        woes.push(ele.id);
      });
      let orderIds = woes.toString();
      let params = {
        channelNo: "YNET22",
        contractId: contractId,
        orderIds: orderIds,
      };
      this.outport(
        "export_manager_contract/queryWorkConfirmDetailExport",
        "工作量明细表",
        params
      );
    },
    //导出工作量明细
    // exportWorkConfirm(){
    //    let contractId = this.saleContractInfo.id
    //    let woes=[]
    //    this.checkworkList.forEach(function(ele) {
    //      woes.push(ele.id)
    //    });
    //    let orderIds = woes.toString();
    //    window.location.href = init.baseUrl2 + `export_manager_contract/queryWorkConfirmDetailExport?channelNo=YNET22&contractId=${contractId}&orderIds=${orderIds}`
    // },
    //导入工作量明细
    getbtn() {
      this.$refs.fileBtn.click();
    },
    getFile(event) {
      let files = event.target.files;
      let formData = new FormData();
      formData.append("channelNo", "YNET22");
      for (var i = 0; i < files.length; i++) {
        formData.append(`file`, files[i]);
      }
      Indicator.open();
      axios({
        method: "POST",
        url: init.addFile,
        headers: { "content-type": "multipart/form-data" },
        data: formData,
      }).then((res) => {
        if (res.data.code == "0") {
          if (res.data.data.length > 0) {
            this.workDetailFile = res.data.data[0];
            this.workDetailFile.fileAddress = this.workDetailFile.annexUrl;
            this.importWorkDetail(event);
          }
          Indicator.close();
        } else {
          MessageBox.alert(res.data.msg, "提示");
          Indicator.close();
        }
        event.target.value = "";
      });
    },
    importWorkDetail(event) {
      let vm = this;
      vm.file = event.target.files[0];
      let formData = new FormData();
      formData.append("multipartFile", vm.file);
      formData.append("channelNo", "YNET22");
      let config = {
        headers: {
          "Content-Type": "multipart/form-data",
        },
      };
      httpServer(
        { url: "/contract_confirm_computer/importWorkDetail", method: "POST" },
        formData,
        config
      ).then(function (res) {
        if (res.data.code == 0) {
          vm.workDetailIncome = res.data.data.workDetailIncome;
          vm.workDetailPerson = res.data.data.workDetailPerson;
        } else {
          if (res.data.msg) {
            MessageBox.alert(res.data.msg, "提示");
          } else {
            MessageBox.alert(res.data.msg, "提示");
          }
        }
      });
    },
    //获取订单信息数据
    queryWorkOrderByABvalue() {
      Indicator.open();
      httpServer(
        {
          url: "contract_confirm_computer/queryWorkOrderByABvalue",
          method: "post",
        },
        { woes: this.checkworkList }
      ).then((res) => {
        if (res.data.code == "0") {
          this.WorkOrderData = res.data.data ? [res.data.data.abInfo] : [];
          // this.projectRevenueList = res.data.data ? res.data.data.projectSplitInfo : []
          this.psInfo = res.data.data ? res.data.data.psInfo : [];
        } else {
          MessageBox.alert(res.data.msg, "提示");
        }
        Indicator.close();
      });
    },

    //订单查询
    queryOrderListByCondition() {
      Indicator.open();
      httpServer(
        {
          url: "system_interface_order/queryOrderListByCondition",
          method: "POST",
        },
        {
          orderNameOrNum: "",
          orderListType: "2",
          projectId: this.projectInfo.id,
        }
      ).then((res) => {
        if (res.data.code == "0") {
          this.orderList = res.data.data ? res.data.data : [];
        } else {
          MessageBox.alert(res.data.msg, "提示");
        }
        Indicator.close();
      });
    },

    //添加订单
    addWork() {
      // this.$router.push({ path: 'checkWorkorder', query: { contractId: this.saleContractInfo.id, projectId: this.projectInfo.id, confirmFileType: this.dataForm.confirmFileType } })
      this.$router.push({
        path: "checkWorkorder",
        query: {
          contractId: this.newSalesContractInfo.id,
          confirmFileType: this.dataForm.confirmFileType,
        },
      });
      sessionStorage.setItem("dataForm", JSON.stringify(this.dataForm));
      sessionStorage.setItem("signetList", JSON.stringify(this.signetList));
      sessionStorage.setItem("authList", JSON.stringify(this.authList));
      sessionStorage.setItem("workDetailIncome", this.workDetailIncome);
      sessionStorage.setItem("workDetailPerson", this.workDetailPerson);
    },
    // 工作量格式校验
    checkWorkload(index, type) {
      if (type == 1) {
        if (!/^\d+(.\d{0,2})?$/.test(this.authList[index].confirmWorkload)) {
          MessageBox.alert(`确认工作量格式不正确`, "提示");
          this.authList[index].confirmWorkload = "";
        } else {
          this.authList[index].confirmWorkload = Number(
            this.authList[index].confirmWorkload
          ).toFixed(2);
        }
      } else if (type == 2) {
        if (!/^\d+(.\d{0,2})?$/.test(this.projectSplitList[index].Workload)) {
          MessageBox.alert(`拆分工作量格式不正确`, "提示");
          this.projectSplitList[index].Workload = "";
        } else {
          this.projectSplitList[index].Workload = Number(
            this.projectSplitList[index].Workload
          ).toFixed(2);
        }
      }
    },
    //判断金额大于万万
    checkMoney(money, index) {
      if (money.toString().replace(/,/g, "") >= 10000000) {
        MessageBox.alert(`您输入的金额为${money}元，请确认`, "提示");
      }
      let json = this.authList[index];
      json.amount = this.$get_thousand_num(money.toString().replace(/,/g, ""));
      this.$set(this.authList, index, json);
    },
    confirmDate() {
      this.startDateCycle = this.dataForm.startCycle + "-01";
    },
    getHistoryUser(type) {
      //获取历史通知人+联系人
      //1-合同备案 2-合同确权 3-合同评审 4-合同变更 5-合同取消
      let userInfo = JSON.parse(localStorage.getItem("userInfo_HETONGQUEQUAN"));
      Indicator.open();
      httpServer(
        { method: "POST", url: "./contractApproval/queryAprovalAndNotifi" },
        { type: type, userId: userInfo.userid }
      ).then((response) => {
        if (response.data.code == "0") {
          this.noticeList = response.data.data.copyUser.map((val) => {
            let json = {
              name: val.memberName,
              avatar: val.avatar,
              userid: val.dingdingId,
            };
            return json;
          });
          this.approverList = response.data.data.approvalUser.map((val) => {
            let json = {
              name: val.memberName,
              avatar: val.avatar,
              userid: val.dingdingId,
            };
            return json;
          });
        } else {
          MessageBox.alert(response.data.msg, "提示");
        }
        Indicator.close();
      });
    },
    //获取审批人
    getapproveUser() {
      if (!this.saleContractInfo.id) {
        return;
      }
      Indicator.open();
      httpServer(
        {
          method: "POST",
          url: "./contract_antiDisplay_mobile/queryContractConfirmApprovalUser",
        },
        {
          confirmType: this.dataForm.confirmationType,
          contractId: this.saleContractInfo.id,
          isSeal: this.dataForm.isSeal,
        }
      ).then((response) => {
        if (response.data.code == "0") {
          this.approverList = response.data.data.approvalList.map((val) => {
            let json = {
              name: val.memberName,
              avatar: val.avatar,
              userid: val.dingdingId,
            };
            return json;
          });
          this.noticeList = response.data.data.ccList.map((val) => {
            let json = {
              name: val.memberName,
              avatar: val.avatar,
              userid: val.dingdingId,
              showflag: "",
            };
            return json;
          });
        } else {
          MessageBox.alert(response.data.msg, "提示");
        }
        Indicator.close();
      });
    },
    /* 下拉选框start */
    onValuesChange: function (picker, values) {
      if (this.slotsType == 3) {
        this.value = values.join("-");
      } else {
        this.value = values[0];
      }
    },
    sure: function () {
      this.popupVisible = false;
      switch (this.slotsType) {
        case 1: // 确权类别
          this.dataForm.confirmationTypeName = this.value;
          this.dataForm.confirmationType = init.authRightType[this.value]; //查询销售合同
          if (this.dataForm.confirmationType == "0") {
            this.queryConfirmLatestRecord();
          }
          // if (sessionStorage.getItem('projectInfo')) {
          //   if (this.dataForm.confirmationType == 0 || this.dataForm.confirmationType) {
          //     this.queryContract()
          //   }
          // }
          // if (this.saleContractInfo) {
          //   this.queryPlan()
          //   this.queryCycleWeek()
          // }
          // if (this.projectInfo != null) {
          //   this.getapproveUser();
          // }
          break;
        case 2: // 确权对应的确权计划
          this.dataForm.planName = this.value;
          this.dataForm.planId = this.planType[this.value].id;

          //根据计划差内部确权信息
          this.queryConfirmByPlan();
          if (this.value == "无") {
            this.queryCycleWeek();
          } else {
            if (
              this.planType[this.value].startCycle &&
              this.planType[this.value].startCycle != ""
            ) {
              this.dataForm.startCycle = this.planType[this.value].startCycle;
              this.authList = this.authList.map((val) => {
                val.confirmStartCycle = this.dataForm.startCycle;
                return val;
              });
              this.startDateCycle = this.dataForm.startCycle + "-01";
              this.hasStartDateCycle = true;
            } else {
              this.queryCycleWeek();
            }
            if (
              this.planType[this.value].endCycle &&
              this.planType[this.value].endCycle != ""
            ) {
              this.dataForm.endCycle = this.planType[this.value].endCycle;
              this.authList = this.authList.map((val) => {
                val.confirmEndCycle = this.dataForm.endCycle;
                return val;
              });
              this.hasEndDateCycle = false;
            } else {
              this.queryCycleWeek();
            }
            this.dataForm.planAmount = this.$get_thousand_num(
              this.planType[this.value].amount
            );
          }
          break;
        case 3: //项目拆分选择项目
          this.projectSplitList[this.slotsIndex].projectName =
            this.splitType[this.value].projectName;
          this.projectSplitList[this.slotsIndex].projectNum =
            this.splitType[this.value].projectNum;
          this.projectSplitList[this.slotsIndex].projectId =
            this.splitType[this.value].id;
          this.projectSplitList = this.projectSplitList;
          break;
        case 4: // 确权文件类型
          if (this.dataForm.confirmationType == "1") {
            this.zbDataform.confirmFileTypeName = this.value;
            this.zbDataform.confirmFileType =
              init.subcontractrightFileType[this.value];
          } else {
            this.dataForm.confirmFileTypeName = this.value;
            this.dataForm.confirmFileType = init.rightFileType[this.value];
          }
          if (sessionStorage.getItem("selectedworkList")) {
            sessionStorage.removeItem("selectedworkList"); //删除添加选中的订单
            this.checkworkList = [];
            this.WorkOrderData = [];
            this.projectRevenueList = [];
            this.psInfo = [];
          }
          for (let i = 0; i < this.authList.length; i++) {
            //改变确权文件类型工作量和确权金额重置
            this.authList[i].amount = 0;
            this.authList[i].confirmWorkload = "";
          }
          break;
        case 5: //预计确权金额是否拆分
          this.dataForm.isProjectSplitName = this.value;
          this.dataForm.isProjectSplit = this.value;
          if (this.dataForm.isProjectSplit == "否") {
            this.projectSplitList = [
              {
                projectName: this.projectInfo.projectName,
                projectNum: this.projectInfo.projectNum,
                projectId: this.projectInfo.id,
                Workload: this.WorkloadTotal,
                projectAmount: this.amountTotal,
              },
            ];
          } else if (this.dataForm.isProjectSplit == "是") {
            this.projectSplitList = [
              {
                projectName: null,
                projectNum: null,
                projectId: null,
                Workload: 0,
                projectAmount: 0,
              },
            ];
          }
          break;
        case 6: //选择业务采购合同
          this.subContractInfo = this.subContractType[this.value];
          this.queryPlan();
          break;
        case 7: //客户确权方式
          this.dataForm.confirmcustomersTypeName = this.value;
          this.dataForm.confirmcustomersType =
            init.confirmcustomersType[this.value]; //客户确权方式
          break;
        case 8: //业务采购合同确权确权状态
          this.zbDataform.zbStatusname = this.value;
          this.zbDataform.projectConfirmState =
            init.authRightStatus[this.value]; //业务采购合同确权确权状态
          break;
        case 9: //业务采购合同确权印章类型
          this.zbDataform.yzTypename = this.value;
          this.zbDataform.sealType = init.sealType[this.value]; //业务采购合同确权印章类型
          break;
        case 10: // 关联付款计划
          this.zbDataform.planName = this.value;
          this.zbDataform.planId = this.planType[this.value].id;
          for (let i = 0; i < this.planList.length; i++) {
            if (this.zbDataform.planId == this.planList[i].id) {
              this.fukuanInfo.collePayForm =
                init.fukuanType[this.planList[i].collePayForm];
              this.fukuanInfo.amount = this.$get_thousand_num(
                this.planList[i].amount
              );
            }
          }
          break;
        case 11: //是否盖章
          this.dataForm.isSealname = this.value;
          this.dataForm.isSeal = init.yesOrNoType[this.value];
          this.getapproveUser(); //获取审批人
          break;
      }
      this.slots = null;
    },
    cancle: function () {
      this.value = "";
      this.popupVisible = false;
      this.slots = null;
    },
    popupSlot(type, index) {
      this.popupVisible = true;
      this.slotsType = type;
      this.slotsIndex = index;
      switch (type) {
        case 1: // 确权类别
          this.slots = init.authRightTypeSlot;
          this.value = init.authRightTypeSlot[0].values[0];
          break;
        case 2: // 确权对应的确权计划
          this.slots = this.planTypeSlot;
          this.value = this.planTypeSlot[0].values[0];
          break;
        case 10: // 关联付款计划
          this.slots = this.planTypeSlot;
          this.value = this.planTypeSlot[0].values[0];
          break;
        case 3: // 项目拆分选择项目
          let arr = [];
          this.saleContractInfo.projects.map((val, index) => {
            this.splitType[val.projectName] = val;
            let status = true;
            for (let i = 0; i < this.projectSplitList.length; i++) {
              if (val.projectName == this.projectSplitList[i].projectName) {
                status = false;
              }
            }
            if (status) {
              arr.push(val.projectName);
            }
          });
          this.splitListSlot[0].values = arr;
          this.slots = this.splitListSlot;
          this.value = arr[0];
          break;
        case 4: // 确权文件类型
          this.slots =
            this.dataForm.confirmationType == "1"
              ? init.subcontractrightFileTypeSlot
              : init.rightFileTypeSlot;
          this.value =
            this.dataForm.confirmationType == "1"
              ? init.subcontractrightFileTypeSlot[0].values[0]
              : init.rightFileTypeSlot[0].values[0];
          break;
        case 5: // 预计确权金额是否拆分
          this.slots = init.yesOrNoTypeSlot;
          this.value = init.yesOrNoTypeSlot[0].values[0];
          break;
        case 6: // 业务采购合同名称
          this.slots = this.subContractTypeSlot;
          this.value = this.subContractTypeSlot[0].values[0];
          break;
        case 7: // 客户确权 方式
          this.slots = init.confirmcustomersTypeSlot;
          this.value = init.confirmcustomersTypeSlot[0].values[0];
          break;
        case 8: // 客户确权 状态
          this.slots = init.authRightStatusSlot;
          this.value = init.authRightStatusSlot[0].values[0];
          break;
        case 9: // 印章类型
          this.slots = init.sealTypeSlot;
          this.value = init.sealTypeSlot[0].values[0];
          break;
        case 11: // 是否盖章
          this.slots = init.yesOrNoTypeSlot;
          this.value = init.yesOrNoTypeSlot[0].values[0];
          break;
      }
    },
    /* 下拉选框end */
    // 根据计划查询确权信息
    queryConfirmByPlan() {
      Indicator.open();
      httpServer(
        {
          method: "POST",
          url: "./contract_confirm_computer/queryPlanContractConfirm",
        },
        { planId: this.dataForm.planId }
      ).then((response) => {
        debugger;
        if (response.data.code == "0") {
          let info = response.data.data;
          let list = info.contractProjectSplitList ? info.contractProjectSplitList : [];
          this.preSplitList = list.map((val, i) => {
            if (val.confirmWorkload && val.projectAmount) {
              val.avgSalary = (val.projectAmount / val.confirmWorkload).toFixed(2);
            }
            val.projectAmount = this.$get_thousand_num(val.projectAmount);
            val.avgSalary = this.$get_thousand_num(val.avgSalary);
            return val;
          });
          this.preConfirmInfo = {
            amount: info.amountSum ? this.$get_thousand_num(info.amountSum) : 0,
            workload: info.confirmWorkloadSum ? info.confirmWorkloadSum : 0,
          };
        } else {
          MessageBox.alert(response.data.msg, "提示");
        }
        Indicator.close();
      });
    },
    // 查询开始周期
    queryCycleWeek() {
      Indicator.open();
      let param = {
        // contractId: this.saleContractInfo.id
        contractId: this.newSalesContractInfo.id,
      };
      httpServer(
        {
          method: "POST",
          url: "./contract_confirm_computer/getEndCycleByContractId",
        },
        param
      ).then((res) => {
        if (res.data.code == "0") {
          if (res.data.data == "") {
            this.hasStartDateCycle = false;
            this.hasEndDateCycle = false;
          } else {
            this.hasStartDateCycle = true;
            this.hasEndDateCycle = false;
            let arr = res.data.data && res.data.data.split("-");
            if (arr[1] === "12") {
              arr[0] = Number(arr[0]) + 1;
              arr[1] = 1;
            } else {
              arr[1] = Number(arr[1]) + 1;
            }
            arr[1] = arr[1] > 9 ? arr[1] : "0" + arr[1];
            this.dataForm.startCycle = arr.join("-");
            this.startDateCycle = this.dataForm.startCycle + "-01";
            this.dataForm.endCycle = arr.join("-");
            this.authList = this.authList.map((val) => {
              val.confirmStartCycle = this.dataForm.startCycle;
              val.confirmEndCycle = this.dataForm.endCycle;
              return val;
            });
          }
        } else {
          MessageBox.alert(res.data.msg, "提示");
        }
        Indicator.close();
      });
    },
    // 确权拆分选择是查询拆分明细
    queryPlan() {
      Indicator.open();
      var id;
      if (this.dataForm.confirmationType == 2) {
        id = this.subContractInfo.id;
      } else {
        id = this.saleContractInfo.id;
      }

      let param = {
        contractId: id,
        // projectId: this.projectInfo.id
      };

      httpServer(
        {
          method: "POST",
          url: "contract_confirm_computer/getPlanByContractIdAndType",
        },
        param
      ).then((res) => {
        if (res.data.code == 0) {
          this.planType = {};
          var arr = []; //['无']
          //this.planType['无'] = {id: ''}
          res.data.data.map((val, i) => {
            this.planType[val.name] = val;
            this.planType[val.id] = val;
            arr.push(val.name);
          });
          if (this.inquiryFlag && this.storageContent) {
            this.dataForm.planId = this.storageContent.planId;
            this.dataForm.planName =
              this.planType[this.storageContent.planId].name;
            //根据计划差内部确权信息
            this.queryConfirmByPlan();
          }
          this.planTypeSlot[0].values = arr;
        } else {
          MessageBox.alert(res.data.msg, "提示");
        }
        Indicator.close();
      });
    },
    //获取关联计划
    queryPlan2(id) {
      Indicator.open();
      let param = {
        contractId: id,
        confirmationType: "2",
      };
      httpServer(
        {
          method: "POST",
          url: "contract_confirm_computer/getPlanByContractIdAndType",
        },
        param
      ).then((res) => {
        if (res.data.code == 0) {
          this.planList = res.data.data;
          this.planType = {};
          var arr = []; //['无']
          //this.planType['无'] = {id: ''}
          res.data.data.map((val, i) => {
            this.planType[val.name] = val;
            this.planType[val.id] = val;
            arr.push(val.name);
          });
          if (this.inquiryFlag && this.storageContent) {
            this.dataForm.planId = this.storageContent.planId;
            this.dataForm.planName =
              this.planType[this.storageContent.planId].name;
          }
          this.planTypeSlot[0].values = arr;
        } else {
          MessageBox.alert(res.data.msg, "提示");
        }
        Indicator.close();
      });
    },
    // 获取项目关联的合同
    queryContract() {
      Indicator.open();
      let param = {
        projectId: this.projectInfo.id,
        type: this.dataForm.confirmationType,
      };
      httpServer(
        {
          method: "POST",
          url: "./contract_confirm_computer/queryProjectContract",
        },
        param
      ).then((res) => {
        if (res.data.code == "0") {
          let arr = [];
          this.subContractType = {};
          arr = res.data.data.map((val, index) => {
            val.subContractTypeName = null;
            if (val.subContractType && val.subContractType != "") {
              val.subContractTypeName =
                init.subcontractType[Number(val.subContractType)];
            }
            this.subContractType[val.name] = val;
            return val.name;
          });
          this.subContractTypeSlot[0].values = arr;

          //会查询真实子合同信息
          this.queryRealSonContractList(this.saleContractInfo.id);
        } else {
          MessageBox.alert(res.data.msg, "提示");
        }
        Indicator.close();
      });
    },
    queryRealSonContractList(id) {
      Indicator.open();
      httpServer(
        {
          method: "POST",
          url: "./contract_confirm_computer/queryRealSonContractList",
        },
        { contractId: id }
      ).then((res) => {
        if (res.data.code == "0") {
          this.realSubContractList = res.data.data ? res.data.data : [];
        } else {
          MessageBox.alert(res.data.msg, "提示");
        }
        Indicator.close();
      });
    },
    //上传图片
    fileChange(event, type, parentIndex) {
      let files = event.target.files;
      let formData = new FormData();
      formData.append("channelNo", "YNET22");
      for (var i = 0; i < files.length; i++) {
        formData.append(`file`, files[i]);
      }
      Indicator.open();
      axios({
        method: "POST",
        url: init.addFile,
        headers: { "content-type": "multipart/form-data" },
        data: formData,
      }).then((res) => {
        if (res.data.code == "0") {
          let list = res.data.data;
          if (type == 1) {
            //附件或者客户验收报告
            this.authList[parentIndex].enclosureUrl =
              this.authList[parentIndex].enclosureUrl.concat(list);
          } else if (type == 2) {
            //上线报告
            this.authList[parentIndex].onlineReport =
              this.authList[parentIndex].onlineReport.concat(list);
          } else if (type == 3) {
            //业务采购合同确权时上传确权文件   去其他都不符合
            this.zbDataform.coFiles = this.zbDataform.coFiles.concat(list);
            this.zbDataform.coFiles.map((item) => {
              item.fileAddress = item.annexUrl;
            });
          }
          this.authList = this.authList;
          sessionStorage.setItem("authList", JSON.stringify(this.authList));
          Indicator.close();
        } else {
          MessageBox.alert(res.data.msg, "提示");
          Indicator.close();
        }
        event.target.value = "";
      });
    },
    deleteFile(index, type, parentIndex) {
      if (type == 1) {
        //附件或者客户验收报告
        this.authList[parentIndex].enclosureUrl.splice(index, 1);
      } else if (type == 2) {
        //上线报告
        this.authList[parentIndex].onlineReport.splice(index, 1);
      } else if (type == 3) {
        //业务采购合同确权时上传确权文件   去其他都不符合
        this.zbDataform.coFiles.splice(index, 1);
      }
      this.authList = this.authList;
      sessionStorage.setItem("authList", JSON.stringify(this.authList));
    },
    checkUser(type) {
      //1:销售负责人， 2: 审批人， 3：通知人
      var vm = this;
      dingding.initConfig();
      let res = JSON.parse(localStorage.getItem("_config_HETONGQUEQUAN"));
      dd.error(function (error) {
        MessageBox.alert(error);
      });
      Indicator.open();
      var vm = this;
      dd.ready(function () {
        dd.biz.contact.complexPicker({
          title: "选择联系人", //标题
          corpId: init.corpId, //企业的corpId
          multiple: type != 1, //是否多选
          limitTips: "超出了", //超过限定人数返回提示
          maxUsers: 1000, //最大可选人数
          pickedUsers: [], //已选用户
          pickedDepartments: [], //已选部门
          disabledUsers: [], //不可选用户
          disabledDepartments: [], //不可选部门
          requiredUsers: [], //必选用户（不可取消选中状态）
          requiredDepartments: [], //必选部门（不可取消选中状态）
          appId: res.agentid, //微应用的Id
          permissionType: "GLOBAL", //选人权限，目前只有GLOBAL这个参数
          responseUserOnly: true, //返回人，或者返回人和部门
          startWithDepartmentId: 0, // 0表示从企业最上层开始，IOS不支持该字段
          onSuccess: function (result) {
            httpServer(
              { method: "POST", url: "./dingding/getMailList" },
              { userInfo: result.users }
            ).then((response) => {
              if (response.data.code == "0") {
                if (type == 1) {
                  vm.dataInfo.saleName =
                    response.data.data.dingdingUser[0].name;
                  vm.dataForm.headOfSales =
                    response.data.data.dingdingUser[0].userid;
                } else if (type == 2) {
                  var users = vm.approverList;
                  for (
                    var i = 0;
                    i < response.data.data.dingdingUser.length;
                    i++
                  ) {
                    var status = true;
                    for (var j = 0; j < vm.approverList.length; j++) {
                      if (
                        vm.approverList[j].userid ==
                        response.data.data[i].userid
                      ) {
                        status = false;
                      }
                    }
                    if (status) {
                      users.push(response.data.data.dingdingUser[i]);
                    }
                    status = true;
                  }
                  vm.approverList = users;
                  sessionStorage.setItem(
                    "approverList",
                    JSON.stringify(vm.approverList)
                  );
                } else if (type == 3) {
                  var users = vm.noticeList;
                  for (
                    var i = 0;
                    i < response.data.data.dingdingUser.length;
                    i++
                  ) {
                    var status = true;
                    for (var j = 0; j < vm.noticeList.length; j++) {
                      if (
                        vm.noticeList[j].userid ==
                        response.data.data.dingdingUser[i].userid
                      ) {
                        status = false;
                      }
                    }
                    if (status) {
                      users.push(response.data.data.dingdingUser[i]);
                    }
                    status = true;
                  }
                  vm.noticeList = users;
                  sessionStorage.setItem(
                    "noticeList",
                    JSON.stringify(vm.noticeList)
                  );
                }
              } else {
                MessageBox.alert(response.data.msg, "提示");
              }
              Indicator.close();
            });
          },
          onFail: function (err) {
            Indicator.close();
          },
        });
      });
    },
    deleteUser(index, type) {
      if (type == 2) {
        this.approverList.splice(index, 1);
        this.approverList = this.approverList;
      } else if (type == 3) {
        this.noticeList.splice(index, 1);
        this.noticeList = this.noticeList;
      }
    },
    nextUrl(url, type, id) {
      if (url == "checkcontract" && !id) {
        MessageBox.alert("请先选择项目！", "提示");
        return false;
      }
      //this.dataForm.confirmationType

      this.$router.push({
        path: url,
        query: {
          type: type,
          confirmationType: this.dataForm.confirmationType,
          id:
            type == 2
              ? this.projectInfo.id
              : type == 3
                ? this.saleContractInfo.id
                : id,
        },
      });
      sessionStorage.setItem("dataForm", JSON.stringify(this.dataForm));
      sessionStorage.setItem("signetList", JSON.stringify(this.signetList));
      sessionStorage.setItem("inquiryFlag", this.inquiryFlag);
      sessionStorage.setItem("workOrder", false);
      sessionStorage.removeItem("authworkerList");
      sessionStorage.removeItem("selectedworkList"); //删除添加选中的订单
    },
    nextUrl2(url) {
      this.$router.push({ path: url });
      sessionStorage.setItem("dataForm", JSON.stringify(this.dataForm));
      sessionStorage.setItem("zbDataform", JSON.stringify(this.zbDataform));
    },
    authListFun() {
      this.authList.push({
        //authList模板
        name: null, // 确权文件名称
        custConfirmTime: null, // 行方确权日期
        confirmStartCycle: this.dataForm.startCycle, //确权开始周期
        confirmEndCycle: this.dataForm.endCycle, //确权结束周期
        incomeEstimate: null, //估算说明
        enclosureUrl: [], // 客户验收报告或者附件list
        onlineReport: [], // 上线报告
        confirmWorkload: null, // 工作量
        amount: "0", // 确权金额
        preCustConfirmTime: null, //预计客户确权时间
      });
      this.authList = this.authList;
    },
    //删除确权数据
    deleteAuth(event, index) {
      event.stopPropagation();
      event.preventDefault();
      this.authList.splice(index, 1);
      this.authList = this.authList;
      sessionStorage.setItem("authList", JSON.stringify(this.authList));
    },
    //删除拆分数据
    deleteSpilit(event, index) {
      event.stopPropagation();
      event.preventDefault();
      this.projectSplitList.splice(index, 1);
      this.projectSplitList = this.projectSplitList;
      sessionStorage.setItem(
        "projectSplitList",
        JSON.stringify(this.projectSplitList)
      );
    },
    splitListFun() {
      this.projectSplitList = this.projectSplitList.concat({
        //authList模板
        projectName: null,
        projectNum: null,
        projectId: null,
        Workload: 0,
        projectAmount: 0,
      });
    },
    //查询是否是历史记录
    queryConfirmLatestRecord() {
      Indicator.open();
      httpServer(
        {
          url: "./contract_confirm_computer/queryConfirmLatestRecord",
          method: "post",
        },
        {}
      ).then((res) => {
        if (res.data.code == 0) {
          let bool, workOrder;
          if (sessionStorage.getItem("inquiryFlag")) {
            bool = JSON.parse(sessionStorage.getItem("inquiryFlag"));
          }
          if (sessionStorage.getItem("workOrder")) {
            workOrder = JSON.parse(sessionStorage.getItem("workOrder"));
          }
          if (
            !res.data.data ||
            (res.data.data && bool) ||
            (res.data.data && workOrder)
          ) {
            //没有历史记录的话对于用户来说是空白页面，用于用户的填写及操作
            //获取用户登录信息
            if (localStorage.getItem("userInfo_HETONGQUEQUAN")) {
              let userInfo = JSON.parse(
                localStorage.getItem("userInfo_HETONGQUEQUAN")
              );
              this.dataForm.createUserId = userInfo.userid; // 获取登录用户id
            }
            //跳页存储dataForm
            if (sessionStorage.getItem("dataForm")) {
              this.dataForm = JSON.parse(sessionStorage.getItem("dataForm"));
            }
            //盖章类型
            if (sessionStorage.getItem("signetList")) {
              this.signetList = JSON.parse(
                sessionStorage.getItem("signetList")
              );
            }
            //审批人
            if (sessionStorage.getItem("approverList")) {
              this.approverList = JSON.parse(
                sessionStorage.getItem("approverList")
              );
            }
            //通知人
            if (sessionStorage.getItem("noticeList")) {
              this.noticeList = JSON.parse(
                sessionStorage.getItem("noticeList")
              );
            }
            //确权数据
            if (sessionStorage.getItem("authList")) {
              this.authList = JSON.parse(sessionStorage.getItem("authList"));
            }
            //项目数据
            if (sessionStorage.getItem("projectInfo")) {
              this.projectInfo = JSON.parse(
                sessionStorage.getItem("projectInfo")
              );
              // 查询项目关联的销售合同
              this.saleContractInfo = this.projectInfo.contractList[0];
              if (this.saleContractInfo) {
                if (
                  this.saleContractInfo.contractType == "0" ||
                  this.saleContractInfo.contractType == "6" ||
                  this.saleContractInfo.contractType == "7" ||
                  this.saleContractInfo.contractType == "8"
                ) {
                  if (this.saleContractInfo.contractType == "6") {
                    this.dataForm.subContractTypeName =
                      init.saleaccountingType[
                      this.saleContractInfo.subContractType
                      ];
                  } else {
                    this.dataForm.subContractTypeName =
                      init.accountingType[
                      this.saleContractInfo.subContractType
                      ];
                  }
                }
                this.saleContractInfo.planAggregateAmount =
                  this.saleContractInfo &&
                    this.saleContractInfo.planAggregateAmount
                    ? this.saleContractInfo.planAggregateAmount
                    : 0;
                this.dataForm.contractStatusName =
                  init.contractStatue[this.saleContractInfo.state];
                this.dataForm.contractStatus = this.saleContractInfo.state;
                if (
                  this.dataForm.contractStatus == "2" ||
                  this.dataForm.contractStatus == "-1"
                ) {
                } else {
                  this.dataForm.isSeal = 0;
                  this.dataForm.isSealname = "否";
                }
                this.queryPlan();
                this.getapproveUser(); //获取审批人
              }

              // 查询项目关联合同
              this.queryContract();
              // this.projectInfo.id ? this.queryOrderListByCondition() : ''
            }
            //去掉项目改为销售合同信息
            if (sessionStorage.getItem("newSalesContractInfo")) {
              let newSalesContractInfo = JSON.parse(
                sessionStorage.getItem("newSalesContractInfo")
              );
              if (newSalesContractInfo) {
                this.newSalesContractInfo = newSalesContractInfo;
              }
              this.saleContractInfo = this.newSalesContractInfo;
              if (this.saleContractInfo) {
                if (
                  this.saleContractInfo.contractType == "0" ||
                  this.saleContractInfo.contractType == "6" ||
                  this.saleContractInfo.contractType == "7" ||
                  this.saleContractInfo.contractType == "8"
                ) {
                  if (this.saleContractInfo.contractType == "6") {
                    this.dataForm.subContractTypeName =
                      init.saleaccountingType[
                      this.saleContractInfo.subContractType
                      ];
                  } else {
                    this.dataForm.subContractTypeName =
                      init.accountingType[
                      this.saleContractInfo.subContractType
                      ];
                  }
                }
                this.saleContractInfo.planAggregateAmount =
                  this.saleContractInfo &&
                    this.saleContractInfo.planAggregateAmount
                    ? this.saleContractInfo.planAggregateAmount
                    : 0;
                this.dataForm.contractStatusName =
                  init.contractStatue[this.saleContractInfo.state];
                this.dataForm.contractStatus = this.saleContractInfo.state;
                if (
                  this.dataForm.contractStatus == "2" ||
                  this.dataForm.contractStatus == "-1"
                ) {
                } else {
                  this.dataForm.isSeal = 0;
                  this.dataForm.isSealname = "否";
                }
              }
              this.queryPlan();
              this.getapproveUser(); //获取审批人
              // 查询项目关联合同
              //this.queryContract();
              //会查询真实子合同信息
              this.queryRealSonContractList(this.saleContractInfo.id);
            }

            //查询选择真实子合同的信息
            if (sessionStorage.getItem("realSubContractInfo")) {
              this.realSubContractInfo = JSON.parse(
                sessionStorage.getItem("realSubContractInfo")
              );
            }
            //订单数据
            if (sessionStorage.getItem("order_infor")) {
              let order_infor = JSON.parse(
                sessionStorage.getItem("order_infor")
              );
              this.dataForm.orderId = order_infor.id;
              this.dataForm.orderName = order_infor.orName;
              this.dataForm.orderNum = order_infor.orNum;
            }
            //拆分数据
            if (sessionStorage.getItem("projectSplitList")) {
              this.projectSplitList = JSON.parse(
                sessionStorage.getItem("projectSplitList")
              );
            }
            //查询订单列表
            if (sessionStorage.getItem("selectedworkList")) {
              this.checkworkList = JSON.parse(
                sessionStorage.getItem("selectedworkList")
              );
            }
            //查询添加完订单后的订单信息以及项目收入拆分信息
            this.checkworkList && this.checkworkList.length > 0
              ? this.queryWorkOrderByABvalue()
              : "";
          } else if (workOrder) {
            let data = res.data.data ? res.data.data : null;
            this.assignment(data);
          }
          // else if(!workOrder){}
          else {
            MessageBox.confirm("是否复用上次发起的申请内容？")
              .then((action) => {
                let data = res.data.data ? res.data.data : null;
                this.assignment(data);
              })
              .catch((error) => {
                this.inquiryFlag = false;
                sessionStorage.setItem("workOrder", true);
                Object.assign(this.$data, this.$options.data.call(this));
                this.dataForm.confirmationTypeName = "客户确权";
                this.dataForm.confirmationType = "0";
              });
          }
        } else {
          MessageBox.alert(res.data.msg, "提示");
        }
        Indicator.close();
      });
    },
    //根据历史记录赋值
    assignment(data) {
      let storageContent = data && JSON.parse(data.storageContent);
      this.storageContent = storageContent;
      this.inquiryFlag = true;
      httpServer(
        { method: "POST", url: "./contract_confirm_computer/queryProject" },
        { projectName: storageContent.projectName, pageNo: 1, pageSize: 50 }
      ).then((res) => {
        if (res.data.code == "0") {
          this.projectInfo = res.data.data ? res.data.data[0] : null;
          sessionStorage.setItem(
            "projectInfo",
            JSON.stringify(this.projectInfo)
          );
          // 查询项目关联的销售合同
          this.saleContractInfo =
            this.projectInfo &&
            this.projectInfo.contractList &&
            this.projectInfo.contractList[0];
          this.saleContractInfo.planAggregateAmount =
            this.saleContractInfo && this.saleContractInfo.planAggregateAmount
              ? this.saleContractInfo.planAggregateAmount
              : 0;
          if (this.saleContractInfo) {
            if (
              this.saleContractInfo.contractType == "0" ||
              this.saleContractInfo.contractType == "6" ||
              this.saleContractInfo.contractType == "7" ||
              this.saleContractInfo.contractType == "8"
            ) {
              if (this.saleContractInfo.contractType == "6") {
                this.dataForm.subContractTypeName =
                  init.saleaccountingType[
                  this.saleContractInfo.subContractType
                  ];
              } else {
                this.dataForm.subContractTypeName =
                  init.accountingType[this.saleContractInfo.subContractType];
              }
            }
            this.dataForm.contractStatusName =
              init.contractStatue[this.saleContractInfo.state];
            this.dataForm.contractStatus = this.saleContractInfo.state;
            if (
              this.dataForm.contractStatus == "2" ||
              this.dataForm.contractStatus == "-1"
            ) {
            } else {
              this.dataForm.isSeal = 0;
              this.dataForm.isSealname = "否";
            }
            this.queryPlan();
            this.getapproveUser(); //获取审批人
          }
          // 查询项目关联合同
          // this.queryContract()
          // this.projectInfo.id ? this.queryOrderListByCondition() : ''
        }
      });
      this.dataForm.startCycle = storageContent.startCycle;
      this.dataForm.endCycle = storageContent.endCycle;
      this.dataForm.confirmFileType = storageContent.confirmFileType;
      this.dataForm.confirmFileTypeName =
        init.rightFileType[storageContent.confirmFileType];
      this.dataForm.confirmcustomersType =
        storageContent.customerConfirmationType;
      this.dataForm.confirmcustomersTypeName =
        init.confirmcustomersType[storageContent.customerConfirmationType];
      if (storageContent.workDetailIncome) {
        this.workDetailIncome = storageContent.workDetailIncome;
      }
      if (storageContent.workDetailPerson) {
        this.workDetailPerson = storageContent.workDetailPerson;
      }
      if (storageContent.workDetailFile) {
        this.workDetailFile = storageContent.workDetailFile;
      }
      let arr = [];
      if (sessionStorage.getItem("selectedworkList")) {
        arr = JSON.parse(sessionStorage.getItem("selectedworkList"));
      }
      let bool = sessionStorage.getItem("workOrder");
      if (bool) {
        this.checkworkList = arr;
      } else {
        this.checkworkList = init.unique(storageContent.woes.concat(arr));
      }
      //查询添加完订单后的订单信息以及项目收入拆分信息
      this.checkworkList && this.checkworkList.length > 0
        ? sessionStorage.setItem(
          "selectedworkList",
          JSON.stringify(this.checkworkList)
        )
        : "";
      this.checkworkList && this.checkworkList.length > 0
        ? this.queryWorkOrderByABvalue()
        : "";
      this.authList = JSON.parse(storageContent.contractConfirm);
      this.authList.map((val, i) => {
        val.enclosureUrl = storageContent.fileList.map((item) => {
          item.annexUrl = item.fileAddress;
          return item;
        });
      });
      this.dataForm.comment = storageContent.otherTypeRemarks;
      this.dataForm.remark = storageContent.remark;
      this.dataForm.isProjectSplit = storageContent.isProjectSplit
        ? storageContent.isProjectSplit
        : null;
    },
    submit() {
      var id1 = [],
        id2 = [],
        id3 = [];
      for (let i = 0; i < this.approverList.length; i++) {
        id1.push(this.approverList[i].userid);
      }
      for (let i = 0; i < this.noticeList.length; i++) {
        id2.push(this.noticeList[i].userid);
      }
      var param = {};
      param.confirmationType = this.dataForm.confirmationType;
      param.customerConfirmationType = this.dataForm.confirmcustomersType;
      param.otherTypeRemarks = this.dataForm.comment;
      // param.projectName = this.projectInfo.projectName
      // param.projectNum = this.projectInfo.projectNum
      // param.projectId = this.projectInfo.id
      // param.projectManager = this.projectInfo.projectManagerName
      param.planId = this.dataForm.planId;
      param.startCycle = this.dataForm.startCycle;
      param.endCycle = this.dataForm.endCycle;
      param.yearResult = this.dataForm.yearResult; //所属业绩年
      param.confirmFileType = this.dataForm.confirmFileType;
      param.orderId = this.dataForm.orderId;
      param.orderName = this.dataForm.orderName;
      param.orderNum = this.dataForm.orderNum;

      let authList1 = this.authList.map((val, i) => {
        let json = {};
        for (let key in val) {
          if (key == "enclosureUrl" || key == "onlineReport") {
          } else if (
            key == "name" ||
            key == "confirmStartCycle" ||
            key == "confirmEndCycle" ||
            key == "amount"
          ) {
            json[key] = val[key];
          } else if (key == "incomeEstimate") {
            if (
              param.confirmationType == 1 &&
              (param.confirmFileType == 0 ||
                param.confirmFileType == 2 ||
                param.confirmFileType == 3)
            ) {
              json[key] = val[key];
            }
          } else if (key == "confirmWorkload") {
            if (param.confirmFileType != 1) {
              json[key] = val[key];
            }
          } else if (key == "custConfirmTime") {
            if (param.confirmationType == "0") {
              //客户确权
              json[key] = val[key];
            }
          } else {
            if (
              param.confirmFileType == 0 ||
              param.confirmFileType == 2 ||
              param.confirmFileType == 3
            ) {
              if (key == "confirmWorkload") {
                json[key] = val[key];
              }
            }
            json[key] = val[key];
          }
        }
        var arr = val.enclosureUrl.map((val) => val.annexUrl);
        json.enclosureUrl = arr.join(",");
        if (param.confirmFileType == 1) {
          val.onlineReport = val.enclosureUrl;
          var arr = val.onlineReport.map((val) => val.annexUrl);
          json.onlineReport = arr.join(",");
        }

        json.amount = json.amount.toString().replace(/,/g, "");
        return json;
      });
      this.authList.map((val, i) => {
        param.fileList = val.enclosureUrl.map((item) => {
          if (item.annexUrl) {
            let json = {
              fileAddress: item.annexUrl,
              fileName: item.fileName,
            };
            return json;
          }
        });
      });
      // 判断确权必填项
      for (let i = 0; i < authList1.length; i++) {
        if (param.confirmFileType == 0 || param.confirmFileType == 3) {
          for (let key in authList1[i]) {
            if (param.confirmationType == 0) {
              if (
                key == "amount" ||
                key == "confirmStartCycle" ||
                key == "confirmEndCycle" ||
                key == "preCustConfirmTime"
              ) {
              } else if (!authList1[i][key]) {
                MessageBox.alert("请完善确权信息", "提示");
                return false;
              }
            } else if (param.confirmationType == 1) {
              if (
                key == "enclosureUrl" ||
                key == "amount" ||
                key == "confirmStartCycle" ||
                key == "name" ||
                key == "confirmEndCycle"
              ) {
              } else if (!authList1[i][key]) {
                MessageBox.alert("请完善确权信息", "提示");
                return false;
              }
            }
          }
        } else if (param.confirmFileType == 1) {
          for (let key in authList1[i]) {
            if (param.confirmationType == 0) {
              if (
                key == "onlineReport" ||
                key == "amount" ||
                key == "confirmStartCycle" ||
                key == "confirmEndCycle" ||
                key == "preCustConfirmTime"
              ) {
              } else if (!authList1[i][key]) {
                MessageBox.alert("请完善确权信息", "提示");
                return false;
              }
            } else if (param.confirmationType == 1) {
              if (
                key == "onlineReport" ||
                key == "amount" ||
                key == "enclosureUrl" ||
                key == "confirmStartCycle" ||
                key == "confirmEndCycle" ||
                key == "name"
              ) {
              } else if (!authList1[i][key]) {
                MessageBox.alert("请完善确权信息", "提示");
                return false;
              }
            }
          }
        } else if (param.confirmFileType == 2) {
          for (let key in authList1[i]) {
            if (param.confirmationType == 0) {
              if (
                key == "confirmWorkload" ||
                key == "enclosureUrl" ||
                key == "confirmStartCycle" ||
                key == "confirmEndCycle" ||
                key == "preCustConfirmTime"
              ) {
              } else if (!authList1[i][key]) {
                MessageBox.alert("请完善确权信息", "提示");
                return false;
              }
            } else if (param.confirmationType == 1) {
              if (
                key == "confirmWorkload" ||
                key == "enclosureUrl" ||
                key == "confirmStartCycle" ||
                key == "confirmEndCycle" ||
                key == "name"
              ) {
              } else if (!authList1[i][key]) {
                MessageBox.alert("请完善确权信息", "提示");
                return false;
              }
            }
          }
        }
      }
      param.contractConfirm = JSON.stringify(authList1);
      // 审批人Id
      // param.approverId = id1.join(',')
      param.approverId = "0942302708731031";
      if (this.checkworkList && this.checkworkList.length > 0) {
      } else {
        MessageBox.alert("请完善订单信息", "提示");
        return false;
      }

      if (this.psInfo && this.psInfo.length > 0) {
      } else {
        MessageBox.alert("请完善项目收入拆分信息", "提示");
        return false;
      }

      if (this.dataForm.confirmationType == 0) {
        //客户确权
        param.workDetailFile = this.workDetailFile;
        param.deviationReason = this.deviationReason;
        param.workDetailIncome = this.workDetailIncome;
        param.workDetailPerson = this.workDetailPerson;
        param.contractName = this.saleContractInfo.name;
        param.contractNum = this.saleContractInfo.contractNum;
        // param.bankName = this.projectInfo.bankName
        param.subContractType = this.saleContractInfo.subContractType;
        param.contractStatus = this.saleContractInfo.state;
        param.contractId = this.saleContractInfo.id;
        param.woes = this.checkworkList; //订单信息
        //新增
        param.name = this.newSalesContractInfo.name; //销售合同名字
        param.contractNum = this.newSalesContractInfo.contractNum; //销售合同编号
        param.headOfSalesName = this.newSalesContractInfo.headOfSalesName; //销售负责人
        // 客户确权+客户验收报告隐藏项目拆分
        if (this.dataForm.confirmFileType != 1) {
          // 确权拆分
          param.isProjectSplit = this.dataForm.isProjectSplit;
          var splitTotal = 0,
            splitWorkload = 0;
          let list = this.projectSplitList.map((val, i) => {
            splitTotal += Number(
              val.projectAmount.toString().replace(/,/g, "")
            );
            splitWorkload += Number(val.Workload);
            let json = {
              projectId: val.projectId,
              projectAmount: val.projectAmount.toString().replace(/,/g, ""),
            };
            if (
              param.confirmFileType == 0 ||
              param.confirmFileType == 2 ||
              param.confirmFileType == 3
            ) {
              json = {
                projectId: val.projectId,
                projectAmount: Number(val.projectAmount),
                confirmWorkload: Number(val.Workload),
              };
            }
            if (
              (this.dataForm.subContractTypeName == "人月类" ||
                this.dataForm.subContractTypeName == "派单类") &&
              (this.dataForm.confirmFileType == 0 ||
                this.dataForm.confirmFileType == 2 ||
                this.dataForm.confirmFileType == 3)
            ) {
              json.avgSalary = (val.projectAmount / val.Workload).toFixed(2); //项目收入/工作量

              json.avgSalary = isNaN(
                json.avgSalary.toString().replace(/,/g, "")
              )
                ? 0
                : json.avgSalary;
            }
            return json;
          });
          param.contractProjectSplit = JSON.stringify(this.psInfo);
        }
      }
      // param.notificationsId = id2.join(',')
      param.notificationsId = "0942302708731031";
      param.remark = this.dataForm.remark;
      if (localStorage.getItem("userInfo_HETONGQUEQUAN")) {
        let userInfo = JSON.parse(
          localStorage.getItem("userInfo_HETONGQUEQUAN")
        );
        this.dataForm.createUserId = userInfo.userid; // 获取登录用户id
        param.createUserId = this.dataForm.createUserId;
      }

      if (this.dataForm.confirmationType == 0) {
        //客户确权
        for (let key in param) {
          if (
            key == "confirmationType" ||
            key == "projectName" ||
            key == "projectNum" ||
            key == "confirmFileType" ||
            key == "isProjectSplit" ||
            key == "approverId" ||
            key == "customerConfirmationType"
          ) {
          } else if (key == "planId") {
            if (param[key] || param[key] == "") {
            } else {
              MessageBox.alert("请完善信息", "提示");
              return false;
            }
          }
        }
        if (param.customerConfirmationType == "4") {
          if (
            !param.otherTypeRemarks ||
            param.otherTypeRemarks == "" ||
            typeof param.otherTypeRemarks == "undefined"
          ) {
            MessageBox.alert("请完善信息", "提示");
            return false;
          }
        }
      }
      if (this.preSplitList.length > 0) {
        let list = this.preSplitList.map((val, i) => {
          let json = {
            projectNum: val.projectNum,
            projectName: val.projectName,
            projectAmount: val.projectAmount.toString().replace(/,/g, ""),
          };
          if (val.confirmWorkload) {
            json.confirmWorkload = val.confirmWorkload;
          }
          if (val.avgSalary) {
            json.avgSalary = val.avgSalary.toString().replace(/,/g, "");
            json.avgSalary = isNaN(json.avgSalary) ? 0 : json.avgSalary;
          }
          return json;
        });
        param.contractProjectSplitListStr = JSON.stringify(this.psInfo);
        param.amountSum = this.preConfirmInfo.amount;
        if (this.preConfirmInfo.workload) {
          param.confirmWorkloadSum = this.preConfirmInfo.workload;
        }
      }

      if (this.deviationPercent) {
        param.deviationPercent = this.deviationPercent;
      }

      // 客户确权+客户验收报告不显示 默认传否
      if (
        this.dataForm.confirmationType == 0 &&
        this.dataForm.confirmFileType == 1
      ) {
        param.isProjectSplit = "否";
        // let list = [{
        //   projectName: this.projectInfo.projectName,
        //   projectNum: this.projectInfo.projectNum,
        //   projectId: this.projectInfo.id,
        //   Workload: this.WorkloadTotal,
        // }]
        param.contractProjectSplit = JSON.stringify(this.psInfo);
      }
      // 是否盖章信息
      param.isSeal = this.dataForm.isSeal;
      if (this.dataForm.isSeal == "1") {
        let arr = [];
        arr = this.signetList.filter((val, index) => {
          return val.checked ? val.id : "";
        });
        param.sealType = init.getseparated(arr);
        param.recipientInfo = this.dataForm.recipientInfo;
      }

      //是否有真实子合同信息
      if (this.realSubContractList && this.realSubContractList.length > 0) {
        if (JSON.stringify(this.realSubContractInfo) == "{}") {
          MessageBox.alert("请选择关联真实子合同", "提示");
          return false;
        } else {
          param.contractByRealSonVO = this.realSubContractInfo;
        }
      }

      Indicator.open();
      let url = "./contract_confirm_computer/insertContractConfirmApproval"; // test新
      httpServer({ method: "POST", url: url }, param).then((res) => {
        if (res.data.code == 0) {
          this.$router.push({
            path: "/result",
            query: { type: this.dataForm.confirmationType },
          });
        } else {
          MessageBox.alert(res.data.msg, "提示");
        }
        Indicator.close();
      });
    },

    //业务采购合同确权审批提交
    subcontractSubmit() {
      this.zbDataform.contractId = this.contractsubInfo.subContract.id;
      let type = {
        planId: "关联付款计划",
        confirmFileType: "确权文件类型",
        amount: "业务采购合同确权金额",
        sealType: "印章类型",
        subConfirmReason: "申请业务采购合同确权理由",
        projectConfirmState: "交付项目客户确权状态",
      };

      for (var key in this.zbDataform) {
        if (key == "planId" || key == "amount" || key == "subConfirmReason") {
          if (
            !this.zbDataform[key] ||
            this.zbDataform[key] == "" ||
            typeof this.zbDataform[key] == "undefined"
          ) {
            MessageBox.alert(`请完善${type[key]}数据信息`, "提示");
            return false;
          }
        }
        if (
          key == "confirmFileType" ||
          key == "sealType" ||
          key == "projectConfirmState"
        ) {
          if (this.zbDataform[key] == "0") {
          } else if (
            !this.zbDataform[key] ||
            this.zbDataform[key] == "" ||
            typeof this.zbDataform[key] == "undefined"
          ) {
            MessageBox.alert(`请完善${type[key]}数据信息`, "提示");
            return false;
          }
        }
      }
      if (this.zbDataform.confirmFileType == "0") {
        if (
          !this.zbDataform.confirmWorkload ||
          this.zbDataform.confirmWorkload == "" ||
          typeof this.zbDataform.confirmWorkload == "undefined"
        ) {
          MessageBox.alert("请完善业务采购合同确权工作量", "提示");
          return false;
        }
      }

      if (this.zbDataform.coFiles && this.zbDataform.coFiles.length > 0) {
      } else {
        MessageBox.alert("请上传确权文件", "提示");
        return false;
      }

      let params = Object.assign(this.zbDataform, {});
      params.amount = params.amount.toString().replace(/,/g, "");
      httpServer(
        {
          method: "POST",
          url: "/contract_confirm_computer/insertSubContractConfirmApproval",
        },
        params
      ).then((res) => {
        if (res.data.code == 0) {
          this.$router.push({
            path: "/result",
            query: { type: this.dataForm.confirmationType },
          });
        } else {
          MessageBox.alert(res.data.msg, "提示");
        }
        Indicator.close();
      });
    },
  },
};
</script>

<style lang="less" scoped>
.signetSelect {
  cursor: pointer;
  color: #999999;
  .checked {
    color: #fff !important;
    border: 1px solid #38adff !important;
    -webkit-box-shadow: 0 0 1px #38adff;
    box-shadow: 0 0 1px #38adff;
    background-color: #38adff;
  }
}

.bgwhite {
  background: #ffffff;
  .workorderAll {
    height: 40px;
    line-height: 40px;
    overflow: hidden;
    border-bottom: solid 1px #e2e2e2;
    p {
      padding-left: 10px;
      float: left;
      width: 50%;
    }
    .workorderqq {
      width: 50%;
    }
  }
}

@grayColor: #e2e2e2;
.container {
  .item {
    border-bottom: 1px solid @grayColor;
    padding: 5px 10px 0;
    background-color: #fff;
  }
  .tip {
    padding: 10px 0;
  }
  .tip-small {
    position: relative;
    top: -1px;
    z-index: 5;
    margin-top: -5px;
    background-color: #fff;
    padding-left: 15px;
    padding-bottom: 5px;
    font-size: 10px;
  }

  .percent-list {
    padding: 0 12px;
    line-height: 40px;
    background-color: #fff;
    border-bottom: 1px solid #e2e2e2;
  }
}

.curgreen {
  color: #38adff;
}

.workLoadstyle {
  color: #909399;
  font-weight: 500;
  text-align: center;
  background: rgb(242, 242, 242);
  height: 50px;
  line-height: 50px;
}

.workLoadstylesecond {
  text-align: center;
  height: 50px;
  line-height: 50px;
}

.bordertop {
  border-top: solid 1px rgb(242, 242, 242);
}

.borderbottom {
  border-bottom: solid 1px rgb(242, 242, 242);
}

.borderright {
  border-right: solid 2px rgb(242, 242, 242);
}

.borderstyle {
  margin: 2px 0;
}

.exportbtn {
  width: 150px;
  display: inline-block;
  margin: 10px 0 10px 20px;
  cursor: pointer;
}
</style>
