<template>
  <div class="gantt-blocks" :class="extraOprStatus == false ? '' : 'gantt-blocks-extra'">
    <div class="gantt-block gantt-block-top-space" :style="{ height: topSpace + 'px' }"></div>
    <div class="gantt-block" v-for="(item, index) in showDatas" :key="index" :style="{ height: `${cellHeight}px` }">

      <div v-for="(itemChild, indexChild) in item.children"
           v-if="scrollLeft <= ((itemChild.start) / 100 * (cellWidth / 10)) + (itemChild.time / 1000 * cellWidth) && getPosition(itemChild) - scrollLeft < blockWidth"
           class="gantt-block-item"
           :style="{left: getPosition(itemChild) + 'px', width: `${ (itemChild.time/100) * (cellWidth/10)}px`,background: indexChild % 2 == 0 ? '#dddddd' : '#212121'}"
           style="height: 35px;top:2px;">

        <div class="moon-ellipsis-class font-size-12 textCenter">
          {{ itemChild.time }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import dr from "../../utils/dynamic-render";
import {isUndef} from "../../utils/utils";
export default {
  layout: 'default',
  mixins: [dr],
  props: {
    dataKey: String,
    itemKey: String,
    arrayKeys: {
      type: Array
    },
    scrollLeft: Number,
    cellWidth: {
      type: Number,
      required: true
    },
    blockWidth: {
      type: Number,
      required: false
    },
    scale: {
      type: Number,
      required: false
    },
    widthOfBlocksWrapper: {
      type: Number,
      required: true
    },
    extraOprStatus: {
      type: Boolean,
      default: false
    },
    endTimeOfRenderArea: [Number, null],
    startTimeOfRenderArea: [Number, null],
    getPositonOffset: Function,
    getWidthAbout2Times: Function,
    customGenerateBlocks: Boolean
  },
  components: {

  },
  computed: {

  },
  data() {
    return {
      timeOutEvent: 0,//定时器
      clickStatus: '',
      touchStatus: '',
      touchStart: 0,
      touchEnd: 0
    }
  },
  mounted() {

  },
  methods: {
    /**
     * 计算时间块偏移
     *
     * @param {{start:string}} block
     * @returns {number}
     */
    getPosition(block) {
      if (isUndef(block.start)) {
        return 0;
      }
      return block.start / 100 * this.cellWidth / 10;
    },
    showBlock(item, index){
      this.$emit("showBlock", item, index);
    },
    gotouchstart(item, index, itemChild, indexChild){
      let that = this;
      clearTimeout(this.timeOutEvent);//清除定时器
      this.timeOutEvent = 0;
      this.touchStart = new Date().getTime();
      this.timeOutEvent = setTimeout(() => {
        this.showItemBlock(item, index, itemChild, indexChild);
      },600);//这里设置定时
    },
    //手释放，如果在500毫秒内就释放，则取消长按事件，此时可以执行onclick应该执行的事件
    gotouchend(item, index, itemChild, indexChild){
      this.touchEnd = new Date().getTime();
      let time = this.touchEnd - this.touchStart;
      clearTimeout(this.timeOutEvent);
      if(this.timeOutEvent != 0 && time < 600){
        //这里写要执行的内容（尤如onclick事件）
        this.showBlock(item, index)
      }
    },
    //如果手指有移动，则取消所有事件，此时说明用户只是要移动而不是长按
    gotouchmove(){
      clearTimeout(this.timeOutEvent);//清除定时器
      this.timeOutEvent = 0;
    }
  }
}
</script>

<style scoped>
.gantt-blocks-wrapper {
  overflow: hidden;
}
.gantt-blocks {
  background-image: linear-gradient(#606060 1px, transparent 0), linear-gradient(90deg, #606060 1px, transparent 0);
}
.gantt-blocks-extra{
  background-image: linear-gradient(#505050 1px, transparent 0), linear-gradient(90deg, #505050 1px, transparent 0);
}
.gantt-block {
  position: relative;
  outline: 1px solid #606060;
}
.gantt-block-item {
  position: absolute;
  height: 100%;
  box-shadow: #808080 0px 0px 0px 1px;
}
.gantt-block-item:hover {
  position: absolute;
  height: 100%;
  box-shadow: #808080 0px 0px 0px 2px;
}
.gantt-scroll-y{
  overflow-y: scroll;
  position: absolute;
  z-index: 100;
  top: 0;
  right: 0;
}
.gantt-scroll-x{
  overflow-x: scroll;
  position: absolute;
  z-index: 1000;
  bottom: 0;
  left: 0;
}
.block-item-plus{
  position: absolute;
  top: 8px;
  right: -20px;
}
.block-item-empty-plus{
  position: absolute;
  top: 10px;
  left: 10px;
}
</style>
