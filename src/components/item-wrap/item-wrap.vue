<!--
 * @Author: daidai
 * @Date: 2022-03-01 09:16:22
 * @LastEditors: Please set LastEditors
 * @LastEditTime: 2022-05-07 11:06:23
 * @FilePath: \web-pc\src\pages\big-screen\components\item-wrap\item-wrap.vue
-->
<template>
  <div class="lr_titles">
      <slot></slot>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  props: {
    title: {
      type: String,
      default: () => "",
    },
  },
  created() {},

  mounted() {},
  methods: {},
};
</script>
<style lang='scss' scoped>
$item-title-height: 38px;
$item_title_content-height: calc(100% - 38px);

.lr_titles {
  box-sizing: border-box;

  ::v-deep .border-box-content {
    box-sizing: border-box;
    padding: 6px 6px 0px;
  }


  .item_title_content {
    height: $item_title_content-height;
  }

  .item_title_content_def {
    width: 100%;
    height: 100%;
  }
}
</style>
