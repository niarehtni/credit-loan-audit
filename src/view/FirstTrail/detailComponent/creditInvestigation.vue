<template>
  <!-- 实地征信=  页面 -->
  <div class="creditInvestigation">
    <el-collapse v-model="activeNames">
      <el-collapse-item name="1">
        <template slot="title">
          <i class="collapse_title_icon"></i>
          <span class="collapse_title_text">实地调查记录</span>
        </template>
        <div class="checkedInf checkedInf_li_width_half clearFix">
          <ul>
            <li>
              <label class="label_width_200">日期：</label>
              <span>{{datas.creditDate}} </span>
            </li>
            <li>
              <label class="label_width_200">实地人员1：</label>
              <span>{{bb}} </span>
            </li>
            <li>
              <label class="label_width_200">是否核对借款人有效证件：</label>
              <span>{{datas.IsCheckDocTxt}} </span>
            </li>
            <li>
              <p v-show="datas.IsCheckDocTxt=='否'">
                <label class="label_width_200">不核对借款人有效证件原因：</label>
                <el-tooltip class="item" effect="dark" :content="datas.notCheckBorrowerDocReason" :disabled="datas.notCheckBorrowerDocReason==null"
                  placement="top">
                  <span>{{datas.notCheckBorrowerDocReason}}</span>
                </el-tooltip>
              </p>
            </li>
            <li>
              <label class="label_width_200">实地场所：</label>
              <span>{{aa}} </span>
            </li>
            <li>
              <label class="label_width_200">实地发起源：</label>
              <span>{{cc}} </span>
            </li>
          </ul>
        </div>
      </el-collapse-item>
      <el-collapse-item name="2">
        <template slot="title">
          <i class="collapse_title_icon"></i>
          <span class="collapse_title_text">家庭实地征信内容</span>
        </template>
        <div class="checkedInf checkedInf_li_width_half clearFix">
          <ul>
            <li style="width:100%;">
              <label class="label_width_200">地址：</label>
              <span>{{datas.homeAddr}}</span>
            </li>
            <li>
              <label class="label_width_200">标志性建筑：</label>
              <span>{{datas.homeLandmark}}</span>
            </li>
            <li>
              <label class="label_width_200">路线：</label>
              <span>{{datas.homeRoute}}</span>
            </li>
            <li>
              <label class="label_width_200">是否核对借款人房产证件：</label>
              <span>{{datas.checkEstateLicenseReason}}</span>
            </li>
            <li>
              <p v-show="datas.checkEstateLicenseReason == '否'">
                <label class="label_width_200">不核对借款人房产证件原因：</label>
                <el-tooltip class="item" effect="dark" :content="datas.homeHouseInfo" :disabled="datas.homeHouseInfo==null"
                  placement="top">
                  <span>{{datas.homeHouseInfo}}</span>
                </el-tooltip>
              </p>
            </li>
            <li>
              <label class="label_width_200">在场人员：</label>
              <span>{{datas.homePresence}}</span>
            </li>
            <li>
              <label class="label_width_200">与借款人关系：</label>
              <span>{{datas.homeBorrowerReleation}}</span>
            </li>
            <li>
              <label class="label_width_200">社区周边环境：</label>
              <span>{{datas.homeSurrounding}}</span>
            </li>
            <li>
              <label class="label_width_200">房屋户型：</label>
              <span>{{datas.homeHouseSizeTxt}}</span>
            </li>
            <li>
              <label class="label_width_200">房屋建筑年代：</label>
              <span>{{datas.homeBuildingAge}}</span>
            </li>
          </ul>
        </div>
      </el-collapse-item>
      <el-collapse-item name="3">
        <template slot="title">
          <i class="collapse_title_icon"></i>
          <span class="collapse_title_text">日常生活迹象</span>
        </template>
        <div class="checkedInf checkedInf_li_width_half clearFix">
          <ul>
            <li>
              <label class="label_width_200">起居室：</label>
              <span>{{datas.homeLivingRoom}}</span>
            </li>
            <li>
              <label class="label_width_200">卫生间：</label>
              <span>{{datas.homeToilet}}</span>
            </li>
            <li>
              <label class="label_width_200">厨房：</label>
              <span>{{datas.homeKichen}}</span>
            </li>
            <li>
              <label class="label_width_200">卫生清洁：</label>
              <span>{{datas.homeHygiene}}</span>
            </li>
            <li>
              <label class="label_width_200">其他：</label>
              <span>{{datas.homeOtherDailyLife}}</span>
            </li>
          </ul>
        </div>
      </el-collapse-item>
      <el-collapse-item name="4">
        <template slot="title">
          <i class="collapse_title_icon"></i>
          <span class="collapse_title_text">其他</span>
        </template>
        <div class="checkedInf checkedInf_li_width_half clearFix">
          <ul>
            <li>
              <label class="label_width_200">植物：</label>
              <span>{{datas.homeBotany}}</span>
            </li>
            <li>
              <label class="label_width_200">宠物：</label>
              <span>{{datas.homePet}}</span>
            </li>
            <li>
              <label class="label_width_200">货物：</label>
              <span>{{datas.homeGoods}}</span>
            </li>
            <li>
              <label class="label_width_200">异常说明：</label>
              <span>{{datas.homeExceptionRemark}}</span>
            </li>
            <li>
              <label class="label_width_200">其他生活情况：</label>
              <span>{{datas.homeOtherLiving}}</span>
            </li>
          </ul>
        </div>
      </el-collapse-item>
      <el-collapse-item name="5">
        <template slot="title">
          <i class="collapse_title_icon"></i>
          <span class="collapse_title_text">企业实地征信内容</span>
        </template>
        <div class="checkedInf checkedInf_li_width_half clearFix">
          <ul>
            <li style="width:100%;">
              <label class="label_width_200">企业征信地址：</label>
              <span>{{datas.compAddr}}</span>
            </li>
            <li>
              <label class="label_width_200">是否查验企业有效证件：</label>
              <span>{{datas.compCheckDocFlag}}</span>
            </li>
            <li>
              <label class="label_width_200">各项材料原件是否收齐比对：</label>
              <span>{{datas.creditDate}}</span>
            </li>
            <li>
              <label class="label_width_200">企业是否存在：</label>
              <span>{{datas.creditDate}}</span>
            </li>
          </ul>
        </div>
      </el-collapse-item>
      <el-collapse-item name="6">
        <template slot="title">
          <i class="collapse_title_icon"></i>
          <span class="collapse_title_text">办公环境</span>
        </template>
        <div class="checkedInf checkedInf_li_width_half clearFix">
          <ul>
            <li>
              <label class="label_width_200">企业水牌位置及新旧程度：</label>
              <span>{{datas.compSignPlace}}</span>
            </li>
            <li>
              <label class="label_width_200">办公场所装修情况：</label>
              <span>{{datas.compOfficeDecora}}</span>
            </li>
            <li>
              <label class="label_width_200">办公场所面积：</label>
              <span>{{datas.compSignPlace}}</span>
            </li>
          </ul>
        </div>
      </el-collapse-item>
      <el-collapse-item name="7">
        <template slot="title">
          <i class="collapse_title_icon"></i>
          <span class="collapse_title_text">库房/车间</span>
        </template>
        <div class="checkedInf checkedInf_li_width_half clearFix">
          <ul>
            <li>
              <label class="label_width_200">机器设备：</label>
              <span>{{datas.compEquipment}}</span>
            </li>
            <li>
              <label class="label_width_200">厂房面积：</label>
              <span>{{datas.compArea}}</span>
            </li>
            <li>
              <label class="label_width_200">现代化程度：</label>
              <span>{{datas.compModenLevel}}</span>
            </li>
            <li>
              <label class="label_width_200">办公区工位数情况：</label>
              <span>{{ee}}</span>
            </li>
            <li>
              <label class="label_width_200">库存：</label>
              <span>{{datas.compStock}}</span>
            </li>
            <li>
              <label class="label_width_200">工位上是否有电话：</label>
              <span>{{ff}}</span>
            </li>
          </ul>
        </div>
      </el-collapse-item>
      <el-collapse-item name="8">
        <template slot="title">
          <i class="collapse_title_icon"></i>
          <span class="collapse_title_text">员工</span>
        </template>
        <div class="checkedInf checkedInf_li_width_half clearFix">
          <ul>
            <li>
              <label class="label_width_200">是否开工中：</label>
              <span>{{datas.compIsStart}}</span>
            </li>
            <li>
              <label class="label_width_200">见到办公人数：</label>
              <span>{{datas.compWorkerStationNum}}</span>
            </li>
            <li>
              <label class="label_width_200">是否统一着装：</label>
              <span>{{datas.compUniformFlag}}</span>
            </li>
            <li>
              <label class="label_width_200">工位上是否有电话：</label>
              <span>{{hh}}</span>
            </li>
            <li>
              <label class="label_width_200">办公区工位数情况：</label>
              <span>{{gg}}</span>
            </li>
          </ul>
        </div>
      </el-collapse-item>
      <el-collapse-item name="9">
        <template slot="title">
          <i class="collapse_title_icon"></i>
          <span class="collapse_title_text">口述企业状况</span>
        </template>
        <div class="checkedInf checkedInf_li_width_half clearFix">
          <ul>
            <li style="width:100%;">
              <label class="label_width_200">规模：</label>
              <span>{{datas.compScale}}</span>
            </li>
            <li>
              <label class="label_width_200">经营年限：</label>
              <span>{{datas.compOperaYears}}</span>
            </li>
            <li>
              <label class="label_width_200">年收支[万元]：</label>
              <span>{{datas.compPay}}</span>
            </li>
            <li>
              <label class="label_width_200">毛利[万元]：</label>
              <span>{{datas.compGroosmargin}}</span>
            </li>
            <li>
              <label class="label_width_200">纯利[万元]：</label>
              <span>{{datas.compNetProfit}}</span>
            </li>
            <li>
              <label class="label_width_200">上下游供货方式：</label>
              <span>{{datas.compSupplyMode}}</span>
            </li>
            <li>
              <label class="label_width_200">上下游结算方式：</label>
              <span>{{datas.compCaculation}}</span>
            </li>
            <li style="width:100%;">
              <label class="label_width_200">上下游结算周期：</label>
              <span>{{datas.compBillcycle}}</span>
            </li>
            <li>
              <label class="label_width_200">应收账款[万元]：</label>
              <span>{{datas.compCollection}}</span>
            </li>
            <li>
              <label class="label_width_200">应付账款[万元]：</label>
              <span>{{datas.compPayables}}</span>
            </li>
            <li style="width:100%;">
              <label class="label_width_200">贷款及抵押情况：</label>
              <span>{{datas.compLoan}}</span>
            </li>
            <li class="text_area_li">
              <label class="label_width_200">其他说明：</label>
              <el-tooltip class="item" effect="dark" :content="datas.compOtherRemark" :disabled="datas.compOtherRemark==null"
                placement="top">
                <span class="text_area_span text_area_span_minus220">{{datas.compOtherRemark}}</span>
              </el-tooltip>
            </li>
            <li class="text_area_li">
              <label class="label_width_200">异常说明：</label>
              <el-tooltip class="item" effect="dark" :content="datas.compExceptionRemark" :disabled="datas.compExceptionRemark==null"
                placement="top">
                <span class="text_area_span text_area_span_minus220">{{datas.compExceptionRemark}}</span>
              </el-tooltip>
            </li>
          </ul>
        </div>
      </el-collapse-item>
    </el-collapse>
  </div>
