<!--
 * @Author: daidai
 * @Date: 2022-03-01 15:27:58
 * @LastEditors: Please set LastEditors
 * @LastEditTime: 2022-05-07 11:24:14
 * @FilePath: \web-pc\src\pages\big-screen\view\indexs\right-center.vue
-->
<template>
  <div v-if="pageflag" class="right_center_wrap beautify-scroll-def" :class="{ 'overflow-y-auto': !sbtxSwiperFlag }">
    <component :is="components" :data="list" :class-option="defaultOption">
      <ul class="right_center ">
        <li class="right_center_item" v-for="(item, i) in list" :key="i">
          <div class="flexm">
            <div class="inner_right">
              <div class="flex">
                <div class="info">
                  <span class="labels ">设备ID：</span>
                  <span class="contents zhuyao"> {{ item.gatewayno }}</span>
                </div>
              </div>
              <div class="flex">
                <div class="info">
                  <span class="labels ">设备ID：</span>
                  <span class="contents zhuyao"> {{ item.gatewayno }}</span>
                </div>
              </div>

              <div class="flex">
                <div class="info">
                  <span class="labels"> 地址：</span>
                  <span class="contents ciyao" style="font-size:12px"> {{ item.provinceName }}/{{ item.cityName }}/{{ item.countyName }}</span>
                </div>

              </div>
              <div class="flex">

                <div class="info">
                  <span class="labels">报警内容：</span>
                  <span class="contents ciyao" :class="{ warning: item.alertdetail }"> {{ item.alertdetail || '无'
                    }}</span>
                </div>
              </div>
            </div>
            <div style="background-color: #00b1d6; width: 10rem;height: 6rem;">

            </div>
          </div>
        </li>
      </ul>
    </component>
  </div>
  <Reacquire v-else @onclick="getData" style="line-height:200px" />

</template>

<script>
import { currentGET } from 'api/modules'
import vueSeamlessScroll from 'vue-seamless-scroll'  // vue2引入方式
import Kong from '../../components/kong.vue'
export default {
  components: { vueSeamlessScroll, Kong },

  data() {
    return {
      list: [],
      pageflag: true,
      defaultOption: {
        ...this.$store.state.setting.defaultOption,
        limitMoveNum: 2,
        singleHeight: 220,
        step:0,
      }

    };
  },
  computed: {
    sbtxSwiperFlag() {
      let ssyjSwiper = this.$store.state.setting.ssyjSwiper
      if (ssyjSwiper) {
        this.components = vueSeamlessScroll
      } else {
        this.components = Kong
      }
      return ssyjSwiper
    }
  },
  created() {
    this.getData()
  },

  mounted() { },
  methods: {
    getData() {
      this.pageflag = true
      // this.pageflag =false
      currentGET('big5', { limitNum: 50 }).then(res => {
        console.log('实时预警', res);
        if (res.success) {
          this.list = res.data.list
          let timer = setTimeout(() => {
            clearTimeout(timer)
            this.defaultOption.step=this.$store.state.setting.defaultOption.step
          }, this.$store.state.setting.defaultOption.waitTime);
        } else {
          this.pageflag = false
          this.$Message.warning(res.msg)
        }
      })
    },

  },
};
</script>
<style lang='scss' scoped>
.flexm {
  display: flex;
  //margin-right: 20px;
}
.right_center {
  width: 100%;
  height: 100%;

  .right_center_item {
    //display: flex;
    align-items: center;
    justify-content: center;
    height: auto;
    padding: 10px;
    font-size: 14px;
    color: #fff;


    .right_img {
      flex: 3;
    }

    .doudong {
      //  vertical-align:middle;
      overflow: hidden;
      -webkit-backface-visibility: hidden;
      -moz-backface-visibility: hidden;
      -ms-backface-visibility: hidden;
      backface-visibility: hidden;
    }

    .inner_right {
      flex: 2;
      position: relative;
      height: 100%;
      //修改左边文字的整体宽度
      width: 300px;
      line-height: 1.5;
    }

    .info {
      margin-right: 10px;
      margin-left: 10px;
      display: flex;
      align-items: center;

      .labels {
        flex-shrink: 0;
        font-size: 12px;
        color: rgba(255, 255, 255, 0.6);
      }

      .zhuyao {
        color: $primary-color;
        font-size: 15px;
      }

      .ciyao {
        color: rgba(255, 255, 255, 0.8);
      }

      .warning {
        color: #E6A23C;
        font-size: 15px;
      }
    }

  }
}

.right_center_wrap {
  overflow: hidden;
  width: 100%;
  height: 200px;
  margin-top: 25px;
}

.overflow-y-auto {
  overflow-y: auto;
}
</style>
