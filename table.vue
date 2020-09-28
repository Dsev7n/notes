<style lang="scss">
.school-result{
  display: flex;
  height: 100vh;
  width: 100vw;
  overflow: scroll;
  border-radius: 7px 7px 0 0;
  .row1 {
    position: sticky;
    top: 0;
    height: 55px;
    background-color: #3B97FF;
    color: #FFFFFF;
  }
  .fixed-col {
    position: sticky;
    left: 0;
    margin-top: 55px;
  }
  .row2 {
    height: 30px;
  }
  .row1, .row2 {
    flex: 1;
    font-size: 12px;
    display: flex;
    align-items: center;
    text-align: center;
    justify-content: center;
  }
  .fixed-col .row2 {
    width: 90px;
    color: #4F4F4F;
  }
  .fixed-col .row2:nth-of-type(2n+1), .scroll-col .row2:nth-of-type(2n) {
    background-color: #D9EAFF;
  }
  .fixed-col .row2:nth-of-type(2n), .scroll-col .row2:nth-of-type(2n+1){
    background-color: #EDF5FF;
  }
  .scroll-col .row2 {
    color: #242424;
  }
  .tick {
    width: 13px;
    height: 10px;
  }
  .top-left {
    position: fixed;
    width: 90px;
    z-index: 2002;
    border-radius: 7px 0 0 0;
  }
  .cell2 {
    width: calc((100vw - 90px) / 2);
  }
  .cell3 {
    width: calc((100vw - 90px) / 3);
  }
}
</style>

<template>
  <div class="school-result">
    <!-- <v-table
      is-horizontal-resize
      style="width:100%"
      :columns="columns"
      :table-data="tableData"
      row-hover-color="#eee"
      row-click-color="#edf7ff"
      :height="600"
    ></v-table> -->
    <div class="fixed-col">
      <!-- <div class="row1">院校名称</div> -->
      <template v-if="tab=='院校'">
        <div class="row2" v-for="(item, index) in schoolHeader" :key="index">{{ item }}</div>
      </template>
      <template v-if="tab=='专业'">
        <div class="row2" v-for="(item, index) in majorHeader" :key="index">{{ item }}</div>
      </template>
    </div>
    <div class="top-left row1">{{ tab == '院校' ? '院校名称' : '专业名称' }}</div>
    <div class="scroll-col" v-for="(item, index) in tableData" :key="index">
      <div :class="widthObj" class="row1">{{ item.majorName }}</div>
      <div :class="widthObj" class="row2" v-for="(k, i) in majorKey" :key="i">{{ item[k] ? item[k] : '-' }}</div>
    </div>
  </div>
</template>

<script>
import { compareSchool } from '@/api/school'
import { compareMajor } from '@/api/major'
import { getQueryString } from '@/utils/tools'
import axios from 'axios'

export default {
  data() {
    return {
      selectArr: [],
      tableData: [],
      columns: [
        {field: 'name', title: '院校名称', width: 80, titleAlign: 'center', columnAlign: 'center',isResize:true,  isFrozen: true},
        {field: 'tel', title: '创办时间', width: 150, titleAlign: 'center', columnAlign: 'center',isResize:true},
        {field: 'hobby', title: '院校类型', width: 150, titleAlign: 'center', columnAlign: 'center',isResize:true},
        {field: 'address', title: '地址', width: 280, titleAlign: 'center', columnAlign: 'left',isResize:true}
      ],
      province: '广东',
      userId: 242233,
      tab: '',
      education: '本科',
      majorKey: ['majorCode', 'majorType', 'subject', 'level', 'schoolYear', 'degree', 'graduateNum', 'wenLikeRage', 'boyGirlRate', 'avgSalary', 'salaryRanking', 'subjectRanking', 'jobRate'],
      majorHeader: ['专业代码', '专科门类', '专业大类', '层次', '学制', '学位', '毕业生规模/人', '文理科比例/%', '男女生比例/%', '平均薪酬/元', '所有专业 薪酬排名', '本学科 薪酬排名', '最新就业率/%'],
      schoolHeader: ['创办时间', '省份', '办学性质', '院校类型', '办学层次', '主管部门', '专任教师/人', '全日制学生/人', '生师比', '985', '211', '双一流', '综合排名', '博士点/个', '专业硕士点/个', '一流学科/个', '学科评估A级/个', '学科评估B级/个', '学科评估C级/个', '院士/含双聘', '国家重点学科', '省重点学科', '国家重点实验室/个', '省市重点实验室/个', '科研经费/亿元', '优势专业/个', '就业率', '深造率', '男女比例', '理科投档院校排名', '理科招生人数', '理科投档分', '理科投档排位', '文科投档院校排名', '文科招生人数', '文科投档分', '文科投档排位']
    }
  },
  created() {
    const obj = getQueryString()
    this.selectArr = obj.arr.split(',')
    this.tab = obj.tab
    this.getTableData()
  },
  computed: {
    widthObj: function() {
      const len = this.tableData.length
      return {
        'cell2': len == 2,
        'cell3': len >= 3
      }
    }
  },
  methods: {
    getTableData() {
      const reqList = []
      this.selectArr.forEach((item) => {
        if (this.tab == '院校') {
          reqList.push(compareSchool(item, this.province))
        }
        if (this.tab == '专业') {
          reqList.push(compareMajor(item, this.education))
        }
      })
      axios.all(reqList).then(res => {
        res.forEach((resItem) => {
          if (resItem.data) {
            this.tableData.push(resItem.data)
          }
        })
      })
    }
  }
}
</script>
