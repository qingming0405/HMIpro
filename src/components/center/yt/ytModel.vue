<template>
  <div>
    <div
      class="wpower-modelitems"
      v-for="(param,key) in ytModel"
      v-if="param.isShow"
    >
      <div class="wpower-modelitems-title">
        <div
          v-for="(item, index) in param.monitorePos"
          class="wpower-monitore-postion"
          @click="choosemonitore(index)"
          :class="{ 'choose-item': item.isChoose }"
        >
          {{ item.name }}
        </div>
      </div>
      <div class="wpower-modelitems-content">
        <keep-alive :include="keepAlive">
          <router-view class="ytModel" />
        </keep-alive>
      </div>
    </div>
  </div>
</template>

<script>
import { cloneObj } from 'utils/utils.js'
export default {
  name: 'ytModel',
  data() {
    return {
      ytModel: {},
      currentKey: '',
    }
  },
  created() {
    this.$store.commit('set_keepAlive', {
      method: 'add',
      keepAlive: 'ytModel',
    })
  },
  computed: {
    keepAlive() {
      return this.$store.getters.keepAlive
    },
  },
  methods: {
    openChartList(key, type) {
      if (typeof key !== 'string') return
      let mac, title
      for (let i in this.ytModel) {
        this.ytModel[i].isShow = false
      }
      /*  总貌图设备模型多开增加了一个type强行变为0的设计，在这边重新判断type */
      const keyArr = Object.keys(this.ytModel)
      if (type === 0 || type === 1 || type === 4) {
        if (keyArr.indexOf(key) === -1) {
          type = 0
          title = cloneObj(this.$store.state.chooseTitle.generalTitle)
        } else {
          type = 1
        }
      }
      if (type === 0) {
        mac = cloneObj(this.$store.state.checkMsg.mac)
        this.currentKey = key
      } else if (type === 1 || type === 4) {
        mac = this.ytModel[key].mac
        this.currentKey = key
      }
      switch (type) {
        case 0 /* 打开图表 */:
          this.$set(this.ytModel, key, {
            mac,
            title, //左上角标题
            monitorePos: [
              {
                name: this.$t('YtModel.macModel'), //设备模型
                router: 'ytMechineModel',
                isChoose: true,
              },
              {
                name: this.$t('YtModel.punchCard'), //电流卡片
                router: 'ytPunchCard',
                isChoose: false,
              },
              {
                name: this.$t('YtModel.analysis'), //沉没度分析
                router: 'ytAnalyse',
                isChoose: false,
              },

              // {
              //   name: '油液参数',
              //   router: 'snOil',
              //   isChoose: false,
              // },
              // {
              //   name: '倾角',
              //   router: 'snTowerdrum',
              //   isChoose: false,
              // },
            ],
            isShow: true,
          })
          this.toModel()
          break
        case 1 /* 切换图表 */:
          this.ytModel[key].isShow = true
          this.toModel()
          break
        case 2 /* 关闭图表 */:
          this.$delete(this.ytModel, key)
          break
        case 4 /* 从其他图表跳转已存在图标 */:
          this.ytModel[key].isShow = true
          this.toModel()
          break
      }
    },
    choosemonitore(index) {
      const param = this.ytModel[this.currentKey]
      for (let i = 0; i < param.monitorePos.length; i++) {
        if (index === i) {
          param.monitorePos[i].isChoose = true
          const mac = param.mac
          const store = this.$store
          let macArray = store.state.mac[mac.t_id]
          let choosemac = store.state.checkMsg.mac
          let choosetree = cloneObj(store.state.checkMsg.tree)
          // 使用线程防止组织测点为重选就进行了跳转
          new Promise((resolve, reject) => {
            if (choosemac !== null && choosemac.machine_id == mac.mac_id) {
              if (choosemac.t_id != choosetree.t_id) {
                let treeArray = store.state.tree
                treeArray.forEach((tree) => {
                  if (choosemac.t_id == tree.t_id) {
                    store.commit('getCheckMsg', {
                      msg: cloneObj(tree),
                      type: 'tree',
                    })
                    store.commit('getCheckMsg', {
                      msg: cloneObj(choosemac),
                      type: 'mac',
                    })
                    resolve('成功')
                  }
                })
              }
              resolve('成功')
            } else {
              for (let i = 0; i < macArray.length; i++) {
                if (macArray[i].mac_id == mac.mac_id) {
                  /* 设置当前的机组 */
                  if (macArray[i].t_id != choosetree.t_id) {
                    let treeArray = store.state.tree
                    treeArray.forEach((tree) => {
                      if (macArray[i].t_id == tree.t_id) {
                        store.commit('getCheckMsg', {
                          msg: cloneObj(tree),
                          type: 'tree',
                        })
                      }
                    })
                  }
                  store.commit('getCheckMsg', {
                    msg: macArray[i],
                    type: 'mac',
                  })
                  resolve('成功')
                  break
                }
              }
            }
          }).then(() => {
            this.$store.commit('setGeneralModel', {
              key: this.currentKey,
              router: param.monitorePos[i].router,
            })
            // 设备模型跳转设备模型使用同一个key值调用getPath方法
            let params = {
              key: this.currentKey,
              val: this.$t('YtModel.macModel'), //'设备模型',
              name: param.monitorePos[i].router,
              icon: 'icon-shijingsanwei-',
            }
            this.$bus.$emit('getPath', params)
          })
        } else {
          param.monitorePos[i].isChoose = false
        }
      }
    },
    toModel() {
      const param = this.ytModel[this.currentKey]
      let router = 'snMechineModel'
      for (let i = 0, l = param.monitorePos.length; i < l; i++) {
        if (param.monitorePos[i].isChoose == true) {
          router = param.monitorePos[i].router
        }
      }
      // 默认推到水泥设备模型中
      let params = {
        key: this.currentKey,
        val: this.$t('YtModel.macModel'), //'设备模型',
        name: router,
        icon: 'icon-shijingsanwei-',
      }
      this.$bus.$emit('getPath', params)
      this.$store.commit('setGeneralModel', {
        key: this.currentKey,
        router: router,
      })
    },
  },
  watch: {
    '$store.state.ytModelMsg': {
      handler(value) {
        if (value.length !== 0) {
          while (value.length) {
            let item = value.shift()
            this.openChartList(item.key, item.state)
          }
        }
      },
      deep: true,
      immediate: true,
    },
  },
}
</script>
<style scoped lang="scss">
@import '@/assets/style/css/model.scss';
</style>
<style scoped lang="scss">
.ytModel {
  height: 100%;
  width: 100%;
}
</style>
