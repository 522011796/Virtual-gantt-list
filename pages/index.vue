<template>
  <div class="container">
    <v-gantt-pro
      :scale="globalScal"
      :datas="dataTest"
      :gantt-data="dataTest"
      :gantt-col-data="ganttColData"
      :gantt-time-data="ganttTimeData"
      :scroll-to-postion="position"
      @scrollLeft="scrollLeftA">

      <div slot="title" class="textCenter">
        <span class="font-size-12 color-F2F6FC">{{$t("任务")}}</span>
      </div>
    </v-gantt-pro>
  </div>
</template>

<script>
import VGanttPro from "../components/gantt/VGanttPro";
import mixins from "../mixins/mixins";
export default {
  layout: 'default',
  mixins: [mixins],
  components: {
    VGanttPro

  },
  data() {
    return {
      tableHeader: {},
      tableData: [],
      dataTest: [],
      ganttData: [],
      ganttColData: [],
      ganttTimeData: [],
      position: 0,
      scrollToY: 0,
      positionA: 0,
    }
  },
  mounted() {
    this.height = window.innerHeight // 动态设置高度
    if (window.history && window.history.pushState) {
      // 向历史记录中插入了当前页
      history.pushState(null, null, document.URL);
      window.addEventListener('popstate', this.goBack, false);
    }
    this.init();
  },
  destroyed() {
    window.removeEventListener('popstate', this.goBack, false);
  },
  methods: {
    init(){
      let data = [];
      let dataCol = [];
      let dataTime = [];
      for (let i = 0; i < 50; i++){
        let testBar = [];
        for (let j = 0; j < i; j++){
          let start = j * 1000;
          let end = (j+1) * 1000;
          testBar.push({start: start,end: end,time: 1000,type: j % 2 == 0 ? 1 : 2});
        }
        data.push({
          time: i,
          menuTitle: i,
          children: testBar
        });
      }
      for (let i = 0; i < 50; i++){
        dataCol.push({
          time: i,
          start: 0,
          end: 1
        });
      }
      for (let i = 0; i < 50; i++){
        dataTime.push({
          time: i
        });
      }
      this.dataTest = data;
      this.ganttColData = dataCol;
      this.ganttTimeData = dataTime;
    },
    scrollLeftA(val) {
      this.position = { x: val };
    },
    goBack () {
      // console.log("点击了浏览器的返回按钮");
      history.pushState(null, null, document.URL);
    }
  }
}
</script>

<style scoped>
</style>
