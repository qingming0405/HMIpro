<script>
import com from "./wave3dCommon";
export default {
  mixins: [com],
  data() {
    return {
     lineColor:"#fff",
     colorList: [
        '#FF623B',
        '#F1B232',
        '#EAD07B',
        '#E9EC46',
        '#BDE970',
        '#74DEA8',
        '#3093B5',
        '#A9BFE2',
        '#A25DB5',
      ] /* 备选颜色 */,
    };
  }
};
</script>

<style scoped lang="scss">
.wave3d {
  position: relative;
  .title {
    position: absolute;
  }
  .histogram {
    position: absolute;
    height: 200px;
    width: 323px;
    font-size: 10px;
    z-index: 100;
  }
  .my-spectrum-3d-chart {
    height: calc(100% - 49px);
    .spectrum-3d-chart {
      width: 100%;
      height: 100%;
    }
  }
  .search-3d-data {
    height: 49px;
    line-height: 49px;
    text-align: center;
    position: absolute;
    bottom: 0;
    width: 100%;
    button {
      height: 30px;
      width: 100px;
      margin-right: 20px;
    }
  }
  .clickRightMenu {
    width: 110px;
    background-color: #152f69;
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
}
</style>
