<template>
  <v-app id="medrec.cloveropen.com">
    <Basepage
      v-bind:dialogSuccess="dialogSuccess"
      v-bind:dialogError="dialogError"
      v-bind:dialogSuccessContent="dialogSuccessContent"
      v-bind:dialogErrorContent="dialogErrorContent"
    />
    <v-container style="background-color:rgb(255,255,255);">
      <div>
        <v-toolbar class="elevation-0">
          <v-toolbar-title>医院门诊情况报表</v-toolbar-title>
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
          <v-btn class="ma-2" outlined color="indigo" @click="selectOutpDaily()">查 询</v-btn>
        </v-col>

        <v-col cols="8" sm="6" md="2">
          <v-btn class="ma-2" outlined color="indigo" @click="saveOutpDaily()">保 存</v-btn>
        </v-col>
        <!-- <v-col cols="8" sm="6" md="2">
          <v-btn class="ma-2" outlined color="indigo" @click="exportExcel()">测试导出excel</v-btn>
        </v-col>-->
      </v-row>
      <v-simple-table class="mytable2">
        <template>
          <thead>
            <tr></tr>
            <tr>
              <th class="text-center">        </th>
              <th class="text-center">总计(人)</th>
              <th class="text-center">内科(人)</th>
              <th class="text-center">外科(人)</th>
              <th class="text-center">骨科(人)</th>
              <th class="text-center">肛肠科(人)</th>
              <th class="text-center">康复理疗科(人)</th>
              <th class="text-center">妇科(人)</th>
              <th class="text-center">儿科(人)</th>
              <th class="text-center">耳鼻喉(人)</th>
              <th class="text-center">口腔科(人)</th>
              <th class="text-center">皮肤科(人)</th>
              <th class="text-center">中医科(人)</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in outpDaily" :key="item.seq" @dblclick="openMedrec(item.seq)">
              <td class="text-center">{{item.recordDate}}</td>
              <td class="text-center">{{item.dept}}</td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.workDays"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.firstVisitNum"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.subVisitNum"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.diagnosticNum"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.healthExam"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.observeStay"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.rescueNum"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.rescueSuccessNum"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.criticalRescueNum"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.emerRoomDeathNum"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.obserRoomDeathNum"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.registeredNum"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.freeRegistered"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.ordinaryNum"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.emergencyNum"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.expertNum"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.basicInsurance"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.commercialInsurance"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.other"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.outCall"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.familySickbed"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.district"></v-text-field>
              </td>
              <td class="text-center">
                <v-text-field class="abc" v-model="item.inpatientNum"></v-text-field>
              </td>
            </tr>
          </tbody>
        </template>
      </v-simple-table>
      <div :style="style">
        <v-btn small class="success" @click="goback()">返回上一页</v-btn>
        <Medrecinfo
          v-bind:desserts="pDesserts"
          v-bind:desserts2="pDesserts2"
          v-bind:medrecInfo="medrecInfo"
          v-bind:medrecCost="medrecCost"
        />
      </div>
    </v-container>
  </v-app>
</template>
<script>
import Basepage from "../components/Basepage";
import Medrecinfo from "../components/Medrecinfo";
export default {
  components: {
    Basepage,
    Medrecinfo
  },
  data: () => ({
    dateBegin: new Date().toISOString().substr(0, 10),
    dateEnd: new Date().toISOString().substr(0, 10),
    menu1: false,
    menu2: false,
    dialogSuccess: false,
    dialogError: false,
    dialogSuccessContent: "",
    dialogErrorContent: "",
    outpDaily: [],
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
    selectOutpDaily() {
      let sel = this;
      let tin = sel.dateBegin + "|" + sel.dateEnd;
      fetch(process.env.VUE_APP_MAIN_URL + "getOutpDaily/" + tin, {
        method: "get",
        mode: "cors",
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json"
        }
      })
        .then(function(response) {
          if (!response.ok) {
            sel.dialogError = true;
            sel.dialogErrorContent = "请求失败 " + response.err;
          }
          return response.json();
        })
        .then(function(data) {
          let topstatus = data.resultCode;
          if (topstatus == "0") {
            sel.outpDaily = JSON.parse(data.outdata);
          } else {
            sel.dialogError = true;
            sel.dialogErrorContent = data.errorMsg;
          }
        })
        .catch(function() {
          sel.loginmsg = "门诊日报失败";
        });
    },
    openMedrec(caseNo) {
      let sel = this;
      fetch(process.env.VUE_APP_MAIN_URL + "medrec/" + caseNo, {
        method: "get",
        mode: "cors",
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json"
        }
      })
        .then(function(response) {
          if (!response.ok) {
            sel.dialogError = true;
            sel.dialogErrorContent = "请求失败 " + response.err;
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
            sel.pDesserts = JSON.parse(data.outdata).medrecDiag;
            sel.pDesserts2 = JSON.parse(data.outdata).medrecOpers;
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
<style >
.mytable2 table tr th,
td {
  border: 1px solid rgba(194, 235, 216, 0.911);
  padding: 0 0 0 0;
  margin: 0 0 0 0;
}
.mytable2 th {
  background-color: rgb(81, 207, 144);
}
</style>
