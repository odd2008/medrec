<template>
  <v-app id="medrec.cloveropen.com">
    <Basepage
      v-bind:dialogSuccess="dialogSuccess"
      v-bind:dialogError="dialogError"
      v-bind:dialogSuccessContent="dialogSuccessContent"
      v-bind:dialogErrorContent="dialogErrorContent"
    />
    <v-container>
      <div>
        <v-toolbar class="elevation-0">
          <v-toolbar-title>岫岩中医院工作报告</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-breadcrumbs :items="items"></v-breadcrumbs>
        </v-toolbar>
      </div>
      <!-- 按时间查询 时间选择器 -->
      <v-row>
        <v-col cols="8" sm="6" md="3">
          <v-menu
            v-model="menu1"
            :close-on-content-click="false"
            :nudge-right="40"
            transition="scale-transition"
            offset-y
            min-width="200px"
          >
            <template v-slot:activator="{ on }">
              <v-text-field
                v-model="dateBegin"
                label="开始时间"
                prepend-icon="event"
                readonly
                v-on="on"
              ></v-text-field>
            </template>
            <v-date-picker v-model="dateBegin" locale="zh-cn" @input="menu1 = false"></v-date-picker>
          </v-menu>
        </v-col>
        <v-col cols="8" sm="4" md="3">
          <v-menu
            v-model="menu2"
            :close-on-content-click="false"
            :nudge-right="40"
            transition="scale-transition"
            offset-y
            min-width="200px"
          >
            <template v-slot:activator="{ on }">
              <v-text-field v-model="dateEnd" label="结束时间" prepend-icon="event" readonly v-on="on"></v-text-field>
            </template>
            <v-date-picker v-model="dateEnd" height="100%" locale="zh-cn" @input="menu2 = false"></v-date-picker>
          </v-menu>
        </v-col>
        <!-- <v-col cols="8" sm="6" md="3">
          <v-text-field label="请输入住院号" outlined v-model="caseNo"></v-text-field>
        </v-col>-->
        <v-col cols="8" sm="6" md="2">
          <v-btn class="ma-2" outlined color="indigo" @click="selectWardDaily()">查 询</v-btn>
        </v-col>
        <v-col cols="8" sm="6" md="2">
          <v-btn class="ma-2" outlined color="indigo" @click="saveWardDaily()">保 存</v-btn>
        </v-col>
      </v-row>
      <v-simple-table dense class="mytable">
        <template>
          <thead>
            <tr></tr>
            <tr>
              <th class="text-center" rowspan="4">病房</th>
              <th class="text-center" rowspan="4">期末实有病床数</th>
              <th class="text-center" rowspan="1" colspan="12">住院动态</th>
              <th class="text-center" rowspan="4">期内实际开放总床日数</th>
              <th class="text-center" rowspan="4">期内实际占用总床日数</th>
              <th class="text-center" rowspan="4">出院者占用总日数</th>
              <th class="text-center" rowspan="4">平均开放病床数</th>
              <th class="text-center" rowspan="4">治愈率(%)</th>
              <th class="text-center" rowspan="4">治疗有效率(%)</th>
              <th class="text-center" rowspan="4">病死率(%)</th>
              <th class="text-center" rowspan="4">平均病床周转次数(次)</th>
              <th class="text-center" rowspan="4">平均病床工作日(日)</th>
              <th class="text-center" rowspan="4">实际病床使用率(%)</th>
              <th class="text-center" rowspan="4">出院者平均住院日数(日)</th>
              <th class="text-center" rowspan="4">说明</th>
            </tr>
            <tr>
              <th class="text-center" rowspan="3">期初原有人数</th>
              <th class="text-center" rowspan="3">期内入院人数</th>
              <th class="text-center" rowspan="3">他科转入人数</th>
              <th class="text-center" rowspan="1" colspan="7">期内出院人数</th>
              <th class="text-center" rowspan="3">转往他科人数</th>
              <th class="text-center" rowspan="3">期末实有人数</th>
            </tr>
            <tr>
              <th class="text-center" rowspan="2" colspan="1">总计</th>
              <th class="text-center" rowspan="1" colspan="5">出院病人数</th>
              <th class="text-center" rowspan="2" colspan="1">其他</th>
            </tr>
            <tr>
              <th class="text-center" rowspan="2" colspan="1">小计</th>
              <th class="text-center" rowspan="2" colspan="1">治愈</th>
              <th class="text-center" rowspan="2" colspan="1">好转</th>
              <th class="text-center" rowspan="2" colspan="1">未愈</th>
              <th class="text-center" rowspan="2" colspan="1">死亡</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in wardDaily" :key="item.seq">
              <td class="text-center">{{item.ward}}</td>
              <td class="text-center">{{item.recordDate}}</td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.openBedsNum"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.originalNum"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.admissionNum"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.otherDeptShift"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.outNum"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.cure"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.becomeBetter"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.healed"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.death"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.terminal"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.invalid"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.untreated"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.subtotal"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.noDiseaseNum"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.normalLabor"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.nulliparousDischarge"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.familyPlanning"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.otherOccupyingBed"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.patientOccupyingBed"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.currentNum"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.shiftDept"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.accompanyNum"></v-text-field>
              </td>
            </tr>
          </tbody>
        </template>
      </v-simple-table>
      <div :style="style">
        <v-btn small class="success" @click="goback()">返回上一页</v-btn>
        <Medrecinfo
          v-bind:desserts="p_desserts"
          v-bind:desserts2="p_desserts2"
          v-bind:medrecInfo="medrecInfo"
          v-bind:medrecCost="medrecCost"
        />
      </div>
    </v-container>
  </v-app>
