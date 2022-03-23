<script>
    export default {
      name: "mixins",
      data(){
        return {
          globalDataKeys: [],
          minxinsScroll: false,
          globalCellWidth: 60,
          globalScal: 0,
          scnenDuration: '',
          contentDefaultStyle:{
            'height': '0px',
            'overflow-y': 'hiddle'
          },
          contentStyle:{
            'height': '0px',
            'overflow-y': 'auto'
          },
          contentWidthStyle:{
            'width': '0px',
            'overflow-y': 'auto'
          }
        }
      },
      mounted() {
        window.addEventListener('orientationchange', (e) => {
          if (process.browser) {
            if (window.orientation == 0 || window.orientation == 180){
              this.globalDrawerHeight = '40%';
              this.globalDrawerSheetHeight = '50%';
              this.globalDrawerBottomHeight = '45%';
              this.globalDrawerSubSheetHeight = '40%';
            }else if (window.orientation == 90 || window.orientation == -90){
              this.globalDrawerHeight = '60%';
              this.globalDrawerSheetHeight = '70%';
              this.globalDrawerBottomHeight = '70%';
              this.globalDrawerSubSheetHeight = '40%';
            }
            this.mainStyle();
          }
        });
      },
      created() {
        this.mainStyle();
        this.defaultStyle();
      },
      methods: {
        defaultStyle(){
          if (process.browser) {
            let screenWidth = window.innerWidth;
            this.contentDefaultStyle.height = window.innerHeight + 'px';
          }
        },
        mainStyle(){
          if (process.browser) {
            let screenWidth = window.innerWidth;
            let headHeight = this.appType == "app" ? 0 : 40;
            this.contentStyle.height = window.innerHeight - headHeight + 'px';
            this.contentWidthStyle.width = window.innerWidth + 'px';
          }
        },
        campareDataTime(data){
          this.globalDataKeys = data;
        },
        changeTime(time, num) {
          let hour = 0
          let minute = 0
          let second = 0
          second = time / 1000
          let ms = time % 1000;
          if (second >= 3600) {
            minute = (second - (second % 60)) / 60
            hour = parseInt((minute / 60).toString())
            minute = minute % 60
            /* eslint-disable */
            hour >= 10 ? hour : hour = '0' + hour
            minute >= 10 ? minute : minute = '0' + minute
            second = second % 60
            second >= 10 ? second : second = '0' + second
            /* eslint-enable */
            if (ms != 0 &&ms / 1000 < 1){
              return hour + ':' + minute + ':' + second + "." + ms
            }
            return hour + ':' + minute + ':' + second
          }
          if (second < 3600 && second >= 60) {
            hour = '00'
            minute = parseInt((second / 60).toString())
            /* eslint-disable */
            minute >= 10 ? minute : minute = '0' + minute
            second = second % 60
            second >= 10 ? second : second = '0' + second
            /* eslint-enable */
            if (ms != 0 &&ms / 1000 < 1){
              return hour + ':' + minute + ':' + second + "." + ms
            }
            return hour + ':' + minute + ':' + second
          }
          if (second < 60 && second >= 1) {
            hour = '00';
            minute = '00';
            second = parseInt(second);
            /* eslint-disable */
            second >= 10 ? second : second = '0' + second
            /* eslint-enable */
            if (ms != 0 &&ms / 1000 < 1){
              return hour + ':' + minute + ':' + second + "." + ms
            }
            return hour + ':' + minute + ':' + second;
          }
          if (second < 1) {
            let minsecond = second * 1000
            hour = '00'
            minute = '00'
            second = '00'
            /* eslint-enable */
            if (minsecond == 0){
              return hour + ':' + minute + ':' + second
            }
            return hour + ':' + minute + ':' + second + "." + minsecond
          }
        }
      }
    }
</script>

<style scoped>

</style>