</template>
<script type="text/javascript">
  export default {
    data() {
      return {
        activeNames: ['1', '2', '3', '4', '5', '6', '7', '8', '9'],
        datas: '',
        aa: '',
        bb: '',
        cc: '',
        dd: '',
        ee: '',
        ff: '',
        gg: '',
        hh: '',
        taskInWaitting: '',
        judgeFlag: '',
      };
    },
    props: {
      applyId: {
        default: '',
        required: true,
        type: String
      }
    },
    mounted() {
      this.mountedInf();
    },
    methods: {
      mountedInf() {
        this.request();
      },
      request(query) {
        this.get("/credit/fieldReference", {
          applyId: this.applyId
        }).then(res => {
          if (res.statusCode == 200) {
            this.datas = res.data;
            //(企业)收支[万元]
            if (this.datas.compPay != null) {
              this.datas.compPay = this.formatNumber(this.datas.compPay, 2, 0);
            };
            //(企业)毛利[万元]
            if (this.datas.compGroosmargin != null) {
              this.datas.compGroosmargin = this.formatNumber(this.datas.compGroosmargin, 2, 0);
            };
            //(企业)纯利[万元]
            if (this.datas.compNetProfit != null) {
              this.datas.compNetProfit = this.formatNumber(this.datas.compNetProfit, 2, 0);
            };
            //(企业)应收账款[万元]
            if (this.datas.compCollection != null) {
              this.datas.compCollection = this.formatNumber(this.datas.compCollection, 2, 0);
            };
            //(企业)应付账款
            if (this.datas.compPayables != null) {
              this.datas.compPayables = this.formatNumber(this.datas.compPayables, 2, 0);
            };
            //办公环境 办公场所面积
            if (this.datas.compOfficeArea != null) {
              this.datas.compOfficeArea = this.formatNumber(this.datas.compOfficeArea, 2, 0).replace(/,/g, '') + '㎡';
            };
            //库房/车间 厂房面积
            if (this.datas.compArea != null) {
              this.datas.compArea = this.formatNumber(this.datas.compArea, 2, 0).replace(/,/g, '') + '㎡';
            };

          };
        });
      },
      //保留两位小数 整数千分位
      formatNumber(num, cent, isThousand) {
        num = num.toString().replace(/\$|\,/g, '');
        // 检查传入数值为数值类型
        if (isNaN(num))
          num = "0";
        // 获取符号(正/负数)
        let sign = (num == (num = Math.abs(num)));
        num = Math.floor(num * Math.pow(10, cent) + 0.50000000001); // 把指定的小数位先转换成整数.多余的小数位四舍五入
        let cents = num % Math.pow(10, cent); // 求出小数位数值
        num = Math.floor(num / Math.pow(10, cent)).toString(); // 求出整数位数值
        cents = cents.toString(); // 把小数位转换成字符串,以便求小数位长度
        // 补足小数位到指定的位数
        while (cents.length < cent)
          cents = "0" + cents;
        for (var i = 0; i < Math.floor((num.length - (1 + i)) / 3); i++)
          num = num.substring(0, num.length - (4 * i + 3)) + ',' + num.substring(num.length - (4 * i + 3));
        if (cent > 0) {
          if (sign == true) {
            return (((sign) ? '' : '-') + num + '.' + cents);
          } else if (sign == false) {
            return '0.00'
          }
        } else {
          return (((sign) ? '' : '-') + num);
        }
      },
    }
  }

</script>
<style type="text/css" scoped>
  .creditInvestigation {
    width: 100%;
    height: 100%;
    background-color: #fafbfc;
    font-size: 14px;
    min-width: 930px;
    color: #475669;
  }

  /* 头部 */

  .creditHead {
    width: 100%;

  }

  /* icon_hat */

  ul.headul {
    float: left;
    width: 50%;
  }

  ul.headul li {
    margin-top: 10px;
    height: 24px;
  }

  ul.headul li:last-child {
    margin-bottom: 10px;
  }

  ul.headul li label {
    width: 210px;
    display: inline-block;
    text-align: right;
  }

</style>