</template>
<script>
import Basepage from "../components/Basepage";
export default {
  components: {
    Basepage
  },
  data: () => ({
    dateBegin: new Date().toISOString().substr(0, 10),
    dateEnd: new Date().toISOString().substr(0, 10),
    menu1: false,
    menu2: false,
    wardDaily: [],
    dialogSuccess: false,
    dialogError: false,
    dialogSuccessContent: "",
    dialogErrorContent: "",
    caseNo: "",
    style: "display:none;",
    style1: ""
  }),
  mounted: function() {
    //获取开始时间 取当前月的前一个月的一号
    let sel = this;
    var today = new Date();
    var year = today.getFullYear();
    var month = today.getUTCMonth();
    var day = today.getDate();
    if (month.toString().length == 1) {
      month = "0" + month;
    }
    if (day.toString().length == 1) {
      day = "0" + day;
    }
    sel.dateBegin = year + "-" + month + "-" + day;
  },
  methods: {
    selectWardDaily() {
      let sel = this;
      let tin = sel.dateBegin + "|" + sel.dateEnd;
      fetch(process.env.VUE_APP_MAIN_URL + "getWardDaily/" + tin, {
        method: "get",
        mode: "cors",
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json"
        }
      })
        .then(function(response) {
          if (!response.ok) {
            sel.loginmsg = "病案查询失败" + response.err;
          }
          return response.json();
        })
        .then(function(data) {
          let topstatus = data.resultCode;
          if (topstatus == "0") {
            sel.wardDaily = JSON.parse(data.outdata);
          } else {
            sel.dialogError = true;
            sel.dialogErrorContent = data.errorMsg;
          }
        })
        .catch(function() {
          sel.loginmsg = "病案查询失败";
        });
    },
    openMedrec(case_no) {
      let sel = this;
      fetch(process.env.VUE_APP_MAIN_URL + "medrec/" + case_no, {
        method: "get",
        mode: "cors",
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json"
        }
      })
        .then(function(response) {
          if (!response.ok) {
            window.alert("查询失败error");
            sel.loginmsg = "查询失败" + response.err;
          }
          return response.json();
        })
        .then(function(data) {
          //console.log("data=" + JSON.stringify(data)); // this will be a string
          let topstatus = data.resultCode;
          if (topstatus == "0") {
            sel.style1 = "display:none;";
            sel.style = "";
            sel.medrecInfo = JSON.parse(data.outdata).medrecInfo;
            sel.medrecCost = JSON.parse(data.outdata).medrecCost;
            sel.p_desserts = JSON.parse(data.outdata).medrecDiag;
            sel.p_desserts2 = JSON.parse(data.outdata).medrecOpers;
          } else {
            sel.dialogError = true;
            sel.dialogErrorContent = data.errorMsg;
          }
        })
        .catch(function(err) {
          sel.dialogError = true;
          sel.dialogErrorContent = "请求异常：" + err;
        });
    },
    goback() {
      let sel = this;
      sel.style = "display:none;";
      sel.style1 = "";
    }
  }
};
</script>
<style>
.mytable table tr th,
td {
  border: 1px solid rgb(35, 155, 155);
  padding: 0 0 0 0;
  margin: 0 0 0 0;
}
.mytable th {
  background-color: rgb(236, 225, 61);
}
</style>
