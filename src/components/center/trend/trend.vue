<!-- 趋势图 -->
<script>
// import 《组件名称》 from '《组件路径》';

import common from "./trendCommon.vue";
export default {
  // import引入的组件需要注入到对象中才能使用
  mixins: [common],
  data(){
    return{
      colorList: [
        '#000000',
        '#CCCC00',
        '#993366',
        '#00CC99',
        '#996666',
        '#6666FF',
        '#999699',
        '#CC9933'
      ] /* 备选颜色 */,
      savePicBackgroundColor:'#fff'
    }
  }
};
</script>
<style scoped lang="scss">
.my-trend-chart {
  overflow: hidden;
  .my-trend {
    position: relative;
    height: 100%;
    width: 100%;
    .trend-chart {
      width: 100%;
      height: calc(100% - 50px);
      display: grid;
      grid-template-columns: 130px calc(100% - 130px);
      & + div {
        margin-top: 1px;
      }
      .trend-pos-list {
        height: 100%;
        overflow: scroll;
        &::-webkit-scrollbar {
          display: none;
        }
      }
      .trend-left-title {
        .trend-eigenvalue {
          padding-left: 5px;
        }
      }
    }
    .search-data {
      height: 49px;
      line-height: 49px;
      width: 100%;
      text-align: center;
      position: absolute;
      bottom: 0;
      .addPosTrend {
        position: absolute;
        left: 15px;
        top: 10px;
      }
      button {
        height: 30px;
        width: 100px;
        margin: 0px 10px;
      }

      .no-real-date {
        background-color: #ccc;
      }
    }
  }
}
.clickRightMenu {
  width: 110px;
  background-color: #fff;
  font-size: 12px;
  position: absolute;
  text-align: left;
  padding: 2px 0;
  border: 1px solid #ccc;
  display: block;
  z-index: 100;
}
.clickRightMenu li {
  list-style: none;
  line-height: 20px;
  padding-left: 15px;
}
.clickRightMenu li:hover {
  background-color: #dcdcdc;
  cursor: pointer;
}
.iconfont {
  font-size: 14px;
  cursor: pointer;
  margin-right: 10px;
}
</style>
