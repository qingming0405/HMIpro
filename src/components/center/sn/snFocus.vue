<template>
  <div>
    <div
      v-for="(param,key) in focus"
      v-show="param.isShow"
      class="general-view-content"
    >
      <div
        v-for="(item, index) in param.mac"
        class="view-content"
        :ref="'view_content'+ key + '_' + index"
        @click="choosemac(index)"
        @dblclick="jumpToModel(item)"
        :class="{
          active: item.chooseflag,
          'general-view-citem': item.status == 1,
          'general-view-citem-warning': item.status == 2,
          'general-view-citem-alarm': item.status == 3,
          'general-view-citem-abnormal': item.status == '自检异常',
          'general-view-citem-offline': item.status == 0,
        }"
      >
        <div class="general-view-ctitle">
          <div class="general-view-f2">{{ item.name }}</div>
          <i
            @click="collection(item)"
            v-if="!item.isCollect"
            class="iconfont icon-shoucang"
          ></i>
          <i
            @click="collection(item)"
            v-else
            class="iconfont icon-mianxing_fuzhi_huaban1"
          ></i>
        </div>
        <div class="general-content">
          <div class="picture">
            <img
              :src="item.imgSrc"
              alt=""
            />
          </div>
          <div class="status">
            <!-- 设备状态 -->
            <p>{{$t('SnFocus.macStatus')}}</p>
            <p>{{ statusValue[item.status] }}</p>
            <!-- 设备实时转速 -->
            <p>{{$t('SnFocus.macSpeed')}}</p>
            <p>{{ item.speed }}</p>
            <!-- 报警预警测点显示 -->
            <!-- <div class="status-content">
              <div
                class="status-content-item"
                :title="$t('SnFocus.Alarm')+$t('SnFocus.PosNumber')+'：'+item.alarmPositions"
              >
                <div>{{$t('SnFocus.Alarm')}}</div>
                <p> {{item.alarmPositions}} </p>
              </div>
              <div
                class="status-content-item"
                :title="$t('SnFocus.warn')+$t('SnFocus.PosNumber')+'：'+item.warningPositions"
              >
                <div>{{$t('SnFocus.warn')}}</div>
                <p>
                  {{item.warningPositions}}
                </p>
              </div>
              <div
                class="status-content-item"
                :title="$t('SnFocus.Normal')+$t('SnFocus.PosNumber')+'：'+item.normalityPositions"
              >
                <div>{{$t('SnFocus.Normal')}}</div>
                <p>
                  {{item.normalityPositions}}
                </p>
              </div>
            </div> -->
                      
          </div>
            <div class="text">
            <!-- 设备近况 -->
            <p>{{$t('SnFocus.macCon')}}</p>
            <!-- 诊断时间 -->
            <p>{{$t('SnFocus.diagTime')}}&nbsp;&nbsp;{{ item.time }}</p>
            <!-- 诊断结果 -->
            <p>{{$t('SnFocus.diagResult')}}&nbsp;&nbsp;{{ item.desc }}</p>
          </div>
          <div></div>
        </div>
      </div>
      <div
        class="none-data-box select-none"
        v-show="param.noData.isShow"
      >
        <div>{{ param.noData.text }}</div>
      </div>
    </div>
  </div>
</template>

<script>
import snFocus from 'common/general/sn/snFocus.js'
export default {
  name: 'snFocus',
  mixins: [snFocus],
  data() {
    return {}
  },
}
</script>

<style scoped lang="scss">
.general-view-content {
  position: relative;
  padding-top: 50px;
  height: 100%;
  display: grid;
  justify-content: center;
  grid-template-columns: repeat(auto-fill, 24.5%);
  @media screen and (max-width: 1366px) {
    grid-template-columns: repeat(auto-fill, 33%);
  }
  grid-template-rows: repeat(auto-fill, 300px);
  grid-row-gap: 30px;
  grid-column-gap: 4px;

  flex-wrap: wrap;
  overflow-y: auto;
  .view-content {
    width: 380px;
    height: 300px;
    background: #0c1858;
    border-radius: 4px;
  }
  .general-view-ctitle {
    height: 37px;
    display: flex;
    flex-direction: row;
    align-items: center;
    // justify-content: center;
    border-bottom: 1px solid #4674d6;
    padding: 0px 30px;
    .general-view-f2 {
      width: 90%;
      height: 16px;
      font-size: 16px;
      font-family: Source Han Sans CN;
      font-weight: 400;
      color: #f2f6fa;
      display: flex;
      justify-content: flex-start;
    }
    .iconfont {
      width: 10%;
      display: flex;
      justify-content: flex-end;
      height: 19px;
      font-size: 19px;
      cursor: pointer;
    }
  }
  .general-content {
    height: calc(100% - 37px);
    width: 100%;
    padding: 20px 20px;
    display: grid;
    grid-template-columns: 60% 38%;
    grid-template-rows: 60% 40%;
    grid-row-gap: 10px;
    grid-column-gap: 10px;
    .picture {
      height: 100%;
      width: 100%;
      background: #122067;
      position: relative;
      border: 1px solid #172a70;
      img {
        max-height: 100%;
        max-width: 100%;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
      }
      background-position: center;
      background-repeat: no-repeat;
    }
    .status {
      align-self: center;
      font-size: 13px;
      font-weight: 400;
      color: #f1f6f9;
      p {
        margin: 5px auto;
      }
      .status-content {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        .status-content-item {
          display: flex;
          flex-direction: column;
          justify-content: center;
          margin-right: 2px;
        }
        .pos-state {
          width: 8px !important;
          height: 8px !important;
          border-radius: 50%;
          background: #009944;
          float: left;
          margin: 10px 2px;
        }
        .state-offline {
          background: #3771b0;
        }
        .state-warn {
          background: #ffa101;
        }
        .state-alarm {
          background: #f80000;
        }
      }
    }
    .text {
      padding-top: 5px;
      padding-left: 5px;
      height: 101%;
      background: #122067;
      grid-column-start: span 2;
      overflow-y: auto;
      border-radius: 4px;
      border: 1px solid #172a70;
      p {
        font-family: Source Han Sans CN;
        &:nth-child(1) {
          font-size: 16px;
          color: #f1f6f9;
          font-weight: 400;
          margin-bottom: 10px;
        }
        &:nth-child(2),
        &:nth-child(3) {
          font-size: 14px;
          color: #f1f6f9;
          font-weight: 400;
          margin-left: 10px;
        }
      }
    }
  }

  //绿色正常
  .general-view-citem {
    border: 1px solid rgba(0, 154, 68, 0.9);
    box-shadow: 0px 0px 20px 1px rgba(0, 154, 68, 0.9) inset;
  }
  //橙色预警
  .general-view-citem-warning {
    border: 1px solid #ffa202;
    box-shadow: rgba(255, 162, 2, 0.9) 0px 0px 20px 1px inset;
  }
  //蓝色自检查异常
  .general-view-citem-abnormal {
    border: 1px solid #4276f6;
    box-shadow: rgba(65, 119, 254, 0.9) 0px 0px 20px 1px inset;
  }
  //红色报警
  .general-view-citem-alarm {
    border: 1px solid #f80000;
    box-shadow: rgba(248, 0, 0, 0.9) 0px 0px 20px 1px inset;
  }
  //离线
  .general-view-citem-offline {
    border: 1px solid #4674d6;
    box-shadow: rgba(6, 70, 168, 0.72) 0px 0px 20px 0px inset;
  }
}
</style>
